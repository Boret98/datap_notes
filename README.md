# datap_notes
## KRAKEND
El archivo Dockerfile de "github.com/iloob/krakend-keycloak" tenia una errata que no dejaba darle permisos al usuario 'krakend' - RUN chown krakend -R /etc/krakend
En 'krakend-keycloak/backend/code/requirements.txt' hay que añadir las siguientes lineas:
    python-jose
    social-auth-core

A la hora de coger el token, un POST en "https://localhost:443/auth/realms/master/protocol/openid-connect/token"
Se le tienen que pasar argumentos en el body (x-www-form-urlencoded):
    - client_id
    - grant_type
    - client_secret
    - scope
    - username
    - password

Para que krakend proteja el endpoint, en krakend.json, en el campo "backend" del endpoint, poner el NOMBRE del servicio docker en vez de localhost.
Ejemplo:
        "backend": [
          {
            "url_pattern": "/v2/{request}",
            "encoding": "no-op",
            "method": "GET",
            "host": [
              "http://orion-fw:1026"
            ]
          }
        ]
        
        
## OAUTH2
En el docker-compose, para el servicio oauth2, se le tiene que activar SIEMPRE la flag "restart: unless-stopped", por algun motivo se intenta lanzar antes de que el keycloak se acabe de lanzar, pese a haber usado la flag "depends_on" lo que da un error.
