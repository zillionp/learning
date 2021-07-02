## Prerequisites
- You must have [kubernetes](https://kubernetes.io/releases/download/) and [helm](https://helm.sh/docs/intro/install) installed.
- You can install [minikube](https://minikube.sigs.k8s.io/docs/start/) or [kind](https://kind.sigs.k8s.io/docs/user/quick-start/#installation) to setup local kubernetes cluster in you machine.

**Start minikube**
- `minikube start --vm-driver=virtualbox --nodes=4`

**Steps**
1. Create a namespace.
   - `kubectl create namespace <namespace_name>` 
2. Add monitor helm chart
   - `helm repo add monitor https://charts.helm.sh/stable` 
3. Update helm
   - `helm repo update`  
4. Install helm repo in your previously created namespace
   - `helm install prometheus monitor/prometheus-operator --namespace <namespace_name>`
5. Get all running pods in namespace
   - `kubectl get pods -n <namespace_name>` 
6. Port forward prometheus and prometheus operator
   - `kubectl port-forward -n <namespace_name> <pod_name> <desired_port>`
   - example: `kubectl port-forward -n prometheus prometheus-prometheus-prometheus-oper-prometheus-0 9090`
   - You will see prometheus dashboard running in http://localhost:9090
7. Port forward grafana (grafana pod name will be listed on running above get pods command)
   - `kubectl port-forward -n <namespace_name> <pod_name> <desired_port>`
   - example: `kubectl port-forward -n prometheus prometheus-grafana-7789c77d9d-p74g7 3000`
   - You will be able to view grafana login screen on address http://localhost:3000
8. Get grafana credentials
   - `kubectl get secret --namespace <namespace_name> prometheus-grafana -o <format>`
   - `kubectl get secret --namespace prometheus prometheus-grafana -o yaml`
9. Admin credentials will be base64 encoded.
   - `echo <admin_user> | base64 --decode`
   - `echo <admin_password> | base64 --decode` 
10. Use the above decoded credentials and explore grafana dashboard.