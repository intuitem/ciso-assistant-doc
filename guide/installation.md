---
description: Docker Compose or Helm for Kubernetes
icon: desktop-arrow-down
---

# Installation



## Docker compose



Make sure to have Docker and Docker Compose installed on your system.

* clone the repo:&#x20;

`git clone https://github.com/intuitem/ciso-assistant-community.git`

* run the preparation script and follow the instructions:

`./docker-compose.sh`



you can also find other variants for different setups as a starting point for your specific needs.

## Helm chart



Make sure to have Helm binary installed and switch to your cluster context.



1. add the helm repository

`helm repo add intuitem https://intuitem.github.io/ca-helm-chart/`

2. get the default values&#x20;

`helm show values intuitem/ciso-assistant > my-values.yaml`

3. check and adjust them to your needs, specifically the `frontendOrigin` parameter&#x20;
4. create a namesapce for your deployment&#x20;

`kubectl create ns ciso-assistant`

5. install&#x20;

`helm install my-octopus intuitem/ciso-assistant -f my-values.yaml -n ciso-assistant`



{% hint style="info" %}
This setup is based on the fact that Caddy will handle the TLS on your behalf. In case you're experiencing ssl related issues, you might want to patch your ingress-nginx-controller to activate the `enable-ssl-passthrough` flag.
{% endhint %}
