****Docker network**<br><br>

**1º Crear unha rede en docker**<br><br>
Co comando `docker network create rede` creamos a red que queramos co nombre que queramos.<br>
No meu caso a rede que e creado e SRI.
<br><br>

**2º Crear dous contenedores unidos a esa rede**<br><br>
Para crear os contenederos diretamente unidos a esa rede utilizaremos o comando `docker run -dit --name contenedorX --network rede ubuntu`.<br>
No meu caso o contenedor e contenedor1 e contenedor2 e a rede anteriormente creada SRI.
<br><br>

**3º Comprobar que os contenedores están na rede**<br><br>
Para comprobar que os contenedores están na rede so necesitamos un comando sinselo `docker network inspect SRI`.
        "ConfigOnly": false,<br>
        "Containers": <br>
            "369d0a8734a097a5807256057d1739e5726601c597a47dae1a74356ce44481f6": {<br>
                "Name": "contenedor1",<br>
                "EndpointID": "36033ba87e31167de3b126a0dd37e265c620d5bb7ccb7edec9c9dba03336c65f",<br>
                "MacAddress": "02:42:ac:12:00:02",<br>
                "IPv4Address": "172.18.0.2/16",<br>
                "IPv6Address": ""<br>
            },<br>
            "7016c2142da39accf256270c530563345cf96cfe436616a5f7c644f68d3d3534": {<br>
                "Name": "contenedor2",<br>
                "EndpointID": "0f01767c59c6c535a07d4ce42d8f08e10d11d506a4e39903013d9512e5ef043e",<br>
                "MacAddress": "02:42:ac:12:00:03",<br>
                "IPv4Address": "172.18.0.3/16",<br>
                "IPv6Address": ""<br>
            }<br><br>
Neste caso podemos comprobar que os dos contenedores pertecen a rede SRI.
<br><br>

**4º Comprobar que os contenedores poden verse entre eles**<br><br>
Para ver que os contenedores poden verse entre eles vamos a utilizar o comando `docker attach contenedor1` para entrar en el contenedor, acontinuación utilizamos o comando `apt install iputils-ping` para que podamos facer ping co contenedor (en caso de non poder simplemente actualizamos a máquina co comando `apt update`).
Por ultimo simplemente facemos o comando `ping contenedor2`<br><br>
root@ad8249f58152:/# ping contenedor2<br>
PING contenedor2 (172.18.0.3) 56(84) bytes of data.<br>
64 bytes from contenedor2.SRI (172.18.0.3): icmp_seq=1 ttl=64 time=0.165 ms<br>
64 bytes from contenedor2.SRI (172.18.0.3): icmp_seq=2 ttl=64 time=0.126 ms<br>
--- contenedor2 ping statistics ---<br>
2 packets transmitted, 2 received, 0% packet loss, time 1001ms<br>
rtt min/avg/max/mdev = 0.126/0.145/0.165/0.019 ms<br>
<br><br>

**5º Listar os contenedores conectados á rede**<br><br>
Co comando `docker network inspect SRI` ao igual que no punto 3 podemos ver os contenedores conectados.<br><br>
        "Containers": {<br>
            "460edcfa4fa5e943856d226dce9167300c80f980eb4bada0c961120af9d29d5f": {<br>
                "Name": "contenedor2",<br>
                "EndpointID": "dc2e197616eb5cb0da3435f431056d74fd47fdd8f9c2ad03a3f704b7bf0cfcb0",<br>
                "MacAddress": "02:42:ac:12:00:03",<br>
                "IPv4Address": "172.18.0.3/16",<br>
                "IPv6Address": ""<br>
            }<br>
        },<br><br>
No meu caso ao entrar con attach no punto 4 e sair do contenedor se cerra pero podemos abrilo de novo e volveriase a conectar automaticamente a rede SRI.
<br><br>

**6º Listar as propiedades da rede**<br><br>
Co mesmo comando que nos puntos 3 e 5 `docker network inspect SRI` vemos as propiedades da rede.<br><br>
        "Name": "SRI",<br>
        "Id": "af49778d1b03495a6c341ad7a1538ab327b5dce125732bf9c4ef4c5a33baee73",<br>
        "Created": "2024-10-22T18:22:59.658714828+02:00",<br>
        "Scope": "local",<br>
        "Driver": "bridge",<br>
        "EnableIPv6": false,<br>
        "IPAM": {<br>
            "Driver": "default",<br>
            "Options": {},<br>
            "Config": [<br>
                {<br>
                    "Subnet": "172.18.0.0/16",<br>
                    "Gateway": "172.18.0.1"<br>
                }<br>
            ]<br>
        },<br>
        "Internal": false,<br>
        "Attachable": false,<br>
        "Ingress": false,<br>
        "ConfigFrom": {<br>
            "Network": ""<br>
    }<br>
<br><br>

**7º Crea outra rede**<br><br>
Co comando do punto 1 `docker network create rede` creamos outra rede que no meu caso chamase SRI2.
<br><br>

**8º Lanza dous contenedores novos conectados a esa nova rede**<br><br>
Ao igual que no punto 2 `docker run -dit --name contenedorX --network SRI2 ubuntu` no meu caso contenedor3 e contenedor4.
<br><br>

**9º Comproba as posibles conexións entre os 4 contenedores**<br><br>
Neste caso so se poden ver entre contenedor1 e contenedor2, e poden verse entre contenedor3 e contenedor4 entre eles, pero non poden verse 1 e 2 con 3 e 4 devido a que estan en redes separadas.
<br><br>

**10º Docker compose:**<br><br>

Segue os pasos da guía de iniciación de docker-compose, e explica coas túas palabras os pasos que segues e qué fan<br>

Agora que sabes algo máis de docker-compose, crea un arquivo (ou varios arquivos) de configuración que ó ser lanzados cun docker-compose up, resulten nunha rede docker á que estean conectados 3 contenedores, explica os parámetros do .yaml usado<br>

Busca e proba 4 parámetros e configuracións diferentes que podes incluir no arquivo compose, explica qué fan. (por exemplo diferentes cousas que facer coa opción RUN).<br><br>

Empezamos creando o arquivo `docker-compose.yml` con `touch` e modificamolo con `nano`.
Dentro configuramos o arquivo.<br><br>

<u>Está e una rede con 3 contenedores.<u><br><br>

version: '3.8'<br>
services:<br>
  contenedor1:<br>
    image: ubuntu<br>
    networks:<br>
      - SRI2<br><br>

  contenedor2:<br>
    image: ubuntu<br>
    networks:<br>
      - SRI2<br><br>

  contenedor3:<br>
    image: ubuntu<br>
    networks:<br>
      - SRI2<br><br>

networks:<br>
  SRI2:<br>
    driver: bridge<br><br>

version: '3.8': Defina a versión do Docker Compose.<br>
services: Aquí definese os contenedores (en este caso, tres).<br>
image: ubuntu: Indica os contenedores que usarán a imaxe ubuntu.<br>
networks: Especifica que os contenedores deben conectarse a red SRI2.<br>
networks: SRI2: Define unha rede personalizada co controlador bridge.<br><br>

Utilizamos o comando `docker-compose up` para iniciar o arquivo coa configuración.<br><br>

**Outras tres configuracions:**<br><br>

<u>Conecta un volumen para compartir os datos entre o host e o contenedor.<u><br>
services:<br>
  contenedor1:<br>
    image: ubuntu<br>
  contenedor2:<br>
    image: ubuntu<br>
    depends_on:<br>
      - contenedor1<br><br>

<u>Define as variables de entorno para os contenedores.<u><br>
services:<br>
  contenedor1:<br>
    image: ubuntu<br>
    volumes:<br>
      - ./datos:/datos<br><br>

<u>Mapea os portos do host ao contenedor.<u><br>
services:<br>
  contenedor1:<br>
    image: nginx<br>
    ports:<br>
      - "8080:80"<br>
