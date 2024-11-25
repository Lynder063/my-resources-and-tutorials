## jellyfin jellyserr sonarr radarr prowlerr flaresolverr qbittorrent i2pd
```yml
services:
  jellyseerr:
    image: fallenbagel/jellyseerr:latest
    container_name: jellyseerr
    environment:
      - LOG_LEVEL=debug
      - TZ=Europe/Prague
      - PORT=5055
    ports:
      - 5055:5055
    volumes:
      - /opt/media-stack/jellyseerr:/app/config
    restart: unless-stopped
    networks: 
      - media-network

  radarr:
    image: lscr.io/linuxserver/radarr:latest
    container_name: radarr
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Europe/Prague
    volumes:
      - /opt/media-stack/radarr/data:/config
      - /media/media/movies:/movies
      - /opt/media-stack/downloads:/downloads
    ports:
      - 7878:7878
    restart: unless-stopped
    networks:
      - media-network
  sonarr:
    image: lscr.io/linuxserver/sonarr:latest
    container_name: sonarr
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Europe/Prague
    volumes:
      - /opt/media-stack/sonarr/data:/config
      - /media/media/shows:/tv
      - /opt/media-stack/downloads:/downloads
    ports:
      - 8989:8989
    restart: unless-stopped
    networks:
      - media-network

  prowlarr:
    image: lscr.io/linuxserver/prowlarr:latest
    container_name: prowlarr
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Europe/Prague
    volumes:
      - /opt/media-stack/prowlarr/data:/config
    ports:
      - 9696:9696
    restart: unless-stopped
    networks:
      - media-network

  flaresolverr:
    image: ghcr.io/flaresolverr/flaresolverr:latest
    container_name: flaresolverr
    ports:
      - "8191:8191"
    environment:
      - LOG_LEVEL=info
    restart: unless-stopped
    networks:
      - media-network

  qbittorrent:
    image: lscr.io/linuxserver/qbittorrent:latest
    container_name: qbittorrent
    privileged: true
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Europe/Prague
      - WEBUI_PORT=8080
    volumes:
      - /opt/media-stack/qbittorrent/config:/config
      - /opt/media-stack/downloads:/downloads
    ports:
      - 8080:8080
      - 6881:6881
      - 6881:6881/udp
    restart: unless-stopped
    networks:
      - media-network

  i2pd:
    image: purplei2p/i2pd
    container_name: i2pd
    volumes:
      - /opt/media-stack/i2pd/i2pd.conf:/home/i2pd/data/i2pd.conf
    ports:
      - 7070:7070
    restart: unless-stopped
    networks:
      - media-network

networks: 
  media-network: 
    driver: bridge
```
