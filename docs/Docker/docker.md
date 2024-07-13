# Docker 介紹

Docker 是一個開源平台，旨在簡化應用程式的部署和管理。它通過使用容器技術將應用程式及其所有依賴打包在一起，確保應用程式在不同環境中能一致地運行。Docker 容器比傳統虛擬機更輕量，啟動速度更快，並且資源利用效率更高。這使得 Docker 成為 DevOps 團隊實現持續集成和持續交付（CI/CD）的理想工具，極大地提高了開發和部署的靈活性和效率。

## Docker 的好處

1. **一致性和可移植性**：確保應用程式在任何環境中都能一致地運行。
2. **資源效率**：共享操作系統內核，比虛擬機更節省資源。
3. **快速部署和擴展**：容器啟動速度快，支持自動化部署和擴展。
4. **依賴隔離**：每個容器內部包含其所有依賴項，避免相依性衝突。
5. **版本控制和回滾**：允許創建和管理應用的多個版本，便於快速回滾至穩定版本。

[**官方網站下載 Docker desktop**](https://www.docker.com/)
[**官方網站下載 Docker compose**]([https://www.docker.com/](https://docs.docker.com/compose/install/)

## 優勢

### 傳統部署困難點（沒有 Docker 容器化技術之前）

![docker_structure_traditional](@site/src/image/docker/docker1.png)

1. **環境不一致**：
    - **範例**：開發人員在本地安裝了 Java 8，但生產環境使用 Java 11，導致應用在本地運行正常，但在生產環境中出現兼容性問題。
2. **依賴性管理**：
    - **範例**：應用 A 需要 libX v1.2，而應用 B 需要 libX v1.3，這在同一台伺服器上很難同時滿足，容易導致衝突和安裝失敗。
3. **部署過程繁瑣**：
    - **範例**：部署一個應用需要手動安裝 Web 伺服器、配置數據庫、設置防火牆規則等，步驟繁多且容易出錯，需要詳細的部署文檔和專業知識。
4. **資源浪費**：
    - **範例**：傳統虛擬機器（如 VMware 或 VirtualBox）每個應用都需要一個完整的操作系統實例，導致 CPU 和內存資源的大量浪費。

### 有 Docker 容器化技術之後

![docker_structure_traditional](@site/src/image/docker/docker2.png)

1. **一致性和可移植性**：
    - **範例**：使用 Docker，開發人員可以在 Dockerfile 中定義應用所需的環境和依賴，無論在哪裡運行這個容器（本地、測試或生產環境），都能保證一致的運行環境。
2. **依賴性隔離**：
    - **範例**：每個應用運行在自己的容器中，容器內部包含所有所需的依賴和庫，這樣應用 A 和應用 B 可以在同一台伺服器上運行而不會相互干擾。
3. **快速部署和擴展**：
    - **範例**：容器可以在幾秒鐘內啟動，並且可以通過 Docker Compose 或 Kubernetes 自動化部署，實現應用的快速擴展和縮減。例如，黑色星期五期間，電商平台可以迅速增加應用副本來應對高流量。
4. **資源效率**：

    - **範例**：Docker 容器共享操作系統內核，因此每個容器只包含應用及其依賴，無需整個操作系統。這樣，同樣的硬體資源可以運行更多的應用。例如，在一台 8GB 內存的伺服器上可以運行 10 個容器，而傳統虛擬機器可能只能運行 2-3 個。

## Docker, Image 和 Container 的關係

![docker_relationship](@site/src/image/docker/docker3.png)

### Docker Image

-   \*映像（Image）\*\*是應用程式的模板，包含運行應用所需的所有文件和設置。具體來說，映像包括：

1. **基礎層**：
    - 包含操作系統核心文件（如 Linux 的基本文件系統）。
2. **應用依賴**：
    - 運行應用程式所需的庫和工具（如 Python、Node.js、Java 等）。
3. **應用代碼**：
    - 應用的源代碼和可執行文件。
4. **配置文件**：
    - 應用運行所需的配置文件。
5. **執行命令**：
    - 應用啟動時需要執行的命令（如啟動腳本）。

映像是只讀的，通過一個或多個層次結構構建，並且可以**多次重用**來創建容器。

### Docker Container

-   \*容器（Container）**是映像(Image 檔案)的運行實例，為應用提供一個**獨立的運行環境\*\*。容器包含：

1. **映像層**：
    - 基於映像創建，包含所有映像中的文件和設置。
2. **可寫層**：
    - 在容器運行過程中生成的動態數據（如日誌文件、臨時文件等）。
3. **運行時設置**：
    - 容器啟動時的環境變量、網絡配置、卷(Volumes)掛載等。

```jsx
# 指定基礎映像，這裡選擇 Python 3.9 映像，作為所有後續指令的基礎層。
FROM python:3.9-slim

# 設置容器內的工作目錄，所有後續指令都在這個目錄下執行。
WORKDIR /app

# 複製應用程序的依賴文件到工作目錄
COPY [my_text] .

# 安裝應用所需的 Python lib。
RUN pip install --no-cache-dir -r requirements.txt

# 複製應用程式到工作目錄
COPY . .

# 指定容器啟動時運行的命令
CMD ["python", "app.py"]
```

dockerfile demo
image 檔案基於 dockerfile 生成。

## 總結

-   **映像**：是一個靜態的模板，包含應用及其運行環境的所有內容。**不可變且可重複使用**。
-   **容器**：是映像的動態實例，提供**獨立的運行環境**，包括一個可寫層，允許在運行時產生和存儲數據。

## 基本指令

### 運行 docker 指令

-   **拉取映像**
    ```bash
    docker pull [image_name]
    ```
    例如，拉取最新版本的 nginx 映像：
    ```bash
    docker pull nginx:latest
    ```
-   **列出所有容器**
    列出所有運行中的容器：
    ```bash
    docker ps
    ```
    列出所有容器（包括停止的容器）：
    ```bash
    docker ps -a
    ```
-   **啟動容器**
    ```bash
    docker run [options] [image_name]
    ```
    例如，啟動一個 nginx 容器並在 80port 運行：
    ```bash
    docker run -d -p 80:80 nginx
    ```
-   **停止容器**
    ```bash
    docker stop [container_id]
    ```
-   **刪除容器**
    ```bash
    docker rm [container_id]
    ```
-   **刪除映像**
    ```bash
    docker rmi [image_name]
    ```
-   **進入容器**
    ```bash
    docker exec -it [container_id] /bin/bash
    ```

### 常用參數

-   **d (**後台運行容器**)**
    -   例如：`docker run -d nginx`
-   **p (**映射主機端口到容器端口**)**
    -   例如：`docker run -p 8080:80 nginx`
-   **-name (**為容器指定一個名字**)**
    -   例如：`docker run --name my_nginx -d nginx`
-   **v (**掛載主機目錄到容器**)**
    -   例如：`docker run -v /host/path:/container/path nginx`
-   **e (**設置環境變量**)**
    -   例如：`docker run -e MY_ENV_VAR=value nginx`
-   **-rm (**容器停止後自動刪除**)**
    -   例如：`docker run --rm nginx`

## 練習

接著我們實際試看看如何操作 docker

```bash
docker create nginx
```

```bash
docker ps -a
//-a 會顯示停止的容器
```

```bash
docker start [container id]
```

```bash
docker exec -it [container name] /bin/bash
```

```bash
docker stop [container id]
```

```bash
docker run -p 8080:80 nginx
```

# Dockerfile

```jsx
//Dockerfile
# 使用 Node.js 18 版本的官方image檔案
# 前端多為node，18為版號可變更或選latest,alpine,若沒給則預設latest
# alpine為輕量化映像檔，優點快速啟動，缺點可能需要手動安裝其他依賴...
FROM node:18

# 創建app資料夾並以其為工作環境
WORKDIR /app

# 複製 package.json 和 package-lock.json（如果存在）
COPY package*.json ./

# 安裝依賴
RUN npm install
# 複製專案文件到工作目錄
COPY . .
#複製.env檔案(如果存在)，建構環境檔案有很多方法，也可以參考docker-compose.yml
COPY .env ./
# 構建 Nuxt 應用
RUN npm run build

# 開放 3000 端口,可有可無(若用docker desktop則必須)
EXPOSE 3000
# 啟動 Nuxt 服務
 CMD ["node", ".output/server/index.mjs"]
```

[**Dockerfile 介紹**](https://docs.docker.com/reference/dockerfile/)

[**alpine 補充**](https://www.docker.com/blog/how-to-use-the-alpine-docker-official-image/)

範例

-   基於 alpine image

    ```bash
    FROM alpine:3.14

    # 安裝 Node.js 和 npm
    RUN apk add --no-cache nodejs npm

    WORKDIR /app

    COPY package*.json ./

    RUN npm install

    COPY . .

    EXPOSE 3000

    CMD ["node", "index.js"]
    ```

-   基於 node:alpine image

    ```bash
    FROM node:18-alpine

    WORKDIR /app

    COPY package*.json ./

    RUN npm install

    COPY . .

    EXPOSE 3000

    CMD ["node", "index.js"]
    ```

# 範例

### 範例一

創建一個專案，並於專案內新增 index.html，內容隨意

```bash
docker run -p [port you want to visit]:80 -v [your path]:/usr/share/nginx/html nginx
```

### 範例二

[https://github.com/tf00185077/docker-practice](https://github.com/tf00185077/docker-practice)

該專案透過 docker compose，以兩個 image 分別創建兩個 container 並定義兩者關係。

可以上傳圖片並觀察 container 內的文件和本地端的文件的關係。

運行`docker compose up`開啟專案，並在 localhost:3000 以及 localhost:80 查看

-   Docker compose 介紹
    ![docker_compose](@site/src/image/docker/docker4.png)

    Docker Compose 是一個用於定義和運行多容器 Docker 應用的工具。使用 Docker Compose，你可以通過一個 `docker-compose.yml` 文件來配置應用所需的所有服務，然後使用簡單的命令來創建並啟動所有這些服務。這使得多容器應用的部署和管理更加簡便。

-   docker-compose.yml 講解
    ```bash
    nginx:
      image: tf00185077/docker-nginx
      container_name: nginx
      deploy:
        restart_policy:
          condition: on-failure
          delay: 5s
          max_attempts: 3
          window: 120s
      ports:
        - '80:80'
      depends_on:
        - nuxt
      volumes:
        - /etc/letsencrypt/live/yourdomain.com/:/etc/letsencrypt/live/yourdomain.com/
        - /etc/letsencrypt/:/etc/letsencrypt
    ```
    -   **image**：使用 `tf00185077/docker-nginx` 映像。
    -   **container_name**：容器名稱設為 `nginx`。
    -   **deploy.restart_policy**：定義重新啟動策略，當容器失敗時重試，每次重試間隔 5 秒，最多重試 3 次，在 120 秒內計數。
    -   **ports**：將宿主機的 80 端口映射到容器的 80 端口。
    -   **depends_on**：設置 nginx 依賴於 `nuxt` 服務。
    -   **volumes**：掛載本地 `/etc/letsencrypt/live/yourdomain.com/` 和 `/etc/letsencrypt/` 到容器內相應目錄，通常用於 SSL 證書。(此處沒有使用)
    ```bash
    nuxt:
      image: tf00185077/docker-practice
      container_name: nuxt
      deploy:
        restart_policy:
          condition: on-failure
          delay: 5s
          max_attempts: 3
          window: 120s
      ports:
        - '3000:3000'
      restart: always
      volumes:
        - ./public/pic:/nuxtapp/public/pic
    ```
    -   **image**：使用 `tf00185077/docker-practice` 映像。
    -   **container_name**：容器名稱設為 `nuxt`。
    -   **deploy.restart_policy**：定義重新啟動策略，當容器失敗時重試，每次重試間隔 5 秒，最多重試 3 次，在 120 秒內計數。
    -   **ports**：將宿主機的 3000 端口映射到容器的 3000 端口。
    -   **restart**：設置容器總是重啟。
    -   **volumes**：將本地目錄 `./public/pic` 映射到容器內的 `/nuxtapp/public/pic` 目錄。
