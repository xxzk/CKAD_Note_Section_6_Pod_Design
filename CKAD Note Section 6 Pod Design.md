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

## 82. Rolling Updates & Rollbacks in Deployments

<br>

這邊主要介紹 rolling update strategy 是怎麼樣的概念，當我們部屬一個 deployment 就會觸發 rollout，可以透過 `kubectl rollout status deployment <name>` 查看 rollout 狀態。

<br>

![rollout_status_0](rollout_status_0.jpg)

▲ log 由上至下。 `kubectl rollout history` 可以查看歷史。

<br>

![rolling_update_strategy](rolling_update_strategy.jpg)

<br>

Kubernetes 預設使用 **rolling update** 策略 (strategy) 來更新 `pod`，比起 **recreate** 後者能夠確保服務不被中斷。\
`kubectl describe deployments.apps rollout-test` 能夠找到 `RollingUpdateStrategy:  25% max unavailable, 25% max surge`

<br>

![describe_deployment](describe_deployment.jpg)

<br>

### rolling update 


首先 `kubectl create deployment nginx-rollout --image=nginx:1.20.1 --replicas=5`

<br>

![nginx_rollout_0](nginx_rollout_0.jpg)

▲ `kubectl rollout status` 顯示完成

<br>

![nginx_rollout_1](nginx_rollout_1.jpg)

▲ `Events` 顯示 scale up replica set **<span style='color:red'>75c585fb66</span>** to 5

<br>

接著我們升級 `nginx -> 1.21`

<br>

![nginx_rollout_2](nginx_rollout_2.jpg)

▲ 我們以 <span style='color:blue'>藍色</span>代表新的 replica set\
scale up replica set **<span style='color:blue'>59b5b5c6bd</span>** to 2\
scale down replica set **<span style='color:red'>75c585fb66</span>** to 4\
scale up replica set **<span style='color:blue'>59b5b5c6bd</span>** to 3\
scale down replica set **<span style='color:red'>75c585fb66</span>** to 3\
scale up replica set **<span style='color:blue'>59b5b5c6bd</span>** to 4\
scale down replica set **<span style='color:red'>75c585fb66</span>** to 2\
scale up replica set **<span style='color:blue'>59b5b5c6bd</span>** to 5 (達標)\
scale down replica set **<span style='color:red'>75c585fb66</span>** to 1\
(combined from similar events): Scaled down replica set <span style='color:red'>75c585fb66</span> to 0

<br>

![nginx_rollout_3](nginx_rollout_3.jpg)

▲ 示意圖

<br>

### rollback


反悔了，來個 rollback 時光倒轉一下吧! `kubectl rollout undo deployment nginx-rollout`

<br>

![rollback_0](rollback_0.jpg)

▲ 舊的 replicaset 又有東西囉~

<br>

![sum](sum.jpg)

▲ 總結

<br>

---

