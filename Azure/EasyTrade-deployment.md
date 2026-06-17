# EasyTrade Deployment

## Clone Repository

git clone https://github.com/Dynatrace/easytrade.git

cd easytrade

## Deploy Application

kubectl apply -f kubernetes-manifests/

## Verify Pods

kubectl get pods -n easytrade

## Verify Services

kubectl get svc -n easytrade

## Expected Components

- Frontend
- Frontend Reverse Proxy
- Manager
- Engine
- Login Service
- Offer Service
- RabbitMQ
- Database
