- Setup chứng chỉ

Bước 1: Khởi tạo sác dev, service chạy trên các NodePort.
VD 1 service: 
```sh
apiVersion: v1
kind: Service
metadata:
  name: server-upload
spec:
  type: NodePort
  selector:
    app: server-upload
  ports:
  - name: server-upload-port
    protocol: TCP
    port: 80
    targetPort: 8081
```

Kiếm tra danh sách các service:
```sh
kubectl get svc
```
Bước 2: Thiết lập Bộ điều khiển Ingress Kubernetes Nginx

```sh
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/nginx-0.26.1/deploy/static/mandatory.yaml
```

```sh
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/nginx-0.26.1/deploy/static/provider/cloud-generic.yaml
```

Kiểm tra:
```sh
apiVersion: networking.k8s.io/v1beta1
kind: Ingress
metadata:
  name: server-ingress
  annotations:
    kubernetes.io/ingress.class: "nginx"
    cert-manager.io/cluster-issuer: "letsencrypt-prod"
spec:
  tls:
    - hosts:
      - cdn.chanhme.com
      secretName: server-tls
  rules:
  - host: cdn.chanhme.com
    http:
      paths:
      - backend:
          serviceName: server-upload
          servicePort: 80
```

   
