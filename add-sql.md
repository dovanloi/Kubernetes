# Connect sql
Mô tả: Để kết nối cơ sở dữ liệu từ Cloud SQL tới kube ta cần tạo ra 1 dịch vụ song song trên po, rồi chạy proxy để kết nối với database trên cloud. Trong pod ta chỉ cần kết nối tới 127.0.0.1 để kết nối cơ sở dữ liệu.

B1: Tạo cơ sở dữ liệu
Các thông tin cần lưu lại:
- Connection name
- DB, User, Password
B2: Tạo proxy 
```sh
wget https://dl.google.com/cloudsql/cloud_sql_proxy.linux.amd64 -O cloud_sql_proxy
```
```sh
chmod +x cloud_sql_proxy
```
B3: Tạo tài khoản và cấp quyền truy vấn csdl
```sh
gcloud iam service-accounts create proxy-user --display-name "proxy-user"
```
Xem danh sách mail đc tạo:
```sh
gcloud iam service-accounts list
```
Thêm quyền:
```sh
gcloud projects add-iam-policy-binding [PROJECT_ID] --member serviceAccount:[SERVICE_ACCOUNT_EMAIL] --role roles/cloudsql.client
```

```sh
gcloud iam service-accounts keys create key.json --iam-account [SERVICE_ACCOUNT_EMAIL]
```
Test kết nối: 
```sh 
./cloud_sql_proxy -instances=[INSTANCE_CONNECTION_NAME]=tcp:5432 -credential_file=key.json &
```
Tắt kế nối:
```sh
killall cloud_sql_proxy
```
Bước 4: Tạo se
```sh
kubectl create secret generic cloudsql-instance-credentials --from-file=credentials.json=../key.json
```
```sh
kubectl create secret generic cloudsql-db-credentials --from-literal=username=[DB_USER] --from-literal=password=[DB_PASS] --from-literal=dbname=[DB_NAME]
```
Bước 5: Tạo file kết nối
```sh
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  name: server-edu
  labels:
    app: server-edu
spec:
  template:
    metadata:
      labels:
        app: server-edu
    spec:
      # This section describes the containers that make up the deployment
      containers:
        - name: server-edu
          image: gcr.io/test03-283406/server-edu:1.0.0
          imagePullPolicy: Always
          ports:
            - containerPort: 8082
          # Set env variables used for Postgres Connection
          env:
            - name: DB_USER
              valueFrom:
                secretKeyRef:
                  name: cloudsql-db-credentials
                  key: username
            - name: DB_PASS
              valueFrom:
                secretKeyRef:
                  name: cloudsql-db-credentials
                  key: password
            - name: DB_NAME
              valueFrom:
                secretKeyRef:
                  name: cloudsql-db-credentials
                  key: dbname
        # Change <INSTANCE_CONNECTION_NAME> here to include your GCP
        # project, the region of your Cloud SQL instance and the name
        # of your Cloud SQL instance. The format is $PROJECT:$REGION:$INSTANCE
        - name: cloudsql-proxy
          image: gcr.io/cloudsql-docker/gce-proxy:1.11
          command: ["/cloud_sql_proxy",
                    "-instances=test03-283406:asia-southeast1:dataedu=tcp:5432",
                    "-credential_file=/secrets/cloudsql/credentials.json"]
          volumeMounts:
            - name: my-secrets-volume
              mountPath: /secrets/cloudsql
              readOnly: true
      volumes:
        - name: my-secrets-volume
          secret:
            secretName: cloudsql-instance-credentials
```

