# Connect sql

B1: Tạo cơ sở dữ liệu
Các thông tin cần lưu lại:
- Connection name
- DB, User, Password
B2: Tạo proxy 
``sh
wget https://dl.google.com/cloudsql/cloud_sql_proxy.linux.amd64 -O cloud_sql_proxy
``
``sh
chmod +x cloud_sql_proxy
``
B3: Tạo tài khoản
``sh
gcloud iam service-accounts create proxy-user --display-name "proxy-user"
``
Xem danh sách mail đc tạo:
``sh
gcloud iam service-accounts list
``
Thêm quyền:
``sh
gcloud projects add-iam-policy-binding [PROJECT_ID] --member serviceAccount:[SERVICE_ACCOUNT_EMAIL] --role roles/cloudsql.client
``

``sh
gcloud iam service-accounts keys create key.json --iam-account [SERVICE_ACCOUNT_EMAIL]
``

Kết nối:
``sh 
./cloud_sql_proxy -instances=[INSTANCE_CONNECTION_NAME]=tcp:5432 -credential_file=key.json &
``
