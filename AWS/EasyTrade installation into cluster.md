# EasyTrade Deployment

## Clone Repo

git clone https://github.com/Dynatrace/easytrade.git

cd easytrade

## Create Namespace

kubectl create namespace easytrade

## Install Helm Chart

helm install easytrade \
.\helm\easytrade \
-n easytrade

## Verify

kubectl get pods -n easytrade

kubectl get svc -n easytrade

## Expose Frontend

kubectl expose deployment easytrade-frontendreverseproxy \
-n easytrade \
--type=LoadBalancer \
--port=80 \
--target-port=8080

## Verify

kubectl get svc -n easytrade

kubectl get ingress -n easytrade
