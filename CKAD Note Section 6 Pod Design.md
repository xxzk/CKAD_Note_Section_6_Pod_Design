# CKAD Note Section 6 Pod Design

<br>

---

## 79. Labels, Selectors and Annotations

<br>


```yaml
apiVersion: v1
kind: Pod
metadata:
  name: label-demo
  labels:
    environment: production
    app: nginx
spec:
  containers:
  - name: nginx
    image: nginx:1.14.2
    ports:
    - containerPort: 80
```


我們可以在 kubernetes object 上貼上標籤 (labels) 用於分類。使用 `kubectl get pod --selector <key>=<value>` 可以篩選。