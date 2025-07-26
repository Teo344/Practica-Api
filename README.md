
# Creación de una API-REST para el manejo de categorías de productos

Mateo Criollo - 25/07/2025 - NRC: 2366


## Documentación

Este proyecto es una API REST desarrollada con **Spring Boot** que permite gestionar categorías de productos. Está diseñada para ejecutarse completamente mediante contenedores de Docker, utilizando **MySQL** como base de datos y con imágenes publicadas en mi Docker Hub personal.
## Tecnologías Utilizadas

- 🧩 Java 17 
- 🧩 Spring Boot
- 🧩 Spring Data JPA
- 🗃️ MySQL
- 🐋Docker
- 🐙 Docker Hub
- 🧩Jakarta



## 🔗 Links de los archivos creados
### 📁 Repositorio del código fuente
Para acceder de forma rápida y sencilla al codigo que permitió crear la API se encuentra aquí:
 - 🔗 [https://github.com/Teo344/Practica-Api](https://github.com/Teo344/Practica-Api)
### 📦 Imagen en Docker Hub
Para poder conocer donde se encuentra la imágen del servicio se encuentra aquí :
 - 🔗 [gabrielmt2004/repositoriocategoria:v4](https://hub.docker.com/r/gabrielmt2004/repositoriocategoria/tags)

## 📁 Estructura
```bash
categories/
├── .mvn/                # Archivos de configuración de Maven Wrapper
├── src/                 # Código fuente del proyecto
├── target/              # Archivos compilados generados (no subir al repositorio)
├── .gitattributes       # Configuración de atributos Git
├── .gitignore           # Archivos y carpetas que Git debe ignorar
├── Dockerfile           # Dockerfile para construir la imagen del contenedor
├── HELP.md              # Archivo de ayuda generado por Spring Initializr
├── mvnw                 # Script para usar Maven Wrapper (Linux/Mac)
├── mvnw.cmd             # Script para usar Maven Wrapper (Windows)
└── pom.xml              # Archivo de configuración de dependencias Maven
```
## FAQ

#### ¿Se utilizó otras dependencias a parte de las indicadas en clase?

Sí, en este caso se utilizo una nueva dependencia pero que se agrego directamente en el pom.xml del trabajo. Se utilizaron para asegurar que los datos enviados por los clientes cumplan ciertas reglas (como campos obligatorios o longitudes mínimas), se utiliza la validación con Jakarta Bean Validation y Hibernate Validator. Esta permite aplicar anotaciones directamente en los modelos de datos para verificar automáticamente que los valores cumplan con los requisitos definidos

```xml
		<dependency>
    <groupId>jakarta.validation</groupId>
    <artifactId>jakarta.validation-api</artifactId>
    <version>3.0.2</version>
		</dependency>
```

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-validation</artifactId>
</dependency>
```


#### ¿Comó se realizó las validaciones para los diferentes campos solicitados?
- name: obligatorio, único, longitud entre 3 y 50 caracteres.
- description : Sea opcional, longitud máxima 255 caracteres.

Con las dependencias ya mencionadas, logramos trabajar y utilizar anotaciones directamente en los modelos de datos para verificar automáticamente que los valores cumplan con los requisitos definidos. 
Como fue el caso de:

```bash
@NotBlank → Campo obligatorio, no puede estar 
vacío ni solo con espacios.

@NotNull → Campo obligatorio, no puede ser null.

@Size(min = x, max = y) → Define una longitud 
mínima y máxima (para String, listas, etc.).

@Email → Valida que el campo tenga un formato
 de correo válido.
```

Como el caso del atributo name:

```java
    @Column(unique = true, length = 50, nullable = false)
    @NotBlank(message = "El nombre es obligatorio")
    @Size(min = 3, max = 50, message = "El nombre debe tener entre 3 y 50 caracteres")
    private String name;
```

##  🛠️Despliegue de la base de datos y la imagen
### 🚀Creación de la base de MySQL

En este caso vamos primero con la creación del contenedor de MySQL y por consiguiente primero debemos crear una red para permitir la comunicación

```bash
docker network create test-network
```
Y luego levantamos el contedor del mismo:
```bash
docker run --name test-db-contenedor -e 
MYSQL_ROOT_PASSWORD=admin123 -e MYSQL_DATABASE=test 
-p 3307:3306 -d --network test-network MySQL
```


### 📤Creación de la imágen de la API
Para poder crear la imágen en donde se realizó la API se utilizó el siguiente comando, además de que esta fue la versión 4 que se subió
```bash
docker build -t img-app-categorias:v4 .
```
Y luego le denominamos un tag para poder enviar a Docker Hub
```bash
docker image tag img-app-categorias:v4 
 gabrielmt2004/repositoriocategoria:v4
```
Y al final los subimos a repositorio de Docker Hub
```bash
docker push gabrielmt2004/repositoriocategoria:v4
```




## 🚀Instalación

Para poder visualizar la API y verificar su fucionalidad debemos primero descargar la imágen:

```bash
docker pull gabrielmt2004/repositoriocategoria:v4
```
Y por ultimó consideramos la creación del contenedor de la API, la cual se configuró para desaparezca si es que se apaga la misma y evitar problemas de crear contenedores en exceso.

```bash
docker run --rm -it -p 8085:8004 
--name c-app-categorias --network 
test-network -e 
DB_HOST=test-db-contenedor:3306 -e 
PORT=8004 gabrielmt2004/repositoriocategoria:v4   
```
## 🌍 API Referencias

#### Obtener todos los datos

```http
  GET localhost:8085/api/categories
```

#### Obtener un ítem según la id

```http
  GET localhost:8085/api/categories/${id}
```
#### Insertar un objeto.

```http
  POST localhost:8085/api/categories
```

| Parameter | Type     | Description                       |
| :-------- | :------- | :-------------------------------- |
| `name`      | `string` | **Requerido. Unico**. Nombre de la categoría |
| `description`      | `string` | **Opcional**. Descripción de la categoría si se diese el caso |

#### Actualizar un campo de un dato.
```http
  PATCH localhost:8085/api/categories/${id}
```
| Parameter | Type     | Description                       |
| :-------- | :------- | :-------------------------------- |
| `name`      | `string` | **Requerido. Unico**. Nombre de la categoría |
| `description`      | `string` | **Opcional**. Descripción de la categoría si se diese el caso |

#### Eliminar un campo.
```http
  DELETE localhost:8085/api/categories/${id}
```
![Logo](https://upload.wikimedia.org/wikipedia/commons/thumb/4/44/Spring_Framework_Logo_2018.svg/1200px-Spring_Framework_Logo_2018.svg.png)


## 🛠️Soporte
Para poder solventar cualquier duda, comunicarse con el correo mgcriollo1@gmail.com

