


# Helloworld service (istio mTLS example)

We will distribute load with mTLS within the scope of istio-example

This sample includes two versions(v1,v2) of a simple helloworld service that returns its project version when called. It can be used as a test service when experimenting with version routing.

This service is also used to demonstrate mTLS deployments working in conjunction. See mTLS deployments using Istio. Code repo for image [github\dotnet-example](https://github.com/OktaySavdi/dotnet-example)

## Start the helloworld service

The following commands assume you have [automatic sidecar injection](https://istio.io/docs/setup/additional-setup/sidecar-injection/#automatic-sidecar-injection) enabled in your cluster. If not, you'll need to modify them to include [manual sidecar injection](https://istio.io/docs/setup/additional-setup/sidecar-injection/#manual-sidecar-injection).

I used that command
```ruby
kubectl label namespace helloworld istio-injection=enabled
```

Deploying version v1, v2 or both:
```ruby
kubectl create -f mTLS_Example/
```
    
![https://github.com/OktaySavdi/istio-examples/tree/master/mTLS_Example](https://user-images.githubusercontent.com/3519706/74230323-33431200-4cd5-11ea-82d1-b3af33463205.png)

To enable mTLS:

```ruby
kubectl create -f istiofiles/authentication-enable-tls.yml -n tutorial
kubectl create -f istiofiles/destination-rule-tls.yml -n tutorial
```

Check the mTLS by  _sniffing_  traffic between services, which is a bit more tedious, open a new terminal tab and run next command:

```ruby
CUSTOMER_POD=$(kubectl get pod | grep helloworld-v1| awk '{ print $1}' )

kubectl exec -it $CUSTOMER_POD -c istio-proxy /bin/bash

# Inside pod shell

ifconfig 
sudo tcpdump -vvvv -A -i eth0 '((dst port 80) and (net 172.17.0.10))' 
```

Get helloworld pod name

Open a shell inside pod

Get IP of current pod (probably the IP represented at  `eth0`  interface)

Capture traffic from  `eth0`  (or your interface) of port  `8080`  and network  `192.168.27.41`  (your IP from  `ifconfig`)

![https://github.com/OktaySavdi/istio-examples/tree/master/mTLS_Example](https://user-images.githubusercontent.com/3519706/74231258-422ac400-4cd7-11ea-8dfc-7b885d35d21e.png)

So now go to a terminal and execute:

```ruby
curl http://10.10.10.10:30292/istio
helloworld-v2-758f56497f-bdxfn | This is Project:1 | CallNumber: 1
```
Obviously, the response is exactly the same, but if you go to the terminal where you are executing `tcpdump`, you should see something like:

![https://github.com/OktaySavdi/istio-examples/tree/master/mTLS_Example](https://user-images.githubusercontent.com/3519706/74231434-9cc42000-4cd7-11ea-9597-c532904e2806.png)

Notice that you cannot see any detail about the communication since it happened through TLS.

Now, let’s disable _TLS_:

![https://github.com/OktaySavdi/istio-examples/tree/master/mTLS_Example](https://user-images.githubusercontent.com/3519706/74231664-270c8400-4cd8-11ea-8fe0-109759bbd5b1.png)

## Monitoring and Control

Control istio Config

![https://github.com/OktaySavdi/istio-examples/tree/master/mTLS_Example](https://user-images.githubusercontent.com/3519706/74230350-435af180-4cd5-11ea-9f37-31fca31f7d4a.png)
