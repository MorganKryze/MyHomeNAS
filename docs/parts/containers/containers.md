# My go to containers list

> This section is dedicated to the containers I use on a daily basis. I will try to keep it updated as much as possible.

## Table of Contents

- [My go to containers list](#my-go-to-containers-list)
  - [Table of Contents](#table-of-contents)
  - [Introduction](#introduction)
  - [Cloudflare](#cloudflare)
  - [Utilities](#utilities)
  - [Media](#media)
  - [Gaming](#gaming)

## Introduction

This list will present the containers alonside their description, config and prerequisites. Keep in mind that they are presented to be used within Cloudflare tunneling. I would recommend a VPS / Dedicated server with a reverse proxy to handle the traffic for production purposes.

> [!WARNING]
> The following files are templates, you will need to change the paths to your own. The paths are based on the shared folders you created in your NAS (e.g. change `CHANGE_TO_COMPOSE_DATA_PATH` to something like `/srv/dev-disk-by-label-Data/`).
>
> The `SKIP_BACKUP` and `BACKUP` tags are used to indicate if the folder should be backed up or not by the `Compose` plugin of OpenMediaVault.

## Cloudflare

> [Cloudflare tunnels](https://developers.cloudflare.com/cloudflare-one/connections/connect-networks/): expose your local services to the internet to test your services before deploying them to production for instance.
>
> You will need:
>
> - a domain name, managed by Cloudflare
> - a tunnel token

File:

```yaml
services:
  cloudflared:
    image: cloudflare/cloudflared:latest
    container_name: cloudflare-tunnel
    restart: unless-stopped
    command: tunnel --no-autoupdate run
    environment:
      - TUNNEL_TOKEN=${TUNNEL_TOKEN}
```

Environment: (mind to change the token)

```plaintext
TUNNEL_TOKEN=your_token_here
```

## Utilities

> [FileBrowser](https://filebrowser.org/): a file manager with multi-user support, file uploading, sharing, and more.
>
> Once started, you can access the web interface by accessing `http://your-nas-ip:9000` on your browser.

```yaml
services:
  filebrowser:
    image: filebrowser/filebrowser:latest
    container_name: filebrowser
    restart: unless-stopped
    ports:
      - '9000:80'
    volumes:
      - CHANGE_TO_BASE_PATH:/srv # SKIP_BACKUP
      - CHANGE_TO_COMPOSE_DATA_PATH/FileBrowser/filebrowser.db:/database/filebrowser.db # SKIP_BACKUP
      - CHANGE_TO_COMPOSE_DATA_PATH/FileBrowser/settings.json:/config/settings.json # BACKUP
    environment:
      - PUID=$(id -u)
      - PGID=$(id -g)
```

> [Stirling pdf](https://www.stirlingpdf.com/): a large pdf tool to convert, merge, split, and compress pdf files.
>
> Once started, you can access the web interface by accessing `http://your-nas-ip:9100` on your browser.

```yaml
services:
  stirling-pdf:
    image: stirlingtools/stirling-pdf:latest-fat
    container_name: stirling-pdf
    restart: unless-stopped
    ports:
      - '9100:8080'
    volumes:
      - CHANGE_TO_COMPOSE_DATA_PATH/StirlingPDF/trainingData:/usr/share/tessdata # SKIP_BACKUP
      - CHANGE_TO_COMPOSE_DATA_PATH/StirlingPDF/extraConfigs:/configs # SKIP_BACKUP
      - CHANGE_TO_COMPOSE_DATA_PATH/StirlingPDF/customFiles:/customFiles/ # SKIP_BACKUP
      - CHANGE_TO_COMPOSE_DATA_PATH/StirlingPDF/logs:/logs/ # SKIP_BACKUP
      - CHANGE_TO_COMPOSE_DATA_PATH/StirlingPDF/pipeline:/pipeline/ # SKIP_BACKUP
    environment:
      - DOCKER_ENABLE_SECURITY=${DOCKER_ENABLE_SECURITY}
      - LANGS=${LANGS}
```

Environment:

```plaintext
DOCKER_ENABLE_SECURITY=false
LANGS=en_GB
```

> [SearXNG](https://docs.searxng.org/): a privacy-respecting, hackable metasearch engine.
>
> Once started, you can access the web interface by accessing `http://your-nas-ip:9200` on your browser. To set it as a default search engine, you can use the following URL: `http://your-nas-ip:9100/?q=%s`.

> [!IMPORTANT]
> After the first run, you will need to uncomment the `cap_drop` lines in the following configuration. As it is a YAML file, you will need to be careful with the indentation.

```yaml
services:
  redis:
    container_name: redis
    image: docker.io/valkey/valkey:8-alpine
    command: valkey-server --save 30 1 --loglevel warning
    restart: unless-stopped
    networks:
      - searxng
    volumes:
      - CHANGE_TO_COMPOSE_DATA_PATH/SearXNG/valkey-data2:/data # SKIP_BACKUP
    # cap_drop: # uncomment after the first run
    #   - ALL
    cap_add:
      - SETGID
      - SETUID
      - DAC_OVERRIDE
    logging:
      driver: 'json-file'
      options:
        max-size: '1m'
        max-file: '1'

  searxng:
    container_name: searxng
    image: docker.io/searxng/searxng:latest
    restart: unless-stopped
    networks:
      - searxng
    ports:
      - '9200:8080'
    volumes:
      - CHANGE_TO_COMPOSE_DATA_PATH/SearXNG/:/etc/searxng:rw # SKIP_BACKUP
    environment:
      - UWSGI_WORKERS=${SEARXNG_UWSGI_WORKERS:-4}
      - UWSGI_THREADS=${SEARXNG_UWSGI_THREADS:-4}
    # cap_drop: # uncomment after the first run
    #   - ALL
    cap_add:
      - CHOWN
      - SETGID
      - SETUID
    logging:
      driver: 'json-file'
      options:
        max-size: '1m'
        max-file: '1'

networks:
  searxng:

volumes:
  valkey-data2:
```

Before starting the container, you will need to create a `settings.yml` file in the `CHANGE_TO_COMPOSE_DATA_PATH/SearXNG/` folder. Here is an example of the file:

```yaml
# see https://docs.searxng.org/admin/installation-searxng.html#configuration
use_default_settings: true

general:
  debug: false
  instance_name: 'SearXNG'
  enable_metrics: false

search:
  safe_search: 2
  autocomplete: 'brave'
  favicon_resolver: 'duckduckgo'

server:
  # base_url is defined in the SEARXNG_BASE_URL environment variable, see .env and docker-compose.yml
  secret_key: 'ultrasecretkey' # change this!
  limiter: true # can be disabled for a private instance
  image_proxy: true
  method: 'GET'

ui:
  static_use_hash: true

redis:
  url: redis://redis:6379/0

enabled_plugins:
  - 'Hash plugin'
  - 'Self Information'
  - 'Tracker URL remover'
  - 'Ahmia blacklist'
```

Then create the `limiter.toml` file in the same folder:

```toml
# This configuration file updates the default configuration file
# See https://github.com/searxng/searxng/blob/master/searx/limiter.toml

[botdetection.ip_limit]
# To get unlimited access in a local network, by default link-local addresses
# (networks) are not monitored by the ip_limit
filter_link_local = true

# activate link_token method in the ip_limit method
link_token = true
```

Then execute th following command to replace the secret key from the `CHANGE_TO_COMPOSE_DATA_PATH/SearXNG/` directory:

```bash
sed -i "s|ultrasecretkey|$(openssl rand -hex 32)|g" ./settings.yml
```

## Media

> [Glance](https://github.com/glanceapp/glance): a self-hosted dashboard that puts all your feeds in one place.
>
> Once started, you can access the web interface by accessing `http://your-nas-ip:9300` on your browser.

```yaml
services:
  glance:
    image: glanceapp/glance
    container_name: glance
    volumes:
      - CHANGE_TO_COMPOSE_DATA_PATH/Glance/glance.yml:/app/glance.yml
      - /etc/timezone:/etc/timezone:ro
      - /etc/localtime:/etc/localtime:ro
    ports:
      - 9300:8080
    restart: unless-stopped
```

Before starting the container, you will need to create a `glance.yml` file in the `CHANGE_TO_COMPOSE_DATA_PATH/Glance/` folder. Here is an example of the file:

```yaml
pages:
  - name: Home
    columns:
      - size: small
        widgets:
          - type: calendar

          - type: rss
            limit: 10
            collapse-after: 3
            cache: 3h
            feeds:
              - url: https://ciechanow.ski/atom.xml
              - url: https://www.joshwcomeau.com/rss.xml
                title: Josh Comeau
              - url: https://samwho.dev/rss.xml
              - url: https://awesomekling.github.io/feed.xml
              - url: https://ishadeed.com/feed.xml
                title: Ahmad Shadeed

          - type: twitch-channels
            channels:
              - theprimeagen
              - cohhcarnage
              - christitustech
              - blurbs
              - asmongold
              - jembawls

      - size: full
        widgets:
          - type: hacker-news

          - type: videos
            channels:
              - UCR-DXc1voovS8nhAvccRZhg # Jeff Geerling
              - UCv6J_jJa8GJqFwQNgNrMuww # ServeTheHome
              - UCOk-gHyjcWZNj3Br4oxwh0A # Techno Tim

          - type: reddit
            subreddit: selfhosted

      - size: small
        widgets:
          - type: weather
            location: London, United Kingdom

          - type: markets
            markets:
              - symbol: SPY
                name: S&P 500
              - symbol: BTC-USD
                name: Bitcoin
              - symbol: NVDA
                name: NVIDIA
              - symbol: AAPL
                name: Apple
              - symbol: MSFT
                name: Microsoft
              - symbol: GOOGL
                name: Google
              - symbol: AMD
                name: AMD
              - symbol: RDDT
                name: Reddit
```

## Gaming

---

Last update: Jan. 2025

Created: Jan. 2025
