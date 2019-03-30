###  登陆用户名 username 登陆service registry.cn-zhangjiakou.aliyuncs.com
` sudo docker login --username=xxxxx@aliyun.com registry.cn-zhangjiakou.aliyuncs.com`

### pod yaml 设置
``` yaml
      containers:
      - image: registry.cn-zhangjiakou.aliyuncs.com/xxxx
        imagePullPolicy: Always
        name: tbapi
        ports:
        - containerPort: 8001
          name: 8001tcp02
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
      imagePullSecrets: # <<--------------------私有镜像登陆
      - name: regsecret # <<--------------------私有镜像名称
      restartPolicy: Always
      schedulerName: default-scheduler
      securityContext: {}
      terminationGracePeriodSeconds: 30
```
