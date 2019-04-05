build:
	protoc -I. --go_out=plugins=grpc:$(GOPATH)/src/github.com/codechaitu/microservices/consignment-service \
	  proto/consignment/consignment.proto
	GOOS=linux GOARCH=amd64 go build
	docker build -t consignment-service .

run:
	docker run -p 50051:50051 consignment-service

create:
	GOOS=linux go build -o consignment-service
	docker build -t gcr.io/<account>/basic-microservice:1a .
	docker push gcr.io/<account>/basic-microservice:1a
	export PROJECT_ID=<account>
	export CLUSTER=codechaitu-microservice
	export ZONE=<zone>
	export PORT=50051
	gcloud container clusters get-credentials codechaitu-microservice --project <account> --zone asia-northeast1-b
	kubectl run consignment-service --image gcr.io/k<account>/basic-microservice:1a
	kubectl get deployments
	kubectl get pods
	kubectl expose deployment consignment-service --type LoadBalancer --port 50051 --target-port 50051
	kubectl get svc

update:
	VERSION="1a"
    GOOS=linux go build -o consignment-service
    docker build -t gcr.io/<account>/basic-microservice:$VERSION .
    docker push gcr.io/<account>/basic-microservice:$VERSION
    kubectl set image deployments/chaitu-go-elasticsearch chaitu-go-elasticsearch=gcr.io/<account>/chaitu-go-elasticsearch:$VERSION
    kubectl get pods
    kubectl get deployments
    kubectl set image deployments/chaitu-go-elasticsearch chaitu-go-elasticsearch=gcr.io/<account>/chaitu-go-elasticsearch:$VERSION
