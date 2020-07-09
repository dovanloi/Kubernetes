# Wordpress 
Tạo ổ đĩa pvc để lưu trữ wp:

```sh
kubectl apply -f $WORKING_DIR/wordpress-volumeclaim.yaml
```

Có thể mất tới mười giây để cung cấp PV được hỗ trợ bởi Persistent Disk và liên kết nó với PVC của bạn. Bạn có thể kiểm tra trạng thái bằng lệnh sau:

```sh
kubectl get persistentvolumeclaim
```

Tạo một SQL SQL cho phiên bản MySQL
Trong Cloud Shell, tạo một thể hiện có tên mysql-wordpress-instance:
```sh
INSTANCE_NAME=mysql-wordpress-instance
gcloud sql instances create $INSTANCE_NAME
``
Thêm tên kết nối thể hiện dưới dạng biến môi trường:
```sh
export INSTANCE_CONNECTION_NAME=$(gcloud sql instances describe $INSTANCE_NAME \
    --format='value(connectionName)')
    ```
