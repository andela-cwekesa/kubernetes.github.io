---
title: Configure the aggregation layer
assignees:
- lavalamp
- cheftako
- chenopis
---

{% capture overview %}

Configuring the [aggregation layer](/docs/concepts/api-extension/apiserver-aggregation/) allows the Kubernetes apiserver to be extended with additional APIs, which are not part of the core Kubernetes APIs. 

{% endcapture %}

{% capture prerequisites %}

{% include task-tutorial-prereqs.md %}

**Note:** There are a few setup requirements for getting the aggregation layer working in your environment to support mutual TLS auth between the proxy and extension apiservers. Kubernetes and the kube-apiserver have multiple CAs, so make sure that the proxy is signed by the aggregation layer CA and not by something else, like the master CA.

{% endcapture %}

{% capture steps %}

## Enable apiserver flags

Enable the aggregation layer via the following kube-apiserver flags. They may have already been taken care of by your provider.

    --requestheader-client-ca-file=<path to aggregator CA cert>
    --requestheader-allowed-names=aggregator
    --requestheader-extra-headers-prefix=X-Remote-Extra-
    --requestheader-group-headers=X-Remote-Group
    --requestheader-username-headers=X-Remote-User
    --proxy-client-cert-file=<path to aggregator proxy cert>
    --proxy-client-key-file=<path to aggregator proxy key>

The [Kubernetes Architectural Roadmap](https://docs.google.com/a/google.com/document/d/1XkjVm4bOeiVkj-Xt1LgoGiqWsBfNozJ51dyI-ljzt1o/edit?usp=sharing) recommends not running kube-proxy on the master. If you follow this recommendation, then you must make sure that the system is enabled with the following apiserver flag. Again, this may have already been taken care of by your provider.

    --enable-aggregator-routing=true

{% endcapture %}

{% capture whatsnext %}

* [Setup an extension api-server](/docs/tasks/access-kubernetes-api/setup-extension-api-server/) to work with the aggregation layer.
* For a high level overview, see [Extending the Kubernetes API with the aggregation layer](/docs/concepts/api-extension/apiserver-aggregation/).
* Learn how to [Extend the Kubernetes API Using Custom Resource Definitions](/docs/tasks/access-kubernetes-api/extend-api-custom-resource-definitions/).

{% endcapture %}

{% include templates/task.md %}

