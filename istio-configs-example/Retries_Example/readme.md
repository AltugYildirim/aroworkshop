

# Helloworld service (istio retries example)

We will distribute load with retries within the scope of istio-example

Along with the timeout, there can be retry as well.
Because when the service dropped after the timeout is should retry sending the request so it won’t be dropped permanently.

This sample includes three versions(v1,v3) of a simple helloworld service that returns its project version when called. It can be used as a test service when experimenting with version routing.

This service is also used to demonstrate  retries deployments working in conjunction. See retries deployments using Istio. Code repo for image [github\dotnet-example](https://github.com/OktaySavdi/dotnet-example)

## Start the helloworld service

The following commands assume you have [automatic sidecar injection](https://istio.io/docs/setup/additional-setup/sidecar-injection/#automatic-sidecar-injection) enabled in your cluster. If not, you'll need to modify them to include [manual sidecar injection](https://istio.io/docs/setup/additional-setup/sidecar-injection/#manual-sidecar-injection).

I used that command
```ruby
kubectl label namespace helloworld istio-injection=enabled
```
Deploying version v1, v3 or both:
```ruby
kubectl create -f Retries_Example/
```
![https://github.com/OktaySavdi/istio-examples/tree/master/Retries_Example](https://user-images.githubusercontent.com/3519706/76602764-b053e800-651c-11ea-95e1-543b5141a7c2.png)


## Generate load
```ruby
istio_ingress="$(kubectl get svc istio-ingressgateway --namespace istio-system --output 'jsonpath={.spec.ports[?(@.port==80)].nodePort}')"
    
while true; do curl -s "http://[NodeIP]:[IstioIngressNodeport]/istio"; sleep 0.5; echo -e '\n'; done
    
while true; do curl -s "http://10.10.10.10:$istio_ingress/istio"; sleep 0.5; echo -e '\n'; done 
```
## Monitoring and Control

Control istio Config

![https://github.com/OktaySavdi/istio-examples/tree/master/Retries_Example](https://user-images.githubusercontent.com/3519706/76602929-fc9f2800-651c-11ea-9402-3c215462a1cd.png)


**Control Graph**

![https://github.com/OktaySavdi/istio-examples/tree/master/Retries_Example](https://user-images.githubusercontent.com/3519706/76602846-d37e9780-651c-11ea-983f-40ee92eff74b.png)

**Overview Console**

![https://github.com/OktaySavdi/istio-examples/tree/master/Retries_Example](https://user-images.githubusercontent.com/3519706/73649574-1f722d00-4691-11ea-8f8f-f9ce8f21b4fa.png)

**Inbound Metrics**

![https://github.com/OktaySavdi/istio-examples/tree/master/Retries_Example](https://user-images.githubusercontent.com/3519706/73649621-3d3f9200-4691-11ea-9a4f-d12737d9c59a.png)

**CLI on Server**

![https://github.com/OktaySavdi/istio-examples/tree/master/Retries_Example](https://user-images.githubusercontent.com/3519706/76602971-1b052380-651d-11ea-997a-2118cd9704a3.png)
