 - Create namespace for Istio Traffic 
`kubectl create namespace istio-system`
Follow STEP12....

 - *First we deploy 'Istio Traffic Management Policies'*
**in istio-manifests directory**: 
`kubectl apply -f ...` 
(Frontend then Redis then Worker). 

 - *Then we deploy the application*:
**in the root directory**: 
`kubectl apply -f ...` 
(redis, redis-service, frontend, worker-primary **?**, worker-service)

$ kubectl -n istio-system get svc istio-ingressgateway
curl 


STEP14

*We need to get IP addresses of pods, they'll be used by Istio-multi*

pilot
`export PILOT_POD_IP=$(kubectl -n istio-system get pod -l istio=pilot -o jsonpath='{.items[0].status.podIP}')`

mixer
`export POLICY_POD_IP=$(kubectl -n istio-system get pod -l istio-mixer-type=policy -o jsonpath='{.items[0].status.podIP}')`



`export TELEMETRY_POD_IP=$(kubectl -n istio-system get pod -l istio-mixer-type=telemetry -o jsonpath='{.items[0].status.podIP}')`

tatsd-prom-bridge
`export STATSD_POD_IP=$(kubectl -n istio-system get pod -l istio=statsd-prom-bridge -o jsonpath='{.items[0].status.podIP}')`

export ZIPKIN_POD_IP=$(kubectl -n istio-system get pod -l app=jaeger -o jsonpath='{range .items[*]}{.status.podIP}{end}')

*Then we create a helm template for istio-multi with on-prem addresses*

helm template istio-1.0.0/install/kubernetes/helm/istio-remote --namespace istio-system \
--name istio-remote \
--set global.remotePilotAddress=${PILOT_POD_IP} \
--set global.remotePolicyAddress=${POLICY_POD_IP} \
--set global.remoteTelemetryAddress=${TELEMETRY_POD_IP} \
--set global.proxy.envoyStatsd.enabled=true \
--set global.proxy.envoyStatsd.host=${STATSD_POD_IP} \
--set global.remoteZipkinAddress=${ZIPKIN_POD_IP} > istio-remote-burst.yaml

*Then we switch to burst cluster, create namespace and deploy istio on it. Then we inject label to enable istio on pods.*

*** then to permit communications between clusters we have to deploy kubeconfig to on-prem cluster ***

Cluster name 
Cluster server name
SECRET_NAME the name of the secret for the istio-multi service account
CA_DATA
Certificate Authority data 
TOKEN

This will make a new file called **burst-kubeconfig** in your current directory which can be used by the on-prem cluster to authenticate and manage the burst cluster