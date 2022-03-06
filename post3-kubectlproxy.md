## kubectl proxy

kubectl proxy helps to expose the kube api-server on the localhost, it has to be run on the master node and by default running it like the below without commands will expose the kube api-server on port 8001 and have the kubectl proxy move to a background process

`kubectl proxy &`

You can then verify that the kube api-server is accessible

`curl http://localhost:8001/api/`

if you have kubernetes dashboard installed, then when you expose the kube api-server as above the kube dashboard will also be accessible via the link below

`curl http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/#/login`

You can also choose to run on a different port by using the “--port" option

`kubectl proxy --port=8080 &`
