# Step 1. </br>
---
* 設定管理員帳號/密碼/db 名稱 (不必加上引號)

***docker-compose.yml***
```docker
environment:
            MONGO_INITDB_ROOT_USERNAME: admin
            MONGO_INITDB_ROOT_PASSWORD: admin
            MONGO_INITDB_DATABASE: <db name>
```
---
# Step 2. </br>
---
* 初始化其餘成員名稱以及可進入的db
***mongo-init.js***
```js
db.createUser(
    {
        user: "teammate1",
        pwd: "eb102",
        roles: [
            {
                role: "readWrite",
                db: "<<db name>>"
            }
        ]
    }
);
```
---
# Step 3. </br>
---
* go cmd
```cmd
docker-compose up --build
```

# Introduction of docker-compose file

***docker-compose 版本***
```docker
version: '3.0'
```
***創立mongodb service***
```docker
services:
    mongodb: # 建立一個 mongodb service
        image: mongo:latest # pull latest mongo iamge
        container_name: mongodb # 為自己的container取名
        restart: always # 重啟docker 總是開啟這個container
```
***初始環境設定***
```docker
        environment:
            MONGO_INITDB_ROOT_USERNAME: <admin username> # 設定管理員
            MONGO_INITDB_ROOT_PASSWORD: <input ur admin secret> #設定管理員密碼
            MONGO_INITDB_DATABASE: <db name># 設定初始db 名稱
```
***跟local連接的port***
```docker
        ports:
            - <這邊放本地端>:<這邊放容器端>
```
# 這邊最為重要
**PS**:docker volumes 中建立mongodb-data, 並對應到container /data/db
```docker
        volumes:
            - mongodb-data:/data/db # 容器中/data/db 對應到docker volumes         
            - ./mongo-init.js:/docker-entrypoint-initdb.d/mongo-init.js:ro  # 進入到mongo-init.js 初始化裡面的設定
volumes:
    mongodb-data:
```




