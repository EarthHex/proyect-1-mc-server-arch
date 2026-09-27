````
# HomeLab Infrastructure: Multi-Instance Minecraft Services
Infraestructura de homelab para ejecutar y monitorizar varias instancias de servidores de Minecraft mediante Docker Compose.
El proyecto fue desplegado sobre un servidor físico con Arch Linux y utiliza contenedores para separar los servicios, mantener los datos persistentes y facilitar la administración del entorno.
## Estado del proyecto
**Finalizado como proyecto de aprendizaje.**
Este repositorio representa una primera versión funcional de una infraestructura personal para servidores de Minecraft. No se encuentra en desarrollo activo y no pretende ser una solución universal ni preparada para producción.
El objetivo principal fue aprender y practicar:
- Docker y Docker Compose.- Administración básica de servicios Linux.- Despliegue de varias instancias.- Persistencia de datos mediante volúmenes.- Monitorización con Prometheus y Grafana.- Observabilidad de contenedores.- Documentación de incidentes.
## Arquitectura
El entorno está dividido en tres áreas principales:
### Servicios de Minecraft
Se ejecutan varias instancias independientes dentro de contenedores.
Entre las instancias utilizadas se encuentran:
- Servidores modificados de Minecraft Java Edition.- Instancias de Minecraft Bedrock Edition.- Modpacks como Dawncraft y Divine Journey 2.
Cada instancia utiliza su propio directorio de datos para conservar mundos, configuraciones y archivos generados por el servidor.
### Red y exposición
Los servicios se conectan mediante una red interna de Docker y exponen únicamente los puertos necesarios hacia el host.
La infraestructura original también utilizaba un servicio de DNS dinámico para mantener accesible el servidor desde Internet cuando cambiaba la dirección IP pública.
### Monitorización
El proyecto incluye una pequeña pila de observabilidad compuesta por:
- Prometheus para recopilar métricas.- Grafana para visualizar información.- cAdvisor para obtener métricas de los contenedores.
La monitorización permite observar principalmente:
- Uso de CPU.- Uso de memoria.- Estado de los contenedores.- Uso de almacenamiento.- Rendimiento general del host.
## Estructura del repositorio
```text.├── .gitignore├── README.md├── docker-compose.yml├── prometheus.yml└── post-mortems/    └── INCIDENT-001.md
````

Los directorios de datos de las instancias de Minecraft no se incluyen en el repositorio porque contienen mundos, configuraciones, logs y archivos generados durante la ejecución.

Entre los directorios utilizados por la infraestructura se encuentran:

text

```
dawncraft/divine\_journey2/y-0/y-1/
```

Estos directorios deben existir localmente en el servidor donde se despliegue el proyecto.

## Requisitos

Para ejecutar el proyecto se necesita:

- Un sistema Linux.
- Docker Engine.
- Docker Compose.
- Suficiente memoria RAM para las instancias de Minecraft.
- Espacio de almacenamiento para los mundos y archivos de los servidores.
- Puertos disponibles para Minecraft y las herramientas de monitorización.

El proyecto fue desarrollado y utilizado principalmente sobre Arch Linux, aunque Docker Compose permite que la configuración pueda adaptarse a otras distribuciones Linux.

## Despliegue

Clona el repositorio:

bash

```
git clone https://github.com/EarthHex/proyect-1-mc-server-arch.gitcd proyect-1-mc-server-arch
```

Crea los directorios locales necesarios:

bash

```
mkdir -p dawncraftmkdir -p divine\_journey2mkdir -p y-0mkdir -p y-1
```

Revisa el archivo `docker-compose.yml` y ajusta, si es necesario:

- Puertos.
- Versiones de Minecraft.
- Modpacks.
- Memoria asignada.
- Rutas de almacenamiento.
- Variables de entorno.

Después, inicia los servicios:

bash

```
docker compose up -d
```

Comprueba el estado de los contenedores:

bash

```
docker compose ps
```

Para consultar los logs:

bash

```
docker compose logs -f
```

Para consultar los logs de un servicio concreto:

bash

```
docker compose logs -f nombre-del-servicio
```

Para detener la infraestructura:

bash

```
docker compose down
```

## Configuración

La configuración principal se encuentra en:

text

```
docker-compose.ymlprometheus.yml
```

El archivo `docker-compose.yml` define:

- Las instancias de Minecraft.
- Los servicios de monitorización.
- Las redes.
- Los puertos.
- Los volúmenes.
- Las variables de entorno.

El archivo `prometheus.yml` define los objetivos desde los que Prometheus obtiene métricas.

Las configuraciones privadas y los datos generados por los servidores se mantienen fuera del control de versiones mediante `.gitignore`.

## Persistencia de datos

Los datos de cada servidor se almacenan en directorios separados.

Esto permite conservar:

- Mundos.
- Configuraciones.
- Logs.
- Mods.
- Plugins.
- Archivos generados por los contenedores.

Los datos reales no se incluyen en este repositorio porque pueden ocupar mucho espacio y pertenecen a la infraestructura local.

Antes de modificar versiones, eliminar contenedores o actualizar modpacks, es recomendable realizar una copia manual de los directorios de datos.

## Monitorización

Los servicios de monitorización permiten consultar el estado general de la infraestructura.

La pila está compuesta por:

text

```
Minecraft containers        │        ├── cAdvisor        │        └── Prometheus                │                └── Grafana
```

Prometheus recopila las métricas disponibles y Grafana se utiliza para visualizarlas mediante paneles.

Las direcciones y puertos exactos dependen de la configuración definida en `docker-compose.yml`.

## Seguridad

Durante el despliegue original se aplicaron algunas medidas básicas:

- Uso de una instalación minimalista de Arch Linux.
- Firewall con puertos limitados.
- Acceso SSH mediante llaves.
- Desactivación del acceso SSH por contraseña.
- Exclusión de credenciales y archivos sensibles mediante `.gitignore`.
- Separación de servicios mediante contenedores.

Estas medidas forman parte de la configuración del entorno original y no todas están automatizadas por este repositorio.

El repositorio no debe considerarse una configuración de seguridad completa. Cada usuario debe revisar su firewall, sus credenciales, sus puertos expuestos y sus reglas de acceso antes de utilizar una configuración similar en Internet.

## Limitaciones conocidas

Este proyecto tiene varias limitaciones:

- No incluye un sistema automatizado de copias de seguridad.
- No incluye un procedimiento automatizado de restauración.
- No automatiza la instalación completa de Arch Linux.
- No automatiza toda la configuración del firewall.
- No automatiza la configuración de SSH.
- No incluye todos los datos de los servidores.
- No garantiza compatibilidad con todas las versiones de Minecraft.
- Algunas configuraciones dependen del servidor físico original.
- La documentación no cubre todos los errores posibles.
- La configuración de recursos debe ajustarse manualmente a cada equipo.
- No está diseñado como una plataforma de producción.

Estas limitaciones forman parte del alcance de esta primera versión.

## Incidentes y aprendizaje

La carpeta `post-mortems/` contiene documentación de incidentes ocurridos durante el desarrollo o la ejecución de la infraestructura.

Su objetivo es registrar:

- Qué ocurrió.
- Qué servicios fueron afectados.
- Cómo se detectó el problema.
- Cuál fue la causa.
- Cómo se resolvió.
- Qué aprendizaje dejó el incidente.

Este registro forma parte del proceso de aprendizaje y mejora del proyecto.

## Objetivos cumplidos

Con este proyecto se consiguió:

- Ejecutar varias instancias de Minecraft mediante contenedores.
- Separar los servicios dentro de una red de Docker.
- Mantener los datos de cada servidor en directorios persistentes.
- Desplegar una pila básica de observabilidad.
- Recopilar métricas con Prometheus.
- Visualizar métricas mediante Grafana.
- Monitorizar recursos de los contenedores.
- Practicar administración de Linux y Docker.
- Documentar un incidente real o simulado mediante un post-mortem.

## Próximas mejoras que no forman parte de este proyecto

Este repositorio no seguirá creciendo por el momento. Algunas mejoras posibles para una futura versión serían:

- Copias de seguridad automatizadas.
- Restauración probada.
- Alertas de Prometheus.
- Dashboards versionados de Grafana.
- Automatización del host mediante Ansible.
- Validación automática de la configuración.
- Mejor gestión de secretos.
- Procedimientos de actualización y rollback.

Estas ideas quedan fuera del alcance de esta primera versión y podrían formar parte de un proyecto posterior.
