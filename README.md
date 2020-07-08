# Kubernetes

Kiểm tra version: 
```sh
kubectl version --short
```
Kiểm tra xem có những máy nào: 
```sh
kubectl get node
```
Xem thông tin chi tiết của node:
```sh
kubectl describe node/[name-node]
```

## Pod
Mỗi Pod sẽ có 1 IP, dùng chung cho các container. Pod có thể chạy trên bất kì node nào.
Triển khai 1 file yaml:
```sh
kubectl apply -f [tên file cần triển khai]
```

Hiển thị thông tin các pod 
```sh
kubectl get po
```
Hiển thị thông tin các pod chi tiết hơn
```sh
kubectl get po -o wide
```
Hiển thị thông tin chi tiết 1 pod:
```sh
kubectl describe po/[tên pod]
```
Xem thông tin file cấu hình pod:
```sh
kubectl get po/[tên pod] -o yaml
```
Sửa thông tin file cấu hình pod:
```sh
kubectl edit po/[tên pod]
```
Xem logs của pod:
```sh
kubectl logs po/[tên pod]
```
Thi hành 1 lệnh container trong 1 pod:
```sh
kubectl exec [tên pod] [lệnh cần thực thi]
```
Truy cập vào container pod:
```sh
kubectl exec -it [tên pod] bash
```
Truy cập vào container pod có nhiều hơn 1 container:
```sh
kubectl exec -it [tên pod] -c [tên container] bash
```
Xóa 1 pod
```sh
kubectl delete po [tên pod]
```
hoặc xóa pod được tạo bằng file
```sh
kubectl delete -f [tên file]
```

## Proxi
```sh
kubectl proxi
```
## Deployment
Lấy thông tin các deploy:
```sh
kubectl get deploy 
```
Lấy thông tin sự kiện deploy:
```sh
kubectl rollout history deploy/[name deploy]
```

Lấy thông tin sự kiện deploy theo phiên bản:
```sh
kubectl rollout history deploy/[name deploy] --revision=[phiên bản]
```

Backup về phiển bản trước:
```sh
kubectl rollout undo deploy/[name deploy] --to-revision=[phiên bản]
```

Xóa deploy
```sh
kubectl delete deploy/[name deploy]
```

## Service
Lấy danh sách service:
```sh
kubectl get service
```
Xem chi tiết service:
```sh
kubectl get service/[tên service]
```
Xem chi tiết cấu hình service:
```sh
kubectl describe service/[tên service]
```
Trong đó Endpoints là IP của pod

## Secret
Danh sách secret:
```sh
kubectl get secret
```
Xem thông tin secret:
```sh
kubectl describe secret/[tên secret]
```
