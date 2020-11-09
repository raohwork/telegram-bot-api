# telegram-bot-api

[Local Telegram bot API server](https://github.com/tdlib/telegram-bot-api) in container.

# Usage

```sh
docker run --rm raohwork/tgbotapi-server -h

docker run -d \
  --name my-local-server \
  -p 8081:8081 -p 127.0.0.1:8082:8082 \
  -v $(pwd)/data:/data \
  raohwork/tgbotapi-server \
    --api-id=00000000 --api-hash=xxxxxxxxx \
    --http-stat-port=8082 --local
```

# License

GPLv2
