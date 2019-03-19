### mysql 安装说明
` 通过rancher部署安装`

` "DefaultConnection3": "server=ip;user=root;database=dbname;port=30306;password=password" `

- mysql:5.6
``` yaml

apiVersion: apps/v1beta2
kind: Deployment
metadata:
  annotations:
    deployment.kubernetes.io/revision: "1"
    field.cattle.io/creatorId: user-844ll
    field.cattle.io/publicEndpoints: '[{"port":30306,"protocol":"TCP","serviceName":"app:mysql","allNodes":true}]'
  creationTimestamp: "2019-03-19T11:59:44Z"
  generation: 3
  labels:
    cattle.io/creator: norman
    workload.user.cattle.io/workloadselector: deployment-app-mysql
  name: mysql
  namespace: app
  resourceVersion: "2005656"
  selfLink: /apis/apps/v1beta2/namespaces/app/deployments/mysql
  uid: 7c0db86c-4a3e-11e9-92d9-00163e02623f
spec:
  progressDeadlineSeconds: 600
  replicas: 1
  revisionHistoryLimit: 10
  selector:
    matchLabels:
      workload.user.cattle.io/workloadselector: deployment-app-mysql
  strategy:
    rollingUpdate:
      maxSurge: 1
      maxUnavailable: 0
    type: RollingUpdate
  template:
    metadata:
      annotations:
        cattle.io/timestamp: "2019-03-19T11:59:43Z"
        field.cattle.io/ports: '[[{"containerPort":3306,"dnsName":"mysql","kind":"ClusterIP","name":"3306tcp02","protocol":"TCP","sourcePort":0}]]'
      creationTimestamp: null
      labels:
        workload.user.cattle.io/workloadselector: deployment-app-mysql
    spec:
      containers:
      - env:
        - name: MYSQL_ROOT_PASSWORD
          value: password
        image: mysql:5.6
        imagePullPolicy: Always
        name: mysql
        ports:
        - containerPort: 3306
          name: 3306tcp02
          protocol: TCP
        resources: {}
        securityContext:
          allowPrivilegeEscalation: false
          capabilities: {}
          privileged: false
          procMount: Default
          readOnlyRootFilesystem: false
          runAsNonRoot: false
        stdin: true
        terminationMessagePath: /dev/termination-log
        terminationMessagePolicy: File
        tty: true
        volumeMounts:
        - mountPath: /var/lib/mysql
          name: vol1
      dnsPolicy: ClusterFirst
      restartPolicy: Always
      schedulerName: default-scheduler
      securityContext: {}
      terminationGracePeriodSeconds: 30
      volumes:
      - hostPath:
          path: /srv/mysql
          type: DirectoryOrCreate
        name: vol1
status:
  availableReplicas: 1
  conditions:
  - lastTransitionTime: "2019-03-19T12:00:08Z"
    lastUpdateTime: "2019-03-19T12:00:08Z"
    message: Deployment has minimum availability.
    reason: MinimumReplicasAvailable
    status: "True"
    type: Available
  - lastTransitionTime: "2019-03-19T11:59:44Z"
    lastUpdateTime: "2019-03-19T12:00:08Z"
    message: ReplicaSet "mysql-5f89478884" has successfully progressed.
    reason: NewReplicaSetAvailable
    status: "True"
    type: Progressing
  observedGeneration: 3
  readyReplicas: 1
  replicas: 1
  updatedReplicas: 1


```

### 服务发现

` 注意 主机端口映射 用nodeport  设置 30306`

``` yaml
apiVersion: v1
kind: Service
metadata:
  annotations:
    field.cattle.io/ipAddresses: "null"
    field.cattle.io/publicEndpoints: '[{"port":30306,"protocol":"TCP","serviceName":"app:mysql","allNodes":true}]'
    field.cattle.io/targetDnsRecordIds: "null"
    field.cattle.io/targetWorkloadIds: "null"
    workload.cattle.io/targetWorkloadIdNoop: "true"
    workload.cattle.io/workloadPortBased: "true"
  creationTimestamp: "2019-03-19T11:59:44Z"
  labels:
    cattle.io/creator: norman
  name: mysql
  namespace: app
  ownerReferences:
  - apiVersion: apps/v1beta2
    controller: true
    kind: deployment
    name: mysql
    uid: 7c0db86c-4a3e-11e9-92d9-00163e02623f
  resourceVersion: "2005653"
  selfLink: /api/v1/namespaces/app/services/mysql
  uid: 7c188d91-4a3e-11e9-92d9-00163e02623f
spec:
  clusterIP: 10.233.42.248
  externalTrafficPolicy: Cluster
  ports:
  - name: 3306tcp02
    nodePort: 30306
    port: 3306
    protocol: TCP
    targetPort: 3306
  selector:
    workload.user.cattle.io/workloadselector: deployment-app-mysql
  sessionAffinity: None
  type: NodePort # <<<<<<<<<<注意<<<<<<<<<<<<<<<<<<<<
status:
  loadBalancer: {}

```
### phpmyadmin安装说明(安装完成后在phomyadmin里面修改密码)

```yaml
apiVersion: apps/v1beta2
kind: Deployment
metadata:
  annotations:
    deployment.kubernetes.io/revision: "2"
    field.cattle.io/creatorId: user-844ll
    field.cattle.io/publicEndpoints: '[{"addresses":[""],"port":443,"protocol":"HTTPS","serviceName":"app:phpmyadmin","ingressName":"app:phpmyadmin","hostname":"mysql.hyx123.cn","allNodes":false}]'
  creationTimestamp: "2019-03-19T12:25:48Z"
  generation: 6
  labels:
    cattle.io/creator: norman
    workload.user.cattle.io/workloadselector: deployment-app-phpmyadmin
  name: phpmyadmin
  namespace: app
  resourceVersion: "2010331"
  selfLink: /apis/apps/v1beta2/namespaces/app/deployments/phpmyadmin
  uid: 206e9f3b-4a42-11e9-92d9-00163e02623f
spec:
  progressDeadlineSeconds: 600
  replicas: 1
  revisionHistoryLimit: 10
  selector:
    matchLabels:
      workload.user.cattle.io/workloadselector: deployment-app-phpmyadmin
  strategy:
    rollingUpdate:
      maxSurge: 1
      maxUnavailable: 0
    type: RollingUpdate
  template:
    metadata:
      annotations:
        cattle.io/timestamp: "2019-03-19T12:35:13Z"
        field.cattle.io/ports: '[[{"containerPort":80,"dnsName":"phpmyadmin","kind":"ClusterIP","name":"80tcp2","protocol":"TCP"},{"containerPort":443,"dnsName":"phpmyadmin","kind":"ClusterIP","name":"443tcp2","protocol":"TCP"}]]'
      creationTimestamp: null
      labels:
        workload.user.cattle.io/workloadselector: deployment-app-phpmyadmin
    spec:
      containers:
      - env:
        - name: PMA_HOST
          value: mysql
        image: phpmyadmin/phpmyadmin
        imagePullPolicy: Always
        name: phpmyadmin
        ports:
        - containerPort: 80
          name: 80tcp2
          protocol: TCP
        - containerPort: 443
          name: 443tcp2
          protocol: TCP
        resources: {}
        securityContext:
          allowPrivilegeEscalation: false
          capabilities: {}
          privileged: false
          procMount: Default
          readOnlyRootFilesystem: false
          runAsNonRoot: false
        stdin: true
        terminationMessagePath: /dev/termination-log
        terminationMessagePolicy: File
        tty: true
      dnsPolicy: ClusterFirst
      restartPolicy: Always
      schedulerName: default-scheduler
      securityContext: {}
      terminationGracePeriodSeconds: 30
status:
  availableReplicas: 1
  conditions:
  - lastTransitionTime: "2019-03-19T12:26:13Z"
    lastUpdateTime: "2019-03-19T12:26:13Z"
    message: Deployment has minimum availability.
    reason: MinimumReplicasAvailable
    status: "True"
    type: Available
  - lastTransitionTime: "2019-03-19T12:25:48Z"
    lastUpdateTime: "2019-03-19T12:35:19Z"
    message: ReplicaSet "phpmyadmin-77cf949ccb" has successfully progressed.
    reason: NewReplicaSetAvailable
    status: "True"
    type: Progressing
  observedGeneration: 6
  readyReplicas: 1
  replicas: 1
  updatedReplicas: 1

```
### 注意环境变量 mysql DNS or host ip
```  - env:
        - name: PMA_HOST
          value: mysql
        image: phpmyadmin/phpmyadmin
        
```
        
        
