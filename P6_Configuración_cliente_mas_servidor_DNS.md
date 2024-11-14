- Una vez el repositorio este montado voy a editar el archivo docker-compose.yml para hacer que el cliente utilice la imagen alpine.

cliente_dns:
image: alpine

No funciona ! No funciona ! No funciona ! No funciona !
- Defino un rango de ips en el archivo docker-compose.yml
config:
networks:
  bind9_subnet:
    driver: bridge
    ipam:
      config:
        - subnet: 172.28.5.0/24 # Rango de IPs de la red
No funciona ! No funciona ! No funciona ! No funciona !

- Defino un rago de ips desde la propia terminal:
docker network create bind9_subnet
docker network create \
  --driver=bridge \
  --subnet=172.28.0.0/16 \
  --ip-range=172.28.5.0/24 \
  --gateway=172.28.5.254 \
  bind9_subnet

Cosas a tener encuenta:
Bajo ningun concepto puede haber otra red utilizando la misma ip
Antes de borrar una red hay que hacer un docker down
al definir el rango de ips el nombre tiene que ser el que le hayamos asignado nosotros en el comando network create
Si tenemos demasiadas redes sin usar lo mejor es utilizar docker network prune

- Continuo iniciando el repositorio con docker-compose up -d
Despues de mucho sufrimiento funciona perfectamente

- Por ultimo queda instalar dig y comprobar su funcionamiento
Lamentablememnte esta ocurriendo un error externo donde ni yo ni el profesor pueden solucionar:
broken trust chain resolving 'dl-cdn.alpiasir_bind9
Intentare probar otro dia pero si no se arregla tendre que dejar la practica así
14/11/24 21:22
Voy a continuar con la instalacion de dig cambiando la imagen de alpine con la de ubuntu porque me da demasiados problemas:
- Primero encendemos el contendor y en otra terminal ejecutamos el siguiente comando para conectarnos:
$ docker exec -it asir2_bind9 bash
- Una vez dentro vamos a actualizar y a instalar la herramienta dig:
$ apt update
$ apt install -y dnsutils
- Por ultimo comprobamos el funcionamiendo del comando
$ dig google

; <<>> DiG 9.18.28-0ubuntu0.24.04.1-Ubuntu <<>> google
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: SERVFAIL, id: 1235
;; flags: qr rd; QUERY: 1, ANSWER: 0, AUTHORITY: 0, ADDITIONAL: 0
;; WARNING: recursion requested but not available

;; QUESTION SECTION:
;google.            	IN	A

;; Query time: 1 msec
;; SERVER: 127.0.0.11#53(127.0.0.11) (UDP)
;; WHEN: Thu Nov 14 20:31:34 UTC 2024
;; MSG SIZE  rcvd: 24





