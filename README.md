## swagger-docker-compose
### Resumen
Repositorio docker forkeado de https://github.com/elevennines-inc/swagger-all-in-one-docker-compose

La idea es la de juntar en un mismo docker-compose todos los servicios de Swagger libres 
### Cómo se ejecuta
```
docker-compose up -d

Si miras con el comando
docker-compose ps
         Name                       Command               State           Ports
----------------------------------------------------------------------------------------
swagger-api      /usr/local/bin/apisprout / ...   Up      0.0.0.0:8083->8000/tcp
swagger-editor   sh /usr/share/nginx/docker ...   Up      0.0.0.0:8081->8080/tcp
swagger-nginx    nginx -g daemon off;             Up      80/tcp, 0.0.0.0:8084->8084/tcp
swagger-ui       sh /usr/share/nginx/docker ...   Up      0.0.0.0:8082->8080/tcp
```

### Cómo utilizar
1. Edita con el editor de SwaggerEdit en la url [http://localhost:8081/]
2. Guarda el fichero de especificación de Swagger con el menú File
3. Mueve y guarda el fichero json en `swagger/openapi.json`
4. Ejecuta `docker-compose restart` y se actualizarán el swagger-ui y swagger-api(servidor de prueba)
5. También se puede importar un fichero openapi.json, desde el editor desde el menú `File > Import File`.

### Heads-up
- El UI o la presentación de la documentación se referencia a `http://localhost:8082/`, ó`http://127.0.0.1:8082/`
- Servira el fichero openapi.json, si falla es que no lo encontrará correctamente desde el contenedor. Utiliza `docker logs` para ver que pasa y arreglarlo.
- Si quieres acceder a swagger-api desde otros dominios (CORS), utiliza el contenedor de swagger-nginx como proxy.

### swagger-editor
- Puedes editar el fichero de especificación al vuelo
- Puede exportarse a json y yaml. swagger-ui puede leer estos fichero para presentarlos de manera bonita como documentación.

### swagger-ui
- Puede presentar la documentación asociada a la especificación
- La especificación de Swagger puede asignarse desde fichero json  o con la ruta API_URL
```
environment:
  SWAGGER_JSON: /openapi.json
  # API_URL: ""
```
- ./swagger/openapi.json se referencia en el repositorio

### swagger-api(apisprout)
- Utiliza [apisprout](https://github.com/danielgtaylor/apisprout) en un contenedor docker.
- Swagger es compatible con el estándar `openapi: 3.x`.
- ./swagger/openapi.json está en el repositorio.
- De todos modos, apisprout no puede añadir `Access-Control-Allow-Origin` a la cabecera por lo que se puede poner un servidor nginx como frontend de swagger-api, accesible por CORS.
