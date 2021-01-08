
### k8s 端生成账户并绑定可访问角色和可访问资源

```
apiVersion: v1
kind: ServiceAccount
metadata:
  name: prometheus
  namespace: kube-system
---
apiVersion: rbac.authorization.k8s.io/v1beta1
kind: ClusterRole
metadata:
  name: prometheus
rules:
- apiGroups: [""]
  resources:
  - nodes
  - nodes/metrics
  - services
  - endpoints
  - pods
  verbs:
   - get
   - list
   - watch
- apiGroups: [""]
  resources:
  - configmaps
  verbs: ["get"]

- nonResourceURLs:
  - "/metrics"
  verbs:
    - get

---
apiVersion: rbac.authorization.k8s.io/v1beta1
kind: ClusterRoleBinding
metadata:
  name: prometheus
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: prometheus
subjects:
- kind: ServiceAccount
  name: prometheus
  namespace: kube-system
```




---
#
#
<meta http-equiv="refresh" content="5">