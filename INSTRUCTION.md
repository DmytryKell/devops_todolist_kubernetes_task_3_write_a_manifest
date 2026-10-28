# Kubernetes deployment instructions

## 1. Apply the manifests

Create the namespace:

```bash
kubectl apply -f .infrastructure/namespace.yml
```

Start the busybox diagnostic pod:

```bash
kubectl apply -f .infrastructure/busybox.yml
```

Start the ToDo application pod:

```bash
kubectl apply -f .infrastructure/todoapp-pod.yml
```

Check pod status:

```bash
kubectl get pods -n todoapp
```

## 2. Test the app with port-forward

Forward the application port to your machine:

```bash
kubectl port-forward pod/todoapp -n todoapp 8000:8000
```

Then test the health endpoints in another terminal:

```bash
curl http://localhost:8000/api/readiness/
curl http://localhost:8000/api/liveness/
```

## 3. Test the app from the busybox container

Get the pod IP address of the ToDo app:

```bash
kubectl get pod todoapp -n todoapp -o wide
```

Open a shell inside the busybox container:

```bash
kubectl exec -it busybox -n todoapp -- sh
```

Inside the container, call the app's health endpoints using the ToDo app pod IP:

```sh
curl http://<todoapp-pod-ip>:8000/api/readiness/
curl http://<todoapp-pod-ip>:8000/api/liveness/
```

Example:

```sh
curl http://10.244.0.15:8000/api/liveness/
```

## 4. Docker image

```yaml
image: dmytrykell/todoapp:3.0.0
```
