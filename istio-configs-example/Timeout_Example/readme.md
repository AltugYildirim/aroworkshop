

# Helloworld service (istio timeout example)

We will distribute load with timeout within the scope of istio-example

The default timeout for HTTP requests is 15 seconds. This makes sure that the services won’t wait for a long time waiting for a response or the service will be failed during a defined time.

This sample includes three versions(v1,v2,v3) of a simple helloworld service that returns its project version when called. It can be used as a test service when experimenting with version routing.

This service is also used to demonstrate  timeout deployments working in conjunction. See timeout deployments using Istio. Code repo for image [github\dotnet-example](https://github.com/OktaySavdi/dotnet-example)

## Start the helloworld service

The following commands assume you have [automatic sidecar injection](https://istio.io/docs/setup/additional-setup/sidecar-injection/#automatic-sidecar-injection) enabled in your cluster. If not, you'll need to modify them to include [manual sidecar injection](https://istio.io/docs/setup/additional-setup/sidecar-injection/#manual-sidecar-injection).

I used that command
```ruby
kubectl label namespace helloworld istio-injection=enabled
```
Deploying version v1, v2, v3 or both:
```ruby
kubectl create -f Timeout_Example/
```
![https://github.com/OktaySavdi/istio-examples/tree/master/Timeout_Example](https://user-images.githubusercontent.com/3519706/74038548-78afc880-49d1-11ea-93df-bd08300e1009.png)


## Generate load
```ruby
istio_ingress="$(kubectl get svc istio-ingressgateway --namespace istio-system --output 'jsonpath={.spec.ports[?(@.port==80)].nodePort}')"
    
while true; do curl -s "http://[NodeIP]:[IstioIngressNodeport]/istio"; sleep 0.5; echo -e '\n'; done
    
while true; do curl -s "http://10.10.10.10:$istio_ingress/istio"; sleep 0.5; echo -e '\n'; done 
```
## Monitoring and Control

Control istio Config

![https://github.com/OktaySavdi/istio-examples/tree/master/Timeout_Example](https://user-images.githubusercontent.com/3519706/74128749-ee44b000-4bee-11ea-9d95-9449b7c0c60b.png)

**Control Graph**

![https://github.com/OktaySavdi/istio-examples/tree/master/Timeout_Example](https://user-images.githubusercontent.com/3519706/74131389-42529300-4bf5-11ea-9029-ba5ee766f85d.png)

**Overview Console**

![https://github.com/OktaySavdi/istio-examples/tree/master/Timeout_Example](https://user-images.githubusercontent.com/3519706/73649574-1f722d00-4691-11ea-8f8f-f9ce8f21b4fa.png)

**Inbound Metrics**

![https://github.com/OktaySavdi/istio-examples/tree/master/Timeout_Example](https://user-images.githubusercontent.com/3519706/73649621-3d3f9200-4691-11ea-9a4f-d12737d9c59a.png)

**CLI on Server**

![https://github.com/OktaySavdi/istio-examples/tree/master/Timeout_Example](https://user-images.githubusercontent.com/3519706/74102023-b4bd6780-4b50-11ea-83be-46282a3753cd.png)
