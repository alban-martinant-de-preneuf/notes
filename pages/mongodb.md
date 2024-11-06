- ## avec Docker
- ```
  docker run -d \
  --name mongo-express-silver \
  -e MONGO_INITDB_ROOT_USERNAME=mongoadmin \
  -e MONGO_INITDB_ROOT_PASSWORD=password \
  -p 27017:27017 \
  -v mongodb_express_silver:/data/db \
  mongo:7.0
  ```
-
- Avec docker-compose
- ```
  version: '3.8'
  services:
    mongo: 
      image: mongo:7.0
      environment:
        MONGO_INITDB_ROOT_USERNAME: mongadmin
        MONGO_INITDB_ROOT_PASSWORD: password
      ports:
        - 27017:27017
      volumes:
        - mongodata_express_silver:/data/db
  volumes:
    mongodata_express_silver:
      driver: local
  ```
- Et `docker compose up`
-
- ## Compass
- https://www.mongodb.com/docs/compass/current/install/
- ## Source
- https://www.youtube.com/watch?v=gFjpv-nZO0U&t=188s