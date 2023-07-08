---
sidebar_position: 1
---

# Installation

## Prerequisites:

* Java 17
* Discord bot token (**Unsure?** Follow
  this [guide](https://github.com/reactiflux/discord-irc/wiki/Creating-a-discord-bot-&-getting-a-token))
* Ensure your bot is on your server
* DB Server

## Docker Installation

### Compose

```yaml title="docker-compose.yaml"
version: '2.8'

volumes:
  db-data:
  plugins:

networks:
  bb:

services:
  db:
    container_name: db
    image: postgres:latest
    restart: unless-stopped
    environment:
      POSTGRES_PASSWORD: 'password-goes-here'
      POSTGRES_USER: 'dbadmin'
      POSTGRES_DB: 'babblebot'
    ports:
      - "5432:5432"
    volumes:
      - db-data:/var/lib/postgresql/data
    networks:
      - bb
  babblebot-server:
    container_name: babblebot-server
    image: bendavies99/babblebot-server:latest
    restart: unless-stopped
    environment:
      spring.datasource.driver-class-name: 'org.postgresql.Driver'
      spring.datasource.url: 'jdbc:postgresql://db/babblebot'
      spring.datasource.username: 'dbadmin'
      spring.datasource.password: 'password-goes-here'
      spring.jpa.hibernate.ddl-auto: 'update'
      DISCORD_TOKEN: 'token-goes-here'
    volumes:
        - plugins:/workspace/plugins
    ports:
      - "21132:8080"
    networks:
      - bb
```

## Updating

### Manual Updating

Update the docker tag

### Automatic Updates

You can use watchtower if you are using docker [Watchtower](https://github.com/containrrr/watchtower)

