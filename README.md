# Description
[zabbix-kubernetes-monitoring](https://github.com/SwarmKit/kubenetes-zabbix-monitor) is zabbix-agent script and template for zabbix server. It is used for Kubernetes monitoring by Zabbix. Easy to deploy and configure. Auto discovery of pods, deployments, services, etc.

# Installation
1. Copy [k8s-stats.py](https://github.com/SwarmKit/kubenetes-zabbix-monitor/master/k8s-stats.py) to /etc/zabbix/scripts/ and [userparameter_k8s.conf](https://raw.githubusercontent.com/SwarmKit/kubenetes-zabbix-monitor/main/userparameter_k8s.conf) to /etc/zabbix/zabbix_agentd.d/. Remind to give execute permission to the file: ``chmod +x k8s-stats.py``
2. Import Zabbix template ([k8s-zabbix-template.xml](https://raw.githubusercontent.com/SwarmKit/kubenetes-zabbix-monitor/main/k8s-zabbix-template.xml)) to Zabbix server
3. Create zabbix user in Kubernetes (can use [zabbix-user-example.yml](https://raw.githubusercontent.com/SwarmKit/kubenetes-zabbix-monitor/master/zabbix-user-example.yml)) and set it's token and API server url in [k8s-stats.py](https://raw.githubusercontent.com/SwarmKit/kubenetes-zabbix-monitor/master/k8s-stats.py).
4. Apply template to host

## How to create zabbix user in Kubernetes
```bash
kubectl create serviceaccount zabbix-admin -n kube-system
kubectl create clusterrolebinding zabbix-admin --clusterrole=cluster-admin --serviceaccount=kube-system:zabbix-admin

```

## How to retrieve TOKEN and API SERVER
1. **TOKEN**:
```bash
kubectl -n kube-system describe secrets $(kubectl -n kube-system get secret | grep zabbix-admin | awk '{print $1}')
```
2. **API SERVER**:
```bash
$ APISERVER=https://$(kubectl -n default get endpoints kubernetes --no-headers | awk '{ print $2 }')
$ echo $APISERVER
```
