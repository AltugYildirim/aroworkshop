
# Helloworld service (istio roundrobin example)

We will distribute load with roundrobin within the scope of istio-example

This sample includes two versions(v1,v2) of a simple helloworld service that returns its project version when called. It can be used as a test service when experimenting with version routing.

This service is also used to demonstrate roundrobin deployments working in conjunction. See roundrobin deployments using Istio. Code repo for image [github\dotnet-example](https://github.com/OktaySavdi/dotnet-example)

## Start the helloworld service

The following commands assume you have [automatic sidecar injection](https://istio.io/docs/setup/additional-setup/sidecar-injection/#automatic-sidecar-injection) enabled in your cluster. If not, you'll need to modify them to include [manual sidecar injection](https://istio.io/docs/setup/additional-setup/sidecar-injection/#manual-sidecar-injection).

I used that command
```ruby
kubectl label namespace helloworld istio-injection=enabled
```
Deploying version v1, v2 or both:
```ruby
kubectl create -f RoundRobin_Example/
```  
![https://github.com/OktaySavdi/istio-examples/tree/master/RoundRobin_Example](https://user-images.githubusercontent.com/3519706/74125496-8ab68480-4be6-11ea-8839-a9855d951440.png)

## Generate load
```ruby
istio_ingress="$(kubectl get svc istio-ingressgateway --namespace istio-system --output 'jsonpath={.spec.ports[?(@.port==80)].nodePort}')"
    
while true; do curl -s "http://[NodeIP]:[IstioIngressNodeport]/istio"; sleep 0.5; echo -e '\n'; done
    
while true; do curl -s "http://10.10.10.10:$istio_ingress/istio"; sleep 0.5; echo -e '\n'; done 
```
## Monitoring and Control

Control istio Config

![https://github.com/OktaySavdi/istio-examples/tree/master/RoundRobin_Example](https://user-images.githubusercontent.com/3519706/73649385-b985a580-4690-11ea-8065-835446113ac8.png)

**Control Graph**

![https://github.com/OktaySavdi/istio-exampless/tree/master/RoundRobin_Example](https://user-images.githubusercontent.com/3519706/74131115-b0e32100-4bf4-11ea-8b00-d6dd60478f22.png)

**Overview Console**

![https://github.com/OktaySavdi/istio-examples/tree/master/RoundRobin_Example](https://user-images.githubusercontent.com/3519706/73649574-1f722d00-4691-11ea-8f8f-f9ce8f21b4fa.png)

**Inbound Metrics**

![https://github.com/OktaySavdi/istio-examples/tree/master/RoundRobin_Example](https://user-images.githubusercontent.com/3519706/73649621-3d3f9200-4691-11ea-9a4f-d12737d9c59a.png)

**CLI on Server**

![https://github.com/OktaySavdi/istio-examples/tree/master/RoundRobin_Example](https://user-images.githubusercontent.com/3519706/74030813-020acf00-49c1-11ea-9427-3d6ee1271eeb.png)
