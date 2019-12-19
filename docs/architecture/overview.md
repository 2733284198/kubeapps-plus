# Kubeapps Plus概述

本文档从高层次描述了Kubeapps Plus架构。

## 组件

### Kubeapps Plus dashboard

Kubeapps Plus的核心是集群内Kubernetes仪表板，可为您提供简单的浏览和单击体验，以安装和管理打包为Helm图表的Kubernetes应用程序。

此外，仪表板还与[Kubernetes服务目录](https://github.com/kubernetes-incubator/service-catalog)集成在一起，使您可以通过[Open Service Broker API](https：//github.com/openservicebrokerapi/servicebroker)。

仪表板使用Javascript编程语言编写，并使用React Javascript库进行开发。

### Tiller proxy

为了保护对Tiller的访问并允许仪表板联系Helm Tiller服务器，我们部署了代理来处理与Tiller的通信。 该代理的目标是验证发出请求的用户是否具有足够的权限来创建或删除作为正在部署或删除的特定图表一部分的所有资源。

该代理是用Go编写的。 在此[document](/cmd/tiller-proxy/README.md)中检查有关实现的更多详细信息。

### Apprepository CRD and Controller

Kubeapps Plus中的图表存储库通过名为“ apprepositories.kubeapps.com”的“ CustomResourceDefinition”进行管理。 添加到Kubeapps Plus的每个存储库都是一个类型为AppRepository的对象，而apprepository-controller将监视这些对象类型的更改以更新要部署的可用图表的列表。

### `chart-repo`

“ chart-repo”组件是一种工具，可扫描Helm图表存储库并在MongoDB数据库中填充图表元数据。 然后，由Chartsvc组件提供此元数据。 它是[Helm Monocular project](https://github.com/helm/monocular/tree/master/cmd/chart-repo)的一部分进行维护。

### `chartsvc`

“ chartsvc”组件是一个微服务，可创建API端点来访问在MongoDB数据库中填充的Helm图表存储库中图表的元数据。 它是[Helm Monocular project](https://github.com/helm/monocular/tree/master/cmd/chartsvc)的一部分进行维护。