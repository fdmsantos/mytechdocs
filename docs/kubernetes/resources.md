# Kubernetes Resources

## Workloads

### Pods

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: myapp-prod
  labels:
    app: myapp
    type: front-end
spec:
  schedulerName: my-scheduler # To Use Custom Scheduler
  securityContext:
    runAsUser: 1000
  priorityClassName: high-priority-nonpreempting
  runningClassName: gvisor
  containers:
    - name: nginx-container
      image: nginx
      ports:
        - containerPort: 8080
      command: ["sleep"]
      args: ["10"]
      env:
        - name: APP_COLOR
          value: pink
        - name: APP_COLOR_2 # Inject ConfigMap as Single Environment Variable
          valueFrom:
            configMapKeyRef:
              name: app-config
              key: APP_COLOR
        - name: APP_COLOR_3 # Inject Secret as Single Environment Variable
          valueFrom:
            secretKeyRef:
              name: app-secret
              key: APP_COLOR
      envFrom:
        - configMapRef: # Inject ConfigMap as Environment
            name: app-config
        - secretRef: # Inject Secret as Environment
            name: app-secret
      volumeMounts:
        - mountPath: /opt
          name: data-volume
        - mountPath: "/var/www/html"
          name: web
          readOnly: false
      resources:
        requests:
          memory: "1Gi"
          cpu: "1"
        limits:
          memory: "2Gi"
          cpu: "2"
      securityContext:
          runAsUser: 1000
          readOnlyRootFilesystem: true
          capabilities:
            add: ["MAC_ADMIN", "SYS_TIME"]
      readinessProbe:
        httpGet: # OR tcpSocket, exec
          port: 8080
          path: /api/ready
        initialDelaySeconds: 10
        periodSeconds: 5
        failureThreshold: 8
      livenessProbe:
        exec:
          command:
            - cat
            - /app/is_healthy
    - name: log-agent
      image: log-agent
  initContainers:
    - name: init-myservice
      image: busybox
      command: ['sh', '-c', 'git clone <some-repository-that-will-be-used-by-application> ; done;']
  imagePullSecrets: # To Configure Private Registry
    - name: regcred
  serviceAccountName: dashboard-sa
  automountServiceAccountToken: false # Deprecated. Not Necessary in latest versions
  volumes:
    - name: app-config-volume # Inject Config Map as a file in a Volume
      configMap:
        name: app-config
    - name: app-secret-volume # Inject Secret as a file in a Volume
      secret:
        secretName: app-secret
    - name: data-volume
      hostPath: # That Works fine on Single Node
        path: /data
        type: Directory
    - name: web
      persistentVolumeClaim:
        claimName: myclaim
  tolerations: # Scheduling
  - key: "app"
    operator: "Equal"
    value: "blue"
    effect: "NoSchedule"
  nodeSelector: # Scheduling
    size: Large
  nodeName: node02 # Scheduling: Manual Scheduling
  affinity: # Scheduling
    nodeAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
        nodeSelectorTerms:
          - matchExpressions:
              - key: size
                operator: In
                values:
                  - Large
                  - Medium
    podAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
        - labelSelector:
            matchExpressions:
              - key: security
                operator: In
                values:
                  - S1
          topologyKey: topology.kubernetes.io/zone
    podAntiAffinity:
      preferredDuringSchedulingIgnoredDuringExecution:
        - weight: 100
          podAffinityTerm:
            labelSelector:
              matchExpressions:
                - key: security
                  operator: In
                  values:
                    - S2
            topologyKey: topology.kubernetes.io/zone
```

### Deployments

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: myapp-deployment
  labels:
    app: myapp
    type: front-end
spec:
 template:
   <POD YAML Template>
 replicas: 3
 selector:
   matchLabels:
    type: front-end
 strategy:
  type: RollingUpdate # Or Recreate
  rollingUpdate:
    maxUnavailable: 1
```

#### Commands

```shell
kubectl rollout status deployment/myapp-deployment
kubectl rollout history deployment/myapp-deployment
kubectl rollout undo deployment/myapp-deployment
```

### DaemonSets

```yaml
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: monitoring-daemon
  labels:
    app: nginx
spec:
  selector:
    matchLabels:
      app: monitoring-agent
  template:
    <POD YAML Template>
```

### Jobs

```yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: random-error-job
spec:
  completions: 3
  template:
    spec:
      containers:
        - name: random-error
          image: kodekloud/random-error
      restartPolicy: Never
```

### CronJobs

```yaml
apiVersion: batch/v1beta1
kind: CronJob
metadata:
  name: reporting-cron-job
spec:
  schedule: "*/1 * * * *"
  jobTemplate:
    spec:
      completions: 3
      parallelism: 3
      template:
        spec:
          containers:
            - name: reporting-tool
              image: reporting-tool
          restartPolicy: Never
```

### StatefulSets

```yaml
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: mysql
  labels:
    app: mysql
spec:
  template:
    <POD YAML Template>
  replicas: 3
  selector:
    matchLabels:
      type: front-end
  serviceName: mysql-h
  volumeClaimTemplates:
    - metadata:
        name: data-volume
      spec:
        accessModes:
          - ReadWriteOnce
        resources:
          requests:
            storage: 500Mi
```

### Replica Sets

```yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
 name: myapp-replicaset
 labels:
   app: myapp
   type: front-end
spec:
 template:
   <POD YAML Template>
 replicas: 3
 selector:
   matchLabels:
     type: front-end
```

## Storage

### Persistent Volumes

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: pv-vol1
spec:
  persistentVolumeReclaimPolicy: Retain # Supports Delete, Recycle and Retain
  accessModes: [ "ReadWriteOnce" ] # Supports ReadOnlyMany, ReadWriteOnce and ReadWriteMany
  capacity:
   storage: 1Gi
  local:
   path: /tmp/data
  nodeAffinity: # Must SET when using Local Volumes
    required:
      nodeSelectorTerms:
        - matchExpressions:
            - key: kubernetes.io/hostname
              operator: In
              values:
                - example-node
```

### Persistent Volumes Claims

```yaml 
kind: PersistentVolumeClaim
apiVersion: v1
metadata:
  name: myclaim
spec:
  accessModes: [ "ReadWriteOnce" ]
  storageClassName: google-storage
  resources:
   requests:
     storage: 1Gi
```

### Storage Class

```yaml
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
   name: google-storage
provisioner: kubernetes.io/gce-pd
volumeBindingMode: WaitForFirstConsumer

parameters:
  type: pd-standard
  replication-type: none

```

## Networking 

### Services

![Services](images/services.png){ loading=lazy }

#### NodePort

```yaml
apiVersion: v1
kind: Service
metadata:
  name: myapp-service
spec:
  type: NodePort
  ports:
    - targetPort: 80
      port: 80
      nodePort: 30008
  selector:
    app: myapp
    type: front-end
```

#### Cluster IP

```yaml
apiVersion: v1
kind: Service
metadata:
 name: back-end
spec:
 type: ClusterIP
 ports:
 - targetPort: 80
   port: 80
 selector:
   app: myapp
   type: back-end
```

#### Headless

```yaml
apiVersion: v1
kind: Service
metadata:
 name: mysql-h
spec:
 clusterIP: None
 ports:
 - port: 80
 selector:
   app: myapp
   type: back-end
```

### Ingress

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: ingress-wear-watch
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
    nginx.ingress.kubernetes.io/ssl-redirect: False
spec:
  rules:
    - http:
        paths:
          - path: /wear
            backend:
              service:
                name: wear-service
                port: 
                  number: 80
          - path: /watch
            backend:
              service:
                name: watch-service
                port:
                  number: 80
    - host: watch.my-online-store.com
      http:
        paths:
          - backend:
              service:
               name: watch-service
               port:
                 number: 80
```

### Network Policy

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
 name: db-policy
spec:
  podSelector:
    matchLabels:
      role: db
  policyTypes:
  - Ingress
  - Egress
  ingress:
  - from:
    - podSelector:
        matchLabels:
          name: api-pod
      namespaceSelector: # Its necessary label namespace
        matchLabels:
          name: prod
    - ipBlock: # To External access
        cidr: 192.168.5.10/32 
    ports:
    - protocol: TCP
      port: 3306
  egress:
    - to:
        - ipBlock:
            cidr: 192.168.5.10/32
      ports:
        - protocol: TCP
          port: 80
```

## Security

### RBAC

#### Role

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  name: developer
rules:
- apiGroups: [""] # "" indicates the core API group
  resources: ["pods"]
  verbs: ["get", "list", "update", "delete", "create"]
  resourceNames: ["blue", "orange"] # Optional
- apiGroups: [""]
  resources: ["ConfigMap"]
  verbs: ["create"]
```

#### Role Binding

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: devuser-developer-binding
subjects:
- kind: User # Or ServiceAccount
  name: dev-user # "name" is case sensitive
  apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: Role
  name: developer
  apiGroup: rbac.authorization.k8s.io
```

#### Cluster Role

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  name: cluster-administrator
rules:
- apiGroups: [""] # "" indicates the core API group
  resources: ["nodes"]
  verbs: ["get", "list", "delete", "create"]
```

#### Cluster Role Binding

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: cluster-admin-role-binding
subjects:
- kind: User
  name: cluster-admin
  apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: ClusterRole
  name: cluster-administrator
  apiGroup: rbac.authorization.k8s.io
```

### Service Accounts

```shell
kubectl create token <sa-account-name>
kubectl create token <sa-account-name> # To create token
```

### Admission Webhooks

```yaml
apiVersion: admissionregistration.k8s.io/v1
kind: ValidatingWebhookConfiguration
metadata:
  name: "pod-policy.example.com"
webhooks:
  - name: "pod-policy.example.com"
    clientConfig:
      service:
        name: "webhook-service"
        namespace: "webhook-namespace"
      caBundle: "..."
    rules:
      - apiGroups: [""]
        apiVersions: ["v1"]
        operations: ["CREATE"]
        resources: ["pods"]
        scope: "Namespaced"
```

## Scheduling

### Priority Classes

```yaml
apiVersion: scheduling.k8s.io/v1
kind: PriorityClass
metadata:
  name: high-priority-nonpreempting
value: 1000000
preemptionPolicy: Never # PreemptLowerPriority
globalDefault: false
description: "This priority class will not cause other pods to be preempted."
```

## Namespaces

### Namespace

```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: dev
```

### Resource Quota

```yaml
apiVersion: v1
kind: ResourceQuota
metadata:
  name: compute-quota
  namespace: dev
spec:
  hard:
    pods: "10"
    requests.cpu: "4"
    requests.memory: 5Gi
    limits.cpu: "10"
    limits.memory: 10Gi
```

## Extending

### Custom Resource Definition

* Example New Resource:

```yaml
apiVersion: flights.com/v1
kind: FlightTicket
metadata:
  name: my-flight-ticket
spec:
  from: Mumbai
  to: London
  number: 2
```

* CRD to Resource FlightTicket

```yaml
apiVersion: apiextensions.k8s.io/v1
kind: CustomResourceDefinition
metadata:
  name: flighttickets.flights.com
spec:
  scope: Namespaced
  group: flights.com
  names:
    kind: FlightTicket
    singular: flightticket
    plural: flighttickets
    shortNames:
      - ft
  versions:
    - name: v1
      served: true
      storage: true
      schema:
        openAPIV3Schema:
          type: object
          properties:
            spec:
              type: object
              properties:
                from:
                  type: string
                to:
                  type: string
                number:
                  type: integer
                  minimum: 1
                  maximum: 10
```