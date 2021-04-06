Install directions for Sysdig on K8s, with SaaS backend, as of 2-21

Main docs for vanilla K8s: https://docs.sysdig.com/en/steps-for-kubernetes--vanilla-.html

Source of yaml files: https://github.com/draios/sysdig-cloud-scripts/tree/master/agent_deploy/kubernetes

DIRECTIONS

1. kubectl create ns sysdig-agent

2. FILL IN: kubectl -n sysdig-agent create secret generic sysdig-agent --from-literal=access-key=<your sysdig access key>

3. kubectl apply -n sysdig-agent -f https://raw.githubusercontent.com/draios/sysdig-cloud-scripts/master/agent_deploy/kubernetes/sysdig-agent-clusterrole.yaml

4. kubectl create serviceaccount sysdig-agent -n sysdig-agent

5. kubectl create clusterrolebinding sysdig-agent --clusterrole=sysdig-agent --serviceaccount=sysdig-agent:sysdig-agent

6. curl -sSLO https://raw.githubusercontent.com/draios/sysdig-cloud-scripts/master/agent_deploy/kubernetes/sysdig-agent-configmap.yaml

7. vi sysdig-agent-configmap.yaml  and add the correct collector URL per: https://docs.sysdig.com/en/saas-regions-and-ip-ranges.html

8. kubectl apply -n sysdig-agent -f sysdig-agent-configmap.yaml

7. kubectl apply -n sysdig-agent -f https://raw.githubusercontent.com/draios/sysdig-cloud-scripts/master/agent_deploy/kubernetes/sysdig-agent-daemonset-v2.yaml

8. curl -sSLO https://raw.githubusercontent.com/draios/sysdig-cloud-scripts/master/agent_deploy/kubernetes/sysdig-image-analyzer-configmap.yaml

9. vi sysdig-image-analyzer-configmap.yaml   and add the endpoint URL per: https://docs.sysdig.com/en/saas-regions-and-ip-ranges.html

10. kubectl apply -n sysdig-agent -f sysdig-image-analyzer-configmap.yaml

11: kubectl apply -n sysdig-agent -f https://raw.githubusercontent.com/draios/sysdig-cloud-scripts/master/agent_deploy/kubernetes/sysdig-image-analyzer-daemonset.yaml

UNINSTALL DIRECTIONS
1. kubectl delete ns sysdig-agent
2. kubectl delete clusterrole sysdig-agent
3. kubectl delete clusterrolebinding sysdig-cluster-role-binding
