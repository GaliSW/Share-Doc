# Docker 實作

# 準備環境

## Jenkinsfile

```jsx
pipeline {
    agent {
        docker {
          image 'docker:dind'
          args '-v /var/run/docker.sock:/var/run/docker.sock' // 掛載 Docker socket 已使用local Docker
        }
      }

    environment {
        NODE_ENV = 'production'
        DOCKERHUB_CREDENTIALS = credentials('dockerhub') // 確保ID正確
    }

    stages {
        stage('Build Docker Image') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'DOCKERHUB_PASSWORD', usernameVariable: 'DOCKERHUB_USERNAME')]) {
                    sh 'echo $DOCKERHUB_PASSWORD | docker login -u $DOCKERHUB_USERNAME --password-stdin'
                    }
                    // 建構Image
                    sh "docker build -t tf00185077/jenkins ."
                }
            }
        }
        stage('Push Docker Image') {
            steps {
                script {
                    // 推送Image
                    sh "docker push tf00185077/jenkins"
                }
            }
        }
    }

}
```

## dockerfile

```
FROM node:lts as build-stage
WORKDIR /nuxtapp
COPY . .
RUN npm install
RUN npm run build
RUN rm -rf node_modules && \
    NODE_ENV=production npm install \
    --prefer-offline \
    --pure-lockfile \
    --non-interactive \
    --production=true
FROM node:lts as prod-stage
WORKDIR /nuxtapp
COPY --from=build-stage /nuxtapp/.output/  ./.output/
CMD [ "node", ".output/server/index.mjs" ]
```

## jenkins 環境

```jsx
docker run -u root -d --name jenkins -p 8080:8080 -p 50000:50000 -v jenkins_home:/var/jenkins_home -v /var/run/docker.sock:/var/run/docker.sock  jenkins/jenkins:lts
```

## ngrok

下載 [\*\*ngrok](https://ngrok.com/)\*\* 並執行

```jsx
ngrok http 8080
```

![docker_practice_1](@site/src/image/docker/docker_practice_1.png)

## dockerhub

在 docker hub 註冊一組帳號，並準備一個 repository

![docker_practice_2](@site/src/image/docker/docker_practice_2.png)

## github 登入 token

![docker_practice_3](@site/src/image/docker/docker_practice_3.png)

## github 專案設置 webhook

![docker_practice_4](@site/src/image/docker/docker_practice_4.png)

Payload URL 填上 ngrok 產生的網址，並在最後面加上/github-webhook/

Content type 選擇 application json

## Jekines 環境設置

### 設置 docker hub 和 git hub 登入資訊

![docker_practice_5](@site/src/image/docker/docker_practice_5.png)

docker hub : 選擇**Username with password 輸入帳號密碼**

github: 選擇 secret text，並將剛剛的 github 登入 TOKEN 填入

### 設置 pipeline

![docker_practice_6](@site/src/image/docker/docker_practice_6.png)

URL 將專案網址填上

### 準備 docker in docker

因為現在 jenkins 會執行 docker 指令，但是我們 jenkins 是用 docker 啟動的 container，在該 container 中沒有 docker，所以現在運行建置流程會有 docker not found 的錯誤，所以我們要在該 container 內安裝 docker，並將本地的 docker 和 container 內的 docker 用 volume 串起，才可以使用 docker 指令。

-   Windows
    ```bash
    # 進到CONTAINER並開啟互動輸入框
    docker exec -it --user root jenkins /bin/bash
    ```
    ```bash
    # 安裝DOCKER
    apt-get update && apt-get -y install apt-transport-https ca-certificates curl gnupg2 software-properties-common && curl -fsSL https://download.docker.com/linux/$(. /etc/os-release; echo "$ID")/gpg > /tmp/dkey; apt-key add /tmp/dkey && add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/$(. /etc/os-release; echo "$ID") $(lsb_release -cs) stable" && apt-get update && apt-get -y install docker-ce
    ```
-   如果是 MAC 主機並且芯片是 m1 以上
    ```bash
    docker exec -it --user root jenkins /bin/bash
    ```
    ```bash
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg |  gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
    echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" |  tee /etc/apt/sources.list.d/docker.list > /dev/null
    ```

以上都完成後，可以試看看 push 一個新的 commit 到 master 看看結果是否順利。

# 補充:

[**CI/CD 選用環境比較**](https://www.infoq.cn/article/9hscujuukmbbwjpr0p0g)
