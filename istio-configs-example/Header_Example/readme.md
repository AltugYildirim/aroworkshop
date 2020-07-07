
# Helloworld service (istio  header example)

We will distribute load with header within the scope of istio-example

This sample includes two versions(v1,v2) of a simple helloworld service that returns its project version when called. It can be used as a test service when experimenting with version routing.

This service is also used to demonstrate  header deployments working in conjunction. See  header deployments using Istio. Code repo for image [github\dotnet-example](https://github.com/OktaySavdi/dotnet-example)

## Start the helloworld service

The following commands assume you have [automatic sidecar injection](https://istio.io/docs/setup/additional-setup/sidecar-injection/#automatic-sidecar-injection) enabled in your cluster. If not, you'll need to modify them to include [manual sidecar injection](https://istio.io/docs/setup/additional-setup/sidecar-injection/#manual-sidecar-injection).

I used that command

    kubectl label namespace helloworld istio-injection=enabled

Deploying version v1, v2, or both:

    kubectl create -f Header_Example/

![https://github.com/OktaySavdi/istio-examples/tree/master/Header_Example](https://user-images.githubusercontent.com/3519706/74125496-8ab68480-4be6-11ea-8839-a9855d951440.png)
## Generate load
    istio_ingress="$(kubectl get svc istio-ingressgateway --namespace istio-system --output 'jsonpath={.spec.ports[?(@.port==80)].nodePort}')"
    
    while true; do curl -H "user-agent: oktay" -s "http://[NodeIP]:[IstioIngressNodeport]/istio"; sleep 0.5; echo -e '\n'; done
    
    while true; do curl -H "user-agent: oktay" -s "http://10.10.10.10:$istio_ingress/istio"; sleep 0.5; echo -e '\n'; done 
   
    or
    
    while true; do curl -A oktay -s "http://10.10.10.10:$istio_ingress/istio"; sleep 0.5; echo -e '\n'; done 

## Monitoring and Control

Control istio Config

![https://github.com/OktaySavdi/istio-examples/tree/master/Header_Example](https://user-images.githubusercontent.com/3519706/73649385-b985a580-4690-11ea-8065-835446113ac8.png)

**Control Graph**

![https://github.com/OktaySavdi/istio-examples/tree/master/Header_Example](https://user-images.githubusercontent.com/3519706/74037118-ae06e700-49ce-11ea-879d-545f19910aed.png)

**Overview Console**

![https://github.com/OktaySavdi/istio-examples/tree/master/Header_Example](https://user-images.githubusercontent.com/3519706/73649574-1f722d00-4691-11ea-8f8f-f9ce8f21b4fa.png)

**Inbound Metrics**

![https://github.com/OktaySavdi/istio-examples/tree/master/Header_Example](https://user-images.githubusercontent.com/3519706/73649621-3d3f9200-4691-11ea-9a4f-d12737d9c59a.png)

**Weight**

![https://github.com/OktaySavdi/istio-examples/tree/master/Header_Example](https://user-images.githubusercontent.com/3519706/74037063-8c0d6480-49ce-11ea-97a2-628ab1452bb9.png)

**CLI on Server**

![https://github.com/OktaySavdi/istio-examples/tree/master/Header_Example](https://user-images.githubusercontent.com/3519706/74037001-71d38680-49ce-11ea-9a8c-a81402a05703.png)
