# Kubernete 사용전 셋팅(Ubuntu)
* root계정에서 실행(#)
* alias 
```
echo "alias k='kubectl'">> ~/.bashrc
echo 'complete -o default -F __start_kubectl k' >>~/.bashrc
. ~/.bashrc
```
# Short names
| Short name    |Full name                  |
|---------------|---------------------------|
| po            |pods                       |
| rs            |replicasets                |
| svc           |services                   |
| ns            |namespaces                 |
| no            |nodes                      |
| ep            |endpoints                  |
| ds            |daemonsets                 |
| deploy        |deployments                |
| -             | -                         |
| cm            |configmaps                 |
| cs            |componentstatuses          |
| csr           |certificatesigningrequests |
| ev            |events                     |
| hpa           |horizontalpodautoscalers   |
| ing           |ingresses                  |
| limits        |limitranges                |
| pdb           |poddisruptionbudgets       |
| psp           |podsecuritypolicies        |
| pv            |persistentvolumes          |
| pvc           |persistentvolumeclaims     |
| quota         |resourcequotas             |
| rc            |replicationcontrollers     |
| sa            |serviceaccounts            |

* cf
```
kubectl get nodes
   ↓
k get no
```

## 정보 얻기
```
kubectl cluster-info
kubectl get nodes
k get  -h
kubectl get pods
kubectl get pods -o wide
kubectl get pods --all-namespaces

kubectl version
kubectl get deployments
kubectl get services
```

## deploy
```
kubectl create deployment --image=nginx --port=80 nginx
k get po
k get deploy
k delete deploy/nginx
k get po
k get deploy

```

## pods
```
k get po
{결과에서 pod명 복사}
k exec nginx-77b4fdf86c-tjcxv -- pwd
k exec -it nginx-77b4fdf86c-tjcxv -- bash
```

## SerViCe
```
kubectl create deployment --image=nginx --port=80 nginx
kubectl expose deploy nginx --type="NodePort" --port 80
k get svc
k describe svc/nginx
curl 10.233.115.197 # vm01
```
## Replication Set
```
kubectl scale deployment nginx --replicas=4
k get deploy
k get po
k get rs
k get po -o wide|grep vm01|wc -l
kubectl scale deployment nginx --replicas=4
```
## daemonsets
cat <<EOF> d.yaml
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: httpd
spec:
  selector:
    matchLabels:
      app: httpd
  template:
    metadata:
      labels:
        app: httpd
    spec:
      containers:
      - name: httpd
        image: httpd
        ports:
        - containerPort: 80
          hostPort: 6379
EOF

kubectl create -f d.yaml

k edit ds/httpd

k get ds/httpd -o yaml > d2.yaml

k delete -f d2.yaml
k create -f d2.yaml
```

# 현재 deploy를 yaml로 만들어 보세요.
* svc,rs 참고
* script history
```
k create deployment --image=nginx --port=80 nginx
k expose deploy nginx --type="NodePort" --port 80
k scale deployment nginx --replicas=2
``` 
* 답
```
k get -o yaml deploy/nginx > d-nginx.yaml
k get -o yaml svc/nginx > s-nginx.yaml
k get rs
#k get rs/nginx-55f598f8d -o yaml > rs_nginx.yaml
k get deploy
k delete deploy/nginx
k get rs
k get svc
k delete svc/nginx
# rs는 자동으로 지워졌음.

```
* 결론 rs,pod와 같이 자동으로 만들어진 객체는 yaml파일 생성할 필요 없음.



# Label
```
k get po
k label po/nginx-55f598f8d-8t45p app2=v1 # 두번하면 실패하는 것으로 나오나 이미 있어서 "not labeled"메세지 뜨는 것임.
k get po -l app2=v1
k get po --show-labels
k label po/nginx-55f598f8d-8t45p app2=v2  --overwrite
k label po/nginx-55f598f8d-8t45p app2-
```

# cf
## jq (https://jsonpath.com/ 활용할 것)
* jq(Json Query)
```
apt install -y jq
k get deploy/nginx -o json |jq 
k get deploy/nginx -o json |jq .metadata.name
# k get po|awk '{print $1}'
k get po -o json|jq .items[].metadata.name
```
