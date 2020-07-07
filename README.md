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

Lấy secret:
```sh
kubectl get secret -n [name]
```

