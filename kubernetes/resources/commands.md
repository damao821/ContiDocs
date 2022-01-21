
## Common commands

### port forwarding
* ```shell
  kubectl port-forward --address 0.0.0.0 $SERVICE_OR_POD 8080:80 # local:pod
  ```
* 通过将认证令牌直接发送到 API 服务器，也可以避免运行 kubectl proxy 命令。 内部的证书机制能够为链接提供保护
  * ```shell
    # 指向内部 API 服务器的主机名
    APISERVER=https://kubernetes.default.svc
    
    # 服务账号令牌的路径
    SERVICEACCOUNT=/var/run/secrets/kubernetes.io/serviceaccount
    
    # 读取 Pod 的名字空间
    NAMESPACE=$(cat ${SERVICEACCOUNT}/namespace)
    
    # 读取服务账号的持有者令牌
    TOKEN=$(cat ${SERVICEACCOUNT}/token)
    
    # 引用内部证书机构（CA）
    CACERT=${SERVICEACCOUNT}/ca.crt
    
    # 使用令牌访问 API
    curl --cacert ${CACERT} --header "Authorization: Bearer ${TOKEN}" -X GET ${APISERVER}/api
    ```

### Empowerment note

```shell
cat <<EOF | kubectl apply -f -
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  name: pod-reader
  namespace: application
rules:
- apiGroups: [""] # "" 标明 core API 组
  resources: ["pods"]
  verbs: ["get", "watch", "list", "create"]
EOF

cat <<EOF | kubectl apply -f -
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: read-pods-rd
  namespace: application
subjects:
  - kind: ServiceAccount
    name: libk
    namespace: application
roleRef:
  kind: Role
  name: pod-reader
  apiGroup: "rbac.authorization.k8s.io"
EOF

cat <<EOF | kubectl apply -f -
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  name: conti-read
rules:
  - apiGroups: [""]
    resources: ["nodes", "namespaces", "configmaps", "endpoints", "persistentvolumes", "persistentvolumeclaims", "secrets", "services", "pods"]
    verbs: ["get", "list", "watch"]
  - apiGroups: ["apps"]
    resources: ["statefulsets", "replicasets", "deployments", "daemonsets"]
    verbs: ["get", "list", "watch"]
  - apiGroups: ["batch"]
    resources: ["cronjobs", "jobs"]
    verbs: ["get", "list", "watch"]
  - apiGroups: ["storage.k8s.io"]
    resources: ["storageclasses"]
    verbs: ["get", "list", "watch"]
EOF

cat <<EOF | kubectl apply -f -
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: conti-read-b
  namespace: application
subjects:
  - kind: ServiceAccount
    name: conti
    namespace: application
roleRef:
  kind: ClusterRole
  name: conti-read
  apiGroup: "rbac.authorization.k8s.io"
EOF
```