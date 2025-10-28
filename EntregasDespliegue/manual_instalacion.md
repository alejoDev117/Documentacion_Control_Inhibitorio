# Manual de Instalación: Variable de Control Inhibitorio

El objetivo de este tutorial es explicar cómo hacer la correcta instalación de la variable de control inhibitorio, tanto el backend como el frontend.

---

## Aspectos a Tener en Cuenta

Es importante entender que la variable de control inhibitorio tiene dependencias clave:

* **Dependencia de Datos:** Depende de datos proporcionados por el "componente centralizador", como el ID, la sección y el estudiante.
* **Ejecución Local:** Debido a estas dependencias, la ejecución local puede presentar fallas.
* **Flujo de Pruebas:** Se puede ejecutar y navegar por el flujo de los diferentes juegos, pero no se almacenarán datos.
* **Simulación:** Para simular el almacenamiento de datos, se debe simular una sesión y registrar esos datos simulados en el esquema de datos de la variable.
* **Endpoints:** La variable necesita consultar algunos endpoints expuestos por el componente centralizador para su flujo adecuado.

---

## Instalación y Configuración

### 1. Obtener los Repositorios

Lo primero es descargar los repositorios desde la organización CognicareUCO.
* [Control Inhibitorio Frontend](https://github.com/CognicareUCO/ControlInhibitorioFrontEnd) --> https://github.com/CognicareUCO/ControlInhibitorioFrontEnd.git
* [control inhibitorio backend](https://github.com/CognicareUCO/ControlInhibitorioBackEnd) --> https://github.com/CognicareUCO/ControlInhibitorioBackEnd.git

Ambos deben clonarse desde la **rama `main`**, ya que es la que contiene los últimos cambios y está actualizada.

### 2. Configuración del Frontend

1.  Abra el repositorio del frontend en su entorno de desarrollo preferido.
2.  Cree o genere el archivo `.env` si no existe.
3.  Configure las siguientes variables de entorno:
    * `VITE_CC_API_BASE_URL`: La API para consumir las APIs del componente centralizador (`ej: http://localhost:3001/api`).
    * `VITE_TRAINER_URL`: La URL para redireccionar a la pantalla principal del componente centralizador (`ej: http://localhost:3001/entrenador`).
    * `VITE_API_BASE_URL`: La URL base para el backend de la variable de control inhibitorio (`ej: http://localhost:3005/api`).

    *(Estas variables son configurables según las URLs del centralizador y la configuración del backend).*
4.  Ejecute el comando `npm run build`  para generar la carpeta `dist`. Esta carpeta contendrá los archivos estáticos (assets) necesarios.

### 3. Configuración del Backend

1.  En el repositorio del backend, tome la carpeta `dist` generada por el frontend y péguela dentro de la ruta `public`. Esto permite que el backend sirva también el frontend.
2.  Configure el archivo `.env` del backend.
3.  Estas variables de entorno (que se pueden setear desde el `docker-compose`) incluyen:
    * Variables de acceso a la base de datos.
        - `DB_USER` Usuario base de datos.
        - `DB_HOST` Host base de datos.
        - `DB_NAME` Nombre base de datos.
        - `DB_PASSWORD` Contraseña para la base de datos.
        - `DB_PORT` Puerto por el que corre la base de datos.

    * El puerto de ejecución de la aplicación control inhibitorio `PORT` (ej. `3005`).
  

### 4. Configuración de Docker

1.  El backend ya cuenta con el respectivo archivo  `Dockerfile`.
2.  Genere la imagen de Docker (ej. `docker build -t [nombre-de-la-imagen]:version .`).
3.  (Opcional) Realice un `push` al Docker Hub si es necesario.
4.  El repositorio incluye un `docker-compose.yml` que genera la imagen y establece las variables de entorno.
5.  **Importante:** Este `docker-compose.yml` *no* incluye un servicio para la base de datos, ya que asume que esta será orquestada junto con el componente centralizador. Sin embargo, se puede agregar un servicio de base de datos fácilmente.

### 5. Configuración de la Base de Datos

1.  Existe un repositorio separado llamado [BaseDeDatos](https://github.com/CognicareUCO/BaseDeDatos) --> https://github.com/CognicareUCO/BaseDeDatos.git.
2.  Navegue a la "feature de controlInhibitorio"  y acceda a la ruta de control inhibitorio.
3.  Allí encontrará los archivos SQL necesarios para iniciar la base de datos correspondiente a esta variable.
4.  Puede ejecutar estos scripts para realizar una ejecución inicial con datos base.

---

## Ejecución en Entorno Local

Para correr en un entorno local, es importante crear una base de datos. Se pueden correr tanto el backend como el frontend de forma aislada, o generar el `dist` del frontend para ejecutarlo directamente desde el backend, como se explicó previamente.