# traefik-dot-files


## .env
Debes agregar tu API Global e Email de cloudflare, además si desea exponer el panel puedes agregar un subdominio base.

```
EMAIL=user@example.com
API_KEY=fe923499959349g9941h546
BASE_SUBDOMAIN=server1.example.com
```




## Panel
Si vas a exponer el panel debes proveer un usuario y contraseña en formato MD5, SHA1, o BCrypt. Los devs de traefik recomiendan la utilidad `htpasswd` incluida en apache2-utils (en la mayoria de distros)
Si deseas una forma simple y facil de obtener dichos requerimiento, usa el siguente comando. Remplaza <USER> y <PASS> dentro de `docker-compose.yml` con tus respectivos datos.

```
docker run --rm elswork/apache2-utils htpasswd -nb "<USER>" "<PASS>" | sed -e s/\\$/\\$\\$/g
```

```
    labels:
      - "traefik.enable=true"
      - "traefik.http.routers.traefik.entrypoints=http"
      - "traefik.http.routers.traefik.rule=Host(`traefik-dashboard.${BASE_SUBDOMAIN}`)"
      - "traefik.http.middlewares.traefik-auth.basicauth.users=<USER>:<PASS>"              < --- REMPLAZAR
      - "traefik.http.middlewares.traefik-https-redirect.redirectscheme.scheme=https"
      - "traefik.http.middlewares.sslheader.headers.customrequestheaders.X-Forwarded-Proto=https"
```



## Traefik.yml
Debes remplazar ${CF_API_EMAIL} con el asociado a tu cuenta de Cloudflare.

```
certificatesResolvers:
  cloudflare:
    acme:
      email: ${CF_API_EMAIL} <--- REMPLAZAR
      storage: acme.json
      dnsChallenge:
        provider: cloudflare
        resolvers:
          - "1.1.1.1:53"
          - "1.0.0.1:53"
```




