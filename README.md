# kubernetes-go-project
Simple Kubernetes Go project with minikube

1. Create a new directory for your project:
```
mkdir kubernetes-go-project
cd kubernetes-go-project
```

2. Initialize a new Go module:
```
go mod init github.com/<username>/kubernetes-go-project
```

3. Create a new Go file with some code. For example, create a file called main.go with the following content:
```
package main

import (
    "fmt"
    "net/http"
)

func main() {
    http.HandleFunc("/", func(w http.ResponseWriter, r *http.Request) {
        fmt.Fprintf(w, "Hello, Kubernetes!")
    })

    http.ListenAndServe(":8080", nil)
}
```

4. Create a Dockerfile in the same directory with the following content:
```
FROM golang:1.16-alpine

WORKDIR /app

COPY . .

RUN go build -o app .

CMD ["/app/app"]
```

5. Create a Kubernetes deployment YAML file with the following content. 
This YAML file will create a deployment with one replica and a service for the deployment.

*Replace your-docker-username with your Docker Hub username.
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: kubernetes-go-project
spec:
  replicas: 1
  selector:
    matchLabels:
      app: kubernetes-go-project
  template:
    metadata:
      labels:
        app: kubernetes-go-project
    spec:
      containers:
      - name: kubernetes-go-project
        image: <your-docker-username>/kubernetes-go-project:latest
        ports:
        - containerPort: 8080
---
apiVersion: v1
kind: Service
metadata:
  name: kubernetes-go-project
spec:
  selector:
    app: kubernetes-go-project
  ports:
    - name: http
      port: 80
      targetPort: 8080
```

6. Build the Docker image:
```
docker build -t <your-docker-username>/kubernetes-go-project:latest .
```

7. and push it to Docker Hub:
```
docker push <your-docker-username>/kubernetes-go-project:latest
```

8. Start minikube and deploy the application to Kubernetes by applying the YAML file:
```
kubectl apply -f deployment.yaml
```

9. Check the status of your service:
```
kubectl get services
```

10. run the following command to access the application:
```
minikube service kubernetes-go-project
```
