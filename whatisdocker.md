# Que es Docker?

### Primero, caso de uso

- Imagina que estas armando un proyecto con un equipo
- Supongamos que este proyecto usa una version especifica de python
- Cada miembro del equipo tiene que configurar todo a mano (dependencias, variables de entorno)
- Esto trae una cantidad variada de problemas
  - "A vos no te anda, en mi pc anda bien"
  - Si tenemos varios proyectos tenemos que invertir mucho tiempo en configurarlos

### Para poder evitar esto

### Usamos Containers

Una definicion basica y por arriba de que es un container es "un container es una carpeta o un paquete con todo lo que tu aplicacion necesita para funcionar"

Gracias a un container no nos tenemos que preocupar por la configuracion del proyecto, ya que todo lo que necesita esta dentro del container

Lo unico que necesitariamos es algo para manipular y administrar estos containers, que es donde aparece

## Docker

Docker es/tiene/puede:

- Tiene una gran comunidad por lo que se cuenta con herramientas y librerias disponibles (dockerhub)
  - Una herramienta para administrar containers
  - Una de las herramientas fundamentales en el area de DevOps
  - Facilita los procesos de CI/CD al proveer entornos para cada stage/etapa?
  - los containers usan menos recursos que una maquina virtual (despues se amplia)
  - Las aplicaciones corren igual sin importar en que maquina esten (OS agnostic)
  - Usa menos espacio ya que usa un file system basado en layers (se amplia despues)
  - ...

# Como se instala Docker?

aca va la guia de la documentacion oficial
terminala con mostrar el docker run hello world para ver que anda bien

# Ya muestra el maldito container

Antes de llegar al container vamos con el dockerfile hace la imagen de la imagen sale el container
Dockerfile ------> Imagen --------> Container

El dockerfile es un archivo de texto (yaml) que tiene las instrucciones de como se va a hacer la imagen, es como armar un molde

(mostra el comando docker build)

la imagen seria el molde, de la imagen sale un container (puede salir mas de uno)

(mostra docker run)

# Anatomia de un Dockerfile

TODO EXPLICAR ESTO

FROM node:0.10
MAINTAINER Anna Doe <anna@example.com>
LABEL "rating"="Five Stars" "class"="First Class"
USER root
ENV AP /data/app
ENV SCPATH /etc/supervisor/conf.d
RUN apt-get -y update

#### The daemons

RUN apt-get -y install supervisor
RUN mkdir -p /var/log/supervisor

#### Supervisor Configuration

ADD ./supervisord/conf.d/\* $SCPATH/

#### Application Code

ADD _.js_ $AP/
WORKDIR $AP
RUN npm install
CMD ["supervisord", "-n"]

TODO
CONTAINERS VS VIRTUAL MACHINES

- por que usa menos recursos (explicar cgroups)
- como se aislan los procesos (explicar kernel namespaces)
