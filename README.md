# Auth Helm
*Auth HELM Charts*

选择此部署方式必须先安装 [Helm](https://helm.sh)。  
请查看 Helm 的 [文档](https://helm.sh/docs) 获取更多信息。

当 Helm 安装完毕后，使用下面命令添加仓库：

    helm repo add auth https://dustlight-cn.github.io/auth-helm

若您已经添加仓库，执行命令 `helm repo update` 获取最新的包。
您可以通过命令 `helm search repo auth` 来查看他们的 charts。

## 安装 Auth Service
> 完整配置文件请查看 [charts/auth-service/values.yaml](charts/auth-service/values.yaml)

创建配置文件 values.yaml：
```yaml
replicaCount: 1

ingress:
  enabled: true
  className: "nginx"
  host: "api.dustlight.cn"
  tls:
    enabled: false
    crt: ""
    key: ""

smtp:
  enabled: false
  host: ""
  username: ""
  password: ""

sms:
  type: "" # tencent | alibaba
  tencent:
    secretKey: ""
    secretId: ""
    appId: ""
  alibaba:
    accessKeyId: ""
    secretAccessKey: ""
  sign: ""
  templates:
    signUp: ""
    changePhone: ""
    resetPassword: ""
```

安装：

    helm install -f values.yaml my-auth auth/auth-service

卸载：

    helm delete my-auth

## 安装 Auth UI
> 完整配置文件请查看 [charts/auth-ui/values.yaml](charts/auth-ui/values.yaml)

创建配置文件 values.yaml：
```yaml
image: "dustlightcn/auth-ui:1.1.1-alpha-1"

replicaCount: 1

service:
https: false
host: "auth-service-my-auth" # 后端服务名

ingress:
enabled: true
className: "nginx"
host: "accounts.dustlight.cn"
tls:
enabled: false
crt: ""
key: ""
```

安装：

    helm install -f values.yaml my-auth-ui auth/auth-ui

卸载：

    helm delete my-auth-ui
