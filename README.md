# 📻 Gestión de Eventos de Emisora de Radio — Java EE
<p align="center">
  <img src="screenshot.png" alt="Captura del proyecto" width="600">
</p>

Aplicación web desarrollada en Java EE diseñada para la gestión de eventos musicales de una emisora de radio. Incluye sistema de autenticación de usuarios (login/logout), listado de eventos y búsqueda por criterio. Arquitectura MVC con patrón Front Controller y manejo de excepciones personalizadas.

---

## 🛠️ Stack Técnico
* **IDE:** Eclipse IDE
* **Lenguaje:** Java 11
* **Plataforma:** Java EE (Servlets, JSP, JSTL)
* **Base de datos:** MySQL8
* **Acceso a datos:** JDBC, PreparedStatement, Connection Pool (DataSource)
* **Servidor de aplicaciones:** Apache Tomcat
* **Control de Versiones:** Git & GitHub

## 🏗 Arquitectura
El proyecto sigue el patrón **MVC (Modelo - Vista - Controlador)** con **Front Controller**:

- **Front Controller** — `ServletEmisora` recibe todas las peticiones HTTP  (GET y POST)
  y las delega al controlador correspondiente según el parámetro `accion`
- **Controladores** — cada operación CRUD tiene su propio controlador que implementa la interfaz `IControlador`
- **Modelo** — `ModeloUsuario` y `ModeloEvento` encapsulan toda la lógica de acceso a base de datos
- **Vista** — páginas JSP con JSTL para la presentación de datos
- **Excepciones** — clase ExcepcionPropia con mensajes de error centralizados por categoría (autenticación, eventos, base de datos)

## 📂 Estructura del Proyecto
```
src/main/java/es/accenture/emisora/
├── servlets/
│   └── ServletEventos.java              ← Front Controller (punto de entrada)
├── controladores/
│   ├── IControlador.java                ← Interfaz común para los controladores
│   ├── ControladorIniciarSesion.java    ← Autenticación de usuario
│   ├── ControladorCerrarSesion.java     ← Cierre de sesión
│   ├── ControladorBuscarEventos.java    ← Búsqueda de eventos por criterio
│   └── ControladorVolver.java           ← Volver al listado de eventos
├── modelos/
│   ├── ModeloUsuario.java               ← Consultas contra tabla 'usuarios'
│   └── ModeloEvento.java                ← Consultas contra tabla 'eventos'
├── entidades/
│   ├── Usuario.java                     ← Entidad usuario
│   └── Evento.java                      ← Entidad evento musical
└── excepciones/
    └── ExcepcionPropia.java             ← Excepción personalizada con mensajes centralizados

src/main/webapp/
├── Login.jsp                            ← Vista de login
├── META-INF/
│   └── context.xml                      ← Configuración DataSource (Tomcat)
└── WEB-INF/
    ├── BuscarEventos.jsp                ← Vista formulario de búsqueda
    ├── MostrarEventos.jsp               ← Vista listado de eventos
    └── web.xml                          ← Configuración del Servlet
```

## 🚀Funcionalidades
- Autenticación de usuarios con validación de credenciales contra base de datos
- Cierre de sesión con invalidación de la sesión HTTP
- Listado completo de eventos musicales disponibles
- Búsqueda de eventos por nombre o descripción (consulta LIKE)
- Manejo centralizado de errores con excepción personalizada ExcepcionPropia

## ⚙️Instalación y ejecución

### Requisitos previos
- Java 11
- Apache Tomcat 9
- MySQL 8
- Eclipse IDE (con plugin para proyectos web dinámicos)


### 1. Clonar el repositorio
```bash
git clone https://github.com/jmpm8/JavaEEGestionEventosEmisora.git
```

### 2. Crear la base de datos en MySQL
Crea una base de datos llamada `musicadb2` y las tablas necesarias:
```sql
CREATE DATABASE festivaldb;
USE festivaldb;

CREATE TABLE usuarios (
    usuarioId INT AUTO_INCREMENT PRIMARY KEY,
    nombre VARCHAR(100) NOT NULL,
    apellido VARCHAR(100),
    dni VARCHAR(20),
    email VARCHAR(100),
    telefono VARCHAR(20),
    direccion VARCHAR(200),
    usuario VARCHAR(50) NOT NULL,
    password VARCHAR(100) NOT NULL
);

CREATE TABLE eventos (
    eventoId INT AUTO_INCREMENT PRIMARY KEY,
    nombre VARCHAR(100) NOT NULL,
    descripcion TEXT,
    lugar VARCHAR(100),
    duracion VARCHAR(50),
    tipoEvento VARCHAR(50),
    asientosDisp INT
);
```

### 3. Configurar la conexión a base de datos
Edita el archivo `src/main/webapp/META-INF/context.xml` con tus credenciales de MySQL:
```xml
<Resource name="jdbc/Festival"
    username="TU_USUARIO"
    password="TU_PASSWORD"
    url="jdbc:mysql://localhost:3306/festivaldb"
    ...
/>
```

### 4. Desplegar en Eclipse con Tomcat
1. Importa el proyecto en Eclipse como **Dynamic Web Project**
2. Añade el proyecto al servidor Tomcat configurado en Eclipse
3. Inicia el servidor
4. Accede a: http://localhost:8080/JavaEEGestionEventosEmisora
