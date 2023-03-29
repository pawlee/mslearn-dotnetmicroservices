Forked from https://github.com/MicrosoftDocs/mslearn-dotnetmicroservices

### Prerequisites

Docker Destop<br>
kind (https://github.com/kubernetes-sigs/kind)

### Steps to run

1. Setup local docker registry
2. Start kind cluster
4. Start pods

### Setup local docker registry

Adapated from https://kind.sigs.k8s.io/docs/user/local-registry/

1. Add insecure registry to Docker Desktop (https://docs.docker.com/registry/insecure/)
2. Start registry <code>docker run -d -p "127.0.0.1:5000:5000" --restart=always --name kind-registry registry:2</code>
3. Connect registry to the cluster network <code>docker network connect "kind" "kind-registry"</code>
4. ?? Don't remember the last step in https://kind.sigs.k8s.io/docs/user/local-registry/
5. Add images to registry:
   - <code>docker-compose build<br>
     docker tag pizzabackend:latest 127.0.0.1:5000/pizzabackend:latest<br>
     docker push 127.0.0.1:5000/pizzabackend:latest<br>
     docker tag pizzafrontend:latest 127.0.0.1:5000/pizzafrontend:latest<br>
     docker push 127.0.0.1:5000/pizzafrontend:latest
     </code>

### Start kind cluster

Adapted from https://kind.sigs.k8s.io/docs/user/quick-start/.

To start: <code>kind create cluster --config kind-dev-cluster.yaml</code>

### Start pods

Kind ngnix ingress (https://kind.sigs.k8s.io/docs/user/ingress/#ingress-nginx) <code>kubectl apply -f nginx-ingress.yaml</code><br>

Wait until ready<br>
<code>kubectl wait --namespace ingress-nginx \ <br>
--for=condition=ready pod \ <br>
--selector=app.kubernetes.io/component=controller \ <br>
--timeout=90s
</code>

Start our services <code>kubectl apply -f backend-deployment.yaml,backend-service.yaml,frontend-deployment.yaml,frontend-service.yaml,frontend-ingress.yaml</code>

Use <code>http://localhost/pizza</code> and <code>http://localhost/pizza/privacy</code> to access the frontend service.