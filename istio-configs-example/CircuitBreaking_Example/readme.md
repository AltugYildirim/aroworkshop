

# Helloworld service (istio circuitbreaking example)

We will distribute load with circuitbreaking within the scope of istio-example

This feature allows setting limits of calls to individual hosts within a service. Such as the number of concurrent connections or how many times calls to this host have failed and etc. Once that limit has been reached the circuit breaker “trips” and stops further connections to that host. Circuit breaker pattern enables fast failure rather than clients trying to connect to an overloaded or failing host.

Circuit breaker rules are applied in destinationRulewith the settings applying to each individual host in the service, because these configurations are applied in a load balancing pool.

This sample includes three versions(v1,v2,v4) of a simple helloworld service that returns its project version when called. It can be used as a test service when experimenting with version routing.

This service is also used to demonstrate  circuitbreaking deployments working in conjunction. See  circuitbreaking deployments using Istio. Code repo for image [github\dotnet-example](https://github.com/OktaySavdi/dotnet-example)

## Start the helloworld service

The following commands assume you have [automatic sidecar injection](https://istio.io/docs/setup/additional-setup/sidecar-injection/#automatic-sidecar-injection) enabled in your cluster. If not, you'll need to modify them to include [manual sidecar injection](https://istio.io/docs/setup/additional-setup/sidecar-injection/#manual-sidecar-injection).

I used that command

    kubectl label namespace helloworld istio-injection=enabled

Deploying version v1, v2, v4 or both:

    kubectl create -f CircuitBreaking_Example/
    
![https://github.com/OktaySavdi/istio-examples/tree/master/CircuitBreaking_Example](https://user-images.githubusercontent.com/3519706/74103391-c147bd00-4b5c-11ea-8394-3e3a8717594f.png)



## Generate load

    istio_ingress="$(kubectl get svc istio-ingressgateway --namespace istio-system --output 'jsonpath={.spec.ports[?(@.port==80)].nodePort}')"
    
    while true; do curl -s "http://[NodeIP]:[IstioIngressNodeport]/istio"; sleep 0.5; echo -e '\n'; done
    
    while true; do curl -s "http://10.10.10.10:$istio_ingress/istio"; sleep 0.5; echo -e '\n'; done 

## Monitoring and Control

Control istio Config

![https://github.com/OktaySavdi/istio-examples/tree/master/CircuitBreaking_Example](https://user-images.githubusercontent.com/3519706/73649385-b985a580-4690-11ea-8065-835446113ac8.png)

**Control Graph**

![ttps://github.com/OktaySavdi/istio-examples/tree/master/CircuitBreaking_Example](https://user-images.githubusercontent.com/3519706/74125216-b422e080-4be5-11ea-816f-9fbfe0fddce7.png)

**Overview Console**

![https://github.com/OktaySavdi/istio-examples/tree/master/CircuitBreaking_Example](https://user-images.githubusercontent.com/3519706/73649574-1f722d00-4691-11ea-8f8f-f9ce8f21b4fa.png)

**Inbound Metrics**

![https://github.com/OktaySavdi/istio-examples/tree/master/CircuitBreaking_Example](https://user-images.githubusercontent.com/3519706/73649621-3d3f9200-4691-11ea-9a4f-d12737d9c59a.png)

**CLI on Server**

![https://github.com/OktaySavdi/istio-examples/tree/master/CircuitBreaking_Example](https://user-images.githubusercontent.com/3519706/74103310-31097800-4b5c-11ea-9b9a-7e97fe57b96c.png)

or try with **siege**

    wget -c https://dl.fedoraproject.org/pub/epel/7/x86_64/Packages/s/siege-4.0.2-2.el7.x86_64.rpm https://dl.fedoraproject.org/pub/epel/7/x86_64/Packages/l/libjoedog-0.1.2-1.el7.x86_64.rpm -P installation

    rpm -ivh installation/*.rpm

    siege --version

**Siege Load**

    siege -r 10 -c 4 -v http://deneme.10.10.10.10.nip.io:30889/

![https://github.com/OktaySavdi/istio-examples/tree/master/CircuitBreaking_Example](https://user-images.githubusercontent.com/3519706/74103538-dbce6600-4b5d-11ea-9528-5e21a008d092.png)
![https://github.com/OktaySavdi/istio-examples/tree/master/CircuitBreaking_Example](https://user-images.githubusercontent.com/3519706/74103548-f274bd00-4b5d-11ea-88da-4bd0be1f1e00.png)
