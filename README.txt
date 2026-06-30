name: Gary Bond
eid: glb2354
comments: n/a

Setup
-----
Install Minikube
Install kubectl


Start Minikube cluster
----------------------
minikube start


Connect docker CLI on Host to Docker Daemon inside Minikube
------------------------------------------------------------
Linux/MacOS:
    eval $(minikube docker-env)

Windows (Powershell):
    minikube docker-env --shell powershell | Invoke-Expression

Verify:
    docker images

Run above steps from any terminal that you will use


Build container:
-----------------

# TODO : Requirement 2 (and 3)
docker build -t glb2354-assignment4:latest .


Deploy application to Minikube:
-------------------------------
kubectl apply -f greetings.yaml

Run Prometheus as a Docker container:
-------------------------------------

# TODO : Requirement 6
docker run -d --name prometheus --network minikube -p 9090:9090 -v ./prometheus.yml:/etc/prometheus/prometheus.yml prom/prometheus

Try app endpoints:
------------------
    Linux:
        MINIKUBE_IP=$(minikube ip)
        URL=http://$MINIKUBE_IP:32767

    MacOS/Windows:
        Use either option 1 or option 2.
        Option 2 is better as the URL will not change even when you re-deploy the application.
        Option 1:
            minikube service greetings --url
            URL=<output from previous command>
            On Windows+Powershell
            $URL=<output from previous command>
        Option 2:
            Terminal 1:
                kubectl port-forward service/greetings 8080:80
            Terminal 2:
                URL="http://localhost:8080"
        
            On Windows+Powershell, set the variable like this:
            $URL = "http://localhost:8080"

    curl $URL
    curl $URL/greetings
    curl $URL/listcontents
    curl $URL/getk8sobjects


Access Prometheus:
------------------
- In browser, open http://localhost:9090
- Go to Status -> Target health and verify that your configured Target is up (green)
- Go to Query -> check custom_request_count_total metric
    - Type "custom_request_count_total" in the Query box and Execute
    - Check the Graph view


Experiment:
-----------
- As you hit the application endpoints, the counts maintained by the Prometheus
counter should increase. You will be able to see the increasing counts in the
Prometheus Graph view


See Kubernetes resources:
-------------------------
kubectl get pods,services,configmaps,serviceaccounts,roles,rolebindings -l app=greetings


See Pod logs:
-------------
kubectl get pods -l app=greetings
kubectl logs <pod-name-from-above-output> 


Delete application:
-------------------
kubectl delete -f greetings.yaml

