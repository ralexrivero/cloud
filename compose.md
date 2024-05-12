# Guía de Docker Compose

Docker Compose es una herramienta para definir y ejecutar aplicaciones Docker multi-contenedor. Mediante un archivo YAML para configurar los servicios se crean e inician todos los servicios desde esa configuración.

## Instalación de Docker y Docker Compose

### Instalación de Docker

Para instalar Docker en Debian, sigue estos pasos:

1. Actualiza los paquetes del sistema:

    ```bash
    sudo apt update && sudo apt -y full-upgrade
    ```

2. Instala las dependencias necesarias:

    ```bash
    sudo apt install curl gnupg2 apt-transport-https software-properties-common ca-certificates
    ```

3. Si es necesario, reinicia el sistema:

    ```bash
    [ -f /var/run/reboot-required ] && sudo reboot -f
    ```

4. Añade la clave GPG oficial de Docker:

    ```bash
    curl -fsSL https://download.docker.com/linux/debian/gpg | sudo gpg --dearmor -o /etc/apt/trusted.gpg.d/docker-archive-keyring.gpg
    ```

5. Añade el repositorio de Docker:

    ```bash
    echo "deb [arch=amd64] https://download.docker.com/linux/debian bullseye stable" | sudo tee /etc/apt/sources.list.d/docker.list
    ```

6. Actualiza los paquetes e instala Docker:

    ```bash
    sudo apt update
    sudo apt install docker-ce docker-ce-cli containerd.io
    ```

### Instalación de Docker Compose

Para instalar Docker Compose, puedes seguir estos pasos:

1. Descarga la versión más reciente de Docker Compose:

    ```bash
    sudo curl -L "https://github.com/docker/compose/releases/download/$(curl -s https://api.github.com/repos/docker/compose/releases/latest | grep tag_name | cut -d '"' -f 4)/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
    ```

2. Aplica permisos ejecutables al binario descargado:

    ```bash
    sudo chmod +x /usr/local/bin/docker-compose
    ```

3. Verifica la instalación:

    ```bash
    docker-compose --version
    ```

## Creación de Archivos docker-compose.yml

### Estructura Básica

Un archivo `docker-compose.yml` es un archivo de configuración en formato YAML que define los servicios, redes y volúmenes para una aplicación Docker.

```yaml
version: '3'
services:
  nombre_del_servicio:
    image: nombre_de_la_imagen
    ports:
      - "80:80"
    environment:
      VARIABLE: valor
```

### Ejemplo: Un Solo Contenedor

Para una aplicación simple con un solo contenedor:

```yaml
version: '3'
services:
  web:
    image: nginx
    ports:
      - "8080:80"
```

En este ejemplo, Docker Compose ejecutará un contenedor Nginx y mapeará el puerto 80 del contenedor al puerto 8080 del host.

### Ejemplo: Varios Contenedores

Para una aplicación con varios contenedores:

```yaml
version: '3'
services:
  web:
    build: ./web
    ports:
      - "8080:80"
    depends_on:
      - db

  db:
    image: postgres
    environment:
      POSTGRES_PASSWORD: ejemplo
```

En este ejemplo, Docker Compose construirá la imagen para el servicio `web` desde el directorio `./web` y ejecutará un contenedor Postgres como servicio `db`. El contenedor `web` depende del contenedor `db`.

## Comandos Principales de Docker Compose

### Ejecutar Servicios

Para iniciar todos los servicios definidos en el archivo `docker-compose.yml`:

```bash
docker-compose up
```

Este comando construirá, creará e iniciará todos los contenedores.

### Detener Servicios

Para detener los servicios:

```bash
docker-compose down
```

Este comando detendrá y eliminará todos los contenedores, redes y volúmenes definidos en el archivo `docker-compose.yml`.

### Otros Comandos Útiles

- `docker-compose build`: Construye las imágenes de los servicios.
- `docker-compose start`: Inicia los servicios detenidos.
- `docker-compose stop`: Detiene los servicios en ejecución.
- `docker-compose restart`: Reinicia los servicios.
- `docker-compose logs`: Muestra los registros de los servicios.

## Variables en Docker Compose

### Variables de Entorno

Las variables de entorno se pueden definir en el archivo `docker-compose.yml` y se pasan a los contenedores:

```yaml
services:
  web:
    image: nginx
    environment:
      - MI_VARIABLE=valor
```

### Argumentos de Construcción

Los argumentos de construcción (build arguments) se utilizan durante la construcción de la imagen:

```yaml
services:
  web:
    build:
      context: .
      args:
        MI_ARG: valor
```

### Más definiciones básicas del archivo docker-compose.yml

Archivo `docker-compose.yml` con mas secciones: `version`, `services`, `volumes`, `networks`, y `configs`. La sección más importante y común es `services`, esta define los contenedores que compondrán la aplicación.

### Definición de Servicios

Cada servicio en el archivo representa un contenedor.

```yaml
version: '3'
services:
  nombre_del_servicio:
    image: nombre_de_la_imagen:tag  # Especifica la imagen y la etiqueta del contenedor
    container_name: nombre_contenedor  # Opcional: Define el nombre del contenedor
    ports:
      - "host_port:container_port"  # Mapea los puertos del host al contenedor
    volumes:
      - /ruta/host:/ruta/contenedor  # Mapea los volúmenes del host al contenedor
    environment:
      - VARIABLE=valor  # Define variables de entorno
    user: usuario  # Opcional: Especifica el usuario bajo el cual se ejecutará el contenedor
    stdin_open: true  # Opcional: Mantiene stdin abierto (equivalente a -i)
    tty: true  # Opcional: Asigna un terminal pseudo-TTY (equivalente a -t)
    restart: "no"  # Opcional: Política de reinicio, "no" indica que no se reiniciará automáticamente
    depends_on:
      - otro_servicio  # Opcional: Define dependencias entre servicios
```

### Ejemplo

```yaml
version: '3'
services:
  web:
    image: nginx:latest
    container_name: web_server
    ports:
      - "8080:80"
    volumes:
      - ./html:/usr/share/nginx/html
    environment:
      - NGINX_HOST=localhost
    restart: always
    depends_on:
      - db

  db:
    image: postgres:13
    container_name: db_server
    volumes:
      - db_data:/var/lib/postgresql/data
    environment:
      - POSTGRES_PASSWORD=ejemplo
    restart: always

volumes:
  db_data:
```

### Explicación de la Configuración

1. **version**: Especifica la versión del formato de archivo de Docker Compose.
2. **services**: Define los servicios (contenedores) que componen la aplicación.
3. **web**: Un servicio que utiliza la imagen `nginx:latest`.
    - `container_name`: Nombra el contenedor como `web_server`.
    - `ports`: Mapea el puerto 80 del contenedor al puerto 8080 del host.
    - `volumes`: Mapea el directorio `./html` del host al directorio `/usr/share/nginx/html` del contenedor.
    - `environment`: Define una variable de entorno `NGINX_HOST`.
    - `restart`: Configura el contenedor para que se reinicie siempre.
    - `depends_on`: Indica que este servicio depende del servicio `db`.
4. **db**: Un servicio que utiliza la imagen `postgres:13`.
    - `container_name`: Nombra el contenedor como `db_server`.
    - `volumes`: Mapea un volumen llamado `db_data` al directorio `/var/lib/postgresql/data` del contenedor.
    - `environment`: Define una variable de entorno `POSTGRES_PASSWORD`.
    - `restart`: Configura el contenedor para que se reinicie siempre.
5. **volumes**: Define volúmenes que pueden ser utilizados por los servicios.

### Comandos de Docker Compose

1. **Iniciar servicios**: Inicia todos los servicios definidos en el archivo `docker-compose.yml`.

    ```bash
    docker-compose up
    ```

2. **Detener servicios**: Detiene y elimina todos los contenedores, redes y volúmenes creados por `up`.

    ```bash
    docker-compose down
    ```

3. **Ejecutar comandos en un contenedor en ejecución**: Ejecuta un comando dentro de un contenedor en ejecución.

    ```bash
    docker-compose exec nombre_del_servicio comando
    ```
