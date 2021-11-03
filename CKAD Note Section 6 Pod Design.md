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


最後 Annotation 就是有點像註解的東西，給人看的 例如: build version, ~~on call 電話號碼~~ 之類的。label 可以幫助我們 select object，annotation 則否。

<br>

