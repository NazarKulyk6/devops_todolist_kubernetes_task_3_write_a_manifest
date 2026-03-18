## Kubernetes task 3 — instructions

### 1) Build and push the image
Build the app image from repository root and push it to Docker Hub as `todoapp:3.0.0`:

```bash
docker build -t <your_dockerhub_username>/todoapp:3.0.0 .
docker push <your_dockerhub_username>/todoapp:3.0.0
```

Then update the image in `.infrastructure/todoapp-pod.yml`:

- `image: <your_dockerhub_username>/todoapp:3.0.0`

### 2) Apply manifests

```bash
kubectl apply -f .infrastructure/namespace.yml
kubectl apply -f .infrastructure/busybox.yml
kubectl apply -f .infrastructure/todoapp-pod.yml
kubectl apply -f .infrastructure/todoapp-service.yml
```

Check resources:

```bash
kubectl -n todoapp get pods -o wide
kubectl -n todoapp describe pod todoapp
```

### 3) Test app using port-forward

```bash
kubectl -n todoapp port-forward pod/todoapp 8080:8080
```

In another terminal:

```bash
curl http://localhost:8080/
curl http://localhost:8080/api/healthz/
curl http://localhost:8080/api/readyz/
```

### 4) Test app using the busyboxplus:curl container

Get the pod IP of `todoapp`:

```bash
kubectl -n todoapp get pod todoapp -o wide
```

Then run curl from the busybox pod:

```bash
kubectl -n todoapp exec -it pod/busybox-curl -- sh
```

Inside the busybox shell:

```sh
curl http://todoapp:8080/api/healthz/
curl http://todoapp:8080/api/readyz/
```

