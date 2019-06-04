### kubernetes efk deploy

1. `elasticsearch`
```
$ kubectl create ns logging

$ kubectl create -f elasticsearch.yaml -n logging

deployment.extensions/elasticsearch created
service/elasticsearch created

$ kubectl get pods -n logging

NAME                          READY   STATUS    RESTARTS   AGE
elasticsearch-bb9f879-d9kmg   1/1     Running   0          75s


$ kubectl get service -n logging

NAME            TYPE       CLUSTER-IP       EXTERNAL-IP   PORT(S)          AGE
elasticsearch   NodePort   10.102.149.212   <none>        9200:30588/TCP   86s
```
2. `kibana`
```
$ kubectl create -f kubernetes/kibana.yaml -n logging

$ kubectl get pods -n logging

NAME                          READY   STATUS    RESTARTS   AGE
elasticsearch-bb9f879-d9kmg   1/1     Running   0          17m
kibana-7f6686674c-mjlb2       1/1     Running   0          60s


$ kubectl get service -n logging

NAME            TYPE       CLUSTER-IP       EXTERNAL-IP   PORT(S)          AGE
elasticsearch   NodePort   10.102.149.212   <none>        9200:30588/TCP   17m
kibana          NodePort   10.106.226.34    <none>        5601:30004/TCP   74s
```
3. `fluentd`
```
$ kubectl create -f fluentd_rbac.yaml

$ kubectl create -f fluentd_daemonset.yaml

NAME                                    READY   STATUS             RESTARTS   AGE
coredns-576cbf47c7-gtvdj                1/1     Running            11         132d
coredns-576cbf47c7-p7sx5                1/1     Running            10         132d
etcd-master                             1/1     Running            10         132d
kube-apiserver-master                   1/1     Running            29         132d
kube-controller-manager-master          1/1     Running            96         132d
kube-flannel-ds-amd64-76khk             1/1     Running            8          132d
kube-proxy-h8gm7                        1/1     Running            9          132d
kube-scheduler-master                   1/1     Running            101        132d
kubernetes-dashboard-56768b7988-bgzl6   1/1     Running            10         132d
metrics-server-5f78f74857-vq8db         1/1     Running            3          25h
node-exporter-6tzrm                     1/1     Running            2          19h
fluentd-6zsdx                           1/1     Running            0          100s

$ kubectl logs fluentd-6zsdx -n kube-system

Connection opened to Elasticsearch cluster =>
  {:host=>"elasticsearch.logging", :port=>9200, :scheme=>"http"}
```
