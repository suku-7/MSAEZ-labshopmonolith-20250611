## Model
## lab-shop-monolith-20250611 (Req/Res 방식의 MSA 연동)
# Proxy 객체를 통한 동기호출 테스트
https://labs.msaez.io/#/189596125/storming/labshopmonolith-230822

![스크린샷 2025-06-11 143749](https://github.com/user-attachments/assets/657cf53f-21d8-43b2-9d49-24a11adc2097)
![스크린샷 2025-06-11 145648](https://github.com/user-attachments/assets/011c5188-3f70-4da4-b616-00f9bb7cbbfe)
![스크린샷 2025-06-11 145714](https://github.com/user-attachments/assets/d676b9be-abfe-4e84-8775-83279f772d1b)
![스크린샷 2025-06-11 145747](https://github.com/user-attachments/assets/eb76302f-469e-418d-9c60-18dee4da7af1)
![스크린샷 2025-06-11 145803](https://github.com/user-attachments/assets/025f51a7-08cf-4ef4-a608-a475a08eb14c)

## Before Running Services
### Make sure there is a Kafka server running
```
cd kafka
docker-compose up
```
- Check the Kafka messages:
```
cd infra
docker-compose exec -it kafka /bin/bash
cd /bin
./kafka-console-consumer --bootstrap-server localhost:9092 --topic
```

## Run the backend micro-services
See the README.md files inside the each microservices directory:

- order
- inventory


## Run API Gateway (Spring Gateway)
```
cd gateway
mvn spring-boot:run
```

## Test by API
- order
```
 http :8088/orders id="id"productId="productId"qty="qty"customerId="customerId"amount="amount"status="status"address="address"
```
- inventory
```
 http :8088/inventories id="id"stock="stock"
```


## Run the frontend
```
cd frontend
npm i
npm run serve
```

## Test by UI
Open a browser to localhost:8088

## Required Utilities

- httpie (alternative for curl / POSTMAN) and network utils
```
sudo apt-get update
sudo apt-get install net-tools
sudo apt install iputils-ping
pip install httpie
```

- kubernetes utilities (kubectl)
```
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
```

- aws cli (aws)
```
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
```

- eksctl 
```
curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp
sudo mv /tmp/eksctl /usr/local/bin
```
