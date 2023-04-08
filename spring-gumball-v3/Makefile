all: clean

clean:
	mvn clean

compile:
	mvn compile

run: compile
	mvn spring-boot:run

build:
	mvn package

run-jar: build
	java -jar target/spring-gumball-3.0.jar


# MySQL DB

mysql:
	docker run -d --network gumball --name mysql -td -p 3306:3306 -e MYSQL_ROOT_PASSWORD=cmpe172 mysql:8.0

mysql-shell:
	docker exec -it mysql bash


# Docker

docker-build: build
	docker build -t spring-gumball-v3 .
	docker images

gumball-network:
	docker network create --driver bridge gumball

docker-run: docker-build
	docker run --network gumball -e "MYSQL_HOST=mysql" --name spring-gumball -td -p 8080:8080 spring-gumball-v3

docker-clean:
	docker stop spring-gumball-v3
	docker rm spring-gumball-v3
	docker rmi spring-gumball-v3

docker-shell:
	docker exec -it spring-gumball-v3 bash

docker-push:
	docker login
	docker build -t $(account)/spring-gumball:v3.0 .
	docker push $(account)/spring-gumball:v3.0

# Compose

network-ls:
	docker network ls

network-create:
	docker network create --driver bridge $(network)

network-prune:
	docker network prune

compose-up:
	docker-compose up --scale gumball=2 -d

lb-up:
	docker-compose up -d lb

gumball-up:
	docker-compose up -d gumball

mysql-up:
	docker-compose up -d mysql

compose-down:
	docker-compose down

lb-stats:
	echo "user = admin | password = admin"
	open http://localhost:1936

lb-test:
	open http://localhost