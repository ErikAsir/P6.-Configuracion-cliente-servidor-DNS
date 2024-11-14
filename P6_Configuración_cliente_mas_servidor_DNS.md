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

