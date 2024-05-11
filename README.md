# arrappstack
A docker compose file that includes sonarr, radarr, qbittorrent, jackett, and binds them all to gluetun. Readme is currently a WIP. 

## Things to note:
1) The taxonomy of the folders within the application stack is made to support [hardlinking](https://trash-guides.info/Hardlinks/Hardlinks-and-Instant-Moves/) in sonarr and radarr. The folders will need to be created in your host first as follows below. Please make sure the permissions are appropriate for these folders.


``` 
 /datastore
  /data
    /torrents
      /movies
      /series
    /media
      /movies
      /series
```
2) You may need to reference [gluetun](https://github.com/qdm12/gluetun#setup) documentation for your VPN configuration within the docker-compose.yml.

3) Once the applications are running, you will need to configure the remote path mappings for the downloads within sonarr and radarr. The remote path within the qbittorrent application is `/downloads` and you need to map it to the local path `/data/torrents`.

![2023-05-13 15_12_21-Window](https://github.com/rsmsctr/arrappstack/assets/95463866/60a288ed-4f0a-482a-90a2-0780ba350bc2)

4) All ports to sonarr, radarr, qbittorrent, and jackett are routed through gluetun which is why these ports are bound to the host within the gluetun service configuration.

5) gluetun has a killswitch, which means that the compose file will not run if gluetun does not establish a connection to a vpn service.

5) You can confirm that qbittorrent traffic is being routed through gluetun by entering the container with `docker exec -it qbittorrent /bin/bash`. Once you are within the qbittorrent container run `wget -qO- https://ipinfo.io`. A json should be returned with an IP that is not yours. 
