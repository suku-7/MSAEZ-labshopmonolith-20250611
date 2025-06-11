# Model
# lab-shop-monolith-20250611 (Req/Res 방식의 MSA 연동)
## Proxy 객체를 통한 동기호출 테스트
https://labs.msaez.io/#/189596125/storming/labshopmonolith-230822

![스크린샷 2025-06-11 143749](https://github.com/user-attachments/assets/657cf53f-21d8-43b2-9d49-24a11adc2097)
![스크린샷 2025-06-11 145648](https://github.com/user-attachments/assets/011c5188-3f70-4da4-b616-00f9bb7cbbfe)
![스크린샷 2025-06-11 145714](https://github.com/user-attachments/assets/d676b9be-abfe-4e84-8775-83279f772d1b)
![스크린샷 2025-06-11 145747](https://github.com/user-attachments/assets/eb76302f-469e-418d-9c60-18dee4da7af1)
![스크린샷 2025-06-11 145803](https://github.com/user-attachments/assets/025f51a7-08cf-4ef4-a608-a475a08eb14c)

## 터미널 작성 참고용

1. sdk install java
2. lombok 1.18.30으로 수정
- inventory, monolith 버전 수정

- order DB에는 inventory 서버가 있을거니까 그걸 8083에 담는다.
- order에 함수를 보면 decrease stock 1번 제품 10개를 까라고 하면은, 그 정보를 decreaseStock 함수에 넣어 실행
- 이걸 실행하면은 put으로 api로 inventory로 put을 쏘고 받아서 처리해주는거임. (대리자-프록시라고 부름)
- 실제는 통신을 해서 가져오는거지만 답만 가져오면 된다.

3. inventory 실행
- cd inventory/
- mvn spring-boot:run

4. monolith 실행
- cd monolith/
- mvn spring-boot:run

5. Proxy 객체를 통한 동기호출 테스트

- http POST :8083/inventories id=1 stock=10
- http PUT :8083/inventories/1/decreasestock qty=3
- http GET :8083/inventories/1  # stock must be 7

- 새 터미널
- http POST :8082/orders productId=1 qty=5
- http GET :8083/inventories/1  # stock must be 2

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
