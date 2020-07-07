
# Helloworld service (istio  mirror example)

We will distribute load with mirror within the scope of istio-example

This sample includes two versions(v1,v2) of a simple helloworld service that returns its project version when called. It can be used as a test service when experimenting with version routing.

This service is also used to demonstrate mirror deployments working in conjunction. See mirror deployments using Istio. Code repo for image [github\dotnet-example](https://github.com/OktaySavdi/dotnet-example)

## Start the helloworld service

The following commands assume you have [automatic sidecar injection](https://istio.io/docs/setup/additional-setup/sidecar-injection/#automatic-sidecar-injection) enabled in your cluster. If not, you'll need to modify them to include [manual sidecar injection](https://istio.io/docs/setup/additional-setup/sidecar-injection/#manual-sidecar-injection).

I used that command

    kubectl label namespace helloworld istio-injection=enabled

Deploying version v1, v2, or both:

    kubectl create -f Mirror_Example/
![https://github.com/OktaySavdi/istio-examples/tree/master/Mirror_Example](https://user-images.githubusercontent.com/3519706/74125496-8ab68480-4be6-11ea-8839-a9855d951440.png)

## Generate load
```bash
istio_ingress="$(kubectl get svc istio-ingressgateway --namespace istio-system --output 'jsonpath={.spec.ports[?(@.port==80)].nodePort}')"
    
while true; do curl -s "http://[NodeIP]:[IstioIngressNodeport]/istio"; sleep 0.5; echo -e '\n'; done
    
while true; do curl -s "http://10.10.10.10:$istio_ingress/istio"; sleep 0.5; echo -e '\n'; done 
```
## Monitoring and Control

Control istio Config

![https://github.com/OktaySavdi/istio-examples/tree/master/Mirror_Example](https://user-images.githubusercontent.com/3519706/73649385-b985a580-4690-11ea-8065-835446113ac8.png)

**Control Graph**

![https://github.com/OktaySavdi/istio-examples/tree/master/Mirror_Example](https://user-images.githubusercontent.com/3519706/74037780-08547780-49d0-11ea-8619-80c092bf3210.png)

**Overview Console**

![https://github.com/OktaySavdi/istio-examples/tree/master/Mirror_Example](https://user-images.githubusercontent.com/3519706/73649574-1f722d00-4691-11ea-8f8f-f9ce8f21b4fa.png)

**Inbound Metrics**

![https://github.com/OktaySavdi/istio-examples/tree/master/Mirror_Example](https://user-images.githubusercontent.com/3519706/73649621-3d3f9200-4691-11ea-9a4f-d12737d9c59a.png)

**Weight**

![https://github.com/OktaySavdi/istio-examples/tree/master/Mirror_Example](https://user-images.githubusercontent.com/3519706/74037995-7731d080-49d0-11ea-9108-3567fe1635cc.png)

**CLI on Server**

![https://github.com/OktaySavdi/istio-examples/tree/master/Mirror_Example](https://user-images.githubusercontent.com/3519706/74037807-14d8d000-49d0-11ea-913f-232db9095b09.png)
