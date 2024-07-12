# Docker Compose

## 什麼是 Docker Compose？

Docker Compose 是一個用於定義和運行多容器 Docker App 的工具。通過 Docker Compose，您可以使用一個 YAML 檔來配置服務。然後，使用一個指令就可以創建並啟動所有服務。

![Docker Compose Flow](@site/src/image/docker/docker_compose_flow.webp)

## 主要特性

-   **多容器應用管理**：可以在一個文件中定義多個服務，並一次性啟動或停止這些服務。
-   **基於文件的配置**：通過 YAML 文件定義服務、網絡和 Volume，便於版本控制和共享。
-   **環境隔離**：每個服務可以在獨立的容器中運行，互不干擾。
-   **輕鬆擴展**：可以輕鬆擴展服務數量，例如增加更多的 Web 伺服器實例來處理流量。

## 安裝 Docker Compose

首先，確保你已經安裝了 Docker。然後，你可以按照以下步驟安裝 Docker Compose：

```bash

# 下載 Docker Compose

sudo curl -L "https://github.com/docker/compose/releases/download/$(curl -s https://api.github.com/repos/docker/compose/releases/latest | grep tag_name | cut -d '"' -f 4)/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

# 賦予執行權限

sudo chmod +x /usr/local/bin/docker-compose

# 檢查安裝是否成功

docker-compose --version
```

## 創建一個 Docker Compose 文件

以下是一個簡單的 \`docker-compose.yml\` 示例文件：

```yaml
version: "3"
services:
    web:
        image: nginx:latest
        ports:
            - "80:80"
    db:
        image: mysql:latest
        environment:
            MYSQL_ROOT_PASSWORD: example
```

在這個示例中，我們定義了兩個服務：\`web\` 和 \`db\`。\`web\` 服務使用 Nginx 映像，並將容器的 80 端口映射到主機的 80 端口。\`db\` 服務使用 MySQL 映像，並設置了環境變量來配置數據庫密碼。

## 使用 Docker Compose

### 啟動服務

要啟動所有定義的服務，只需運行：

```bash
docker-compose up
```

### 在後台運行

如果希望在後台運行服務，可以使用 \`-d\` 選項：

```bash
docker-compose up -d
```

### 停止服務

要停止所有運行的服務，可以使用：

```bash
docker-compose down
```

### 查看服務日誌

可以使用以下命令查看服務日誌：

```bash
docker-compose logs
```

## 更多功能

### Volume

可以在 \`docker-compose.yml\` 中定義 Volume 來持久化數據：

```yaml
version: "3"
services:
    web:
        image: nginx:latest
        volumes:
            - ./webdata:/usr/share/nginx/html
```

### 定義 networks

可以定義自定義的 networks 來隔離服務：

```yaml
version: "3"
services:
    web:
        image: nginx:latest
        networks:
            - frontend
    db:
        image: mysql:latest
        networks:
            - backend

networks:
    frontend:
    backend:
```

## 結論

Docker Compose 是一個強大的工具，能夠簡化多容器應用的管理和部署。通過一個配置文件，你可以輕鬆地定義和運行複雜的應用程序棧，極大地提高了開發和維護的效率。

想了解更多，可以前往 [Docker Compose 官方文檔](https://docs.docker.com/compose/)。
