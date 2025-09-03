# Plataforma Tecnológica - Cogniverse

## Lenguajes de Programación

### Backend
- **JavaScript/Node.js**: El backend está desarrollado utilizando Node.js, un entorno de ejecución de JavaScript del lado del servidor que permite crear aplicaciones escalables y de alto rendimiento.

### Frontend
- **JavaScript/React**: El frontend está desarrollado con React, una biblioteca de JavaScript para construir interfaces de usuario interactivas y de una sola página (SPA).
- **JSX**: Extensión de sintaxis utilizada con React para combinar HTML y JavaScript.

## Frameworks y Librerías Principales

### Backend
- **Express.js**: Framework web minimalista para Node.js que proporciona un conjunto robusto de características para aplicaciones web y móviles.
- **Sequelize**: ORM (Object-Relational Mapping) para Node.js que facilita la interacción con bases de datos relacionales como PostgreSQL.
- **Swagger**: Utilizado para la documentación de la API REST.
- **Winston**: Biblioteca para el registro de logs con soporte para múltiples transportes.
- **Cors**: Middleware para habilitar el acceso a recursos desde diferentes dominios (Cross-Origin Resource Sharing).
- **Helmet**: Middleware que ayuda a proteger aplicaciones Express configurando varios encabezados HTTP relacionados con la seguridad.
- **Axios**: Cliente HTTP basado en promesas para realizar solicitudes a servicios externos.

### Frontend
- **React Router**: Biblioteca para la navegación entre diferentes componentes en aplicaciones React.
- **Vite**: Herramienta de construcción moderna que proporciona una experiencia de desarrollo más rápida y eficiente.
- **ESLint**: Herramienta de análisis de código estático para identificar patrones problemáticos en el código JavaScript.

## Sistemas de Bases de Datos

- **PostgreSQL**: Sistema de gestión de bases de datos relacional utilizado para almacenar datos estructurados como:
  - Sesiones de entrenamiento
  - Tablas de rendimiento
  - Información de estudiantes
  - Configuración de niveles y tareas

La base de datos utiliza múltiples esquemas:
- Esquema `ci` (Control Inhibitorio): Contiene tablas relacionadas con el rendimiento.
- Esquema `cc` (posiblemente Control Cognitivo): Contiene tablas relacionadas con las sesiones de entrenamiento.

## Servicios Externos o Integraciones

- **Componente Centralizador**: La aplicación se integra con un componente externo a través de una API REST (accesible en el puerto 3001) que proporciona:
  - Autenticación de usuarios
  - Seguimiento centralizado de progreso
  - Posiblemente otras funcionalidades de gestión centralizada

- **API de Entrenamiento Cognitivo**: Integración con un servicio externo para funcionalidades específicas de entrenamiento cognitivo, referenciado en la configuración como `COGNITIVE_TRAINING_API_URL`.

## Entorno de Despliegue

- **Docker**: La aplicación utiliza contenedores Docker para el despliegue, lo que proporciona:
  - Aislamiento de entorno
  - Portabilidad entre diferentes sistemas
  - Facilidad de despliegue y escalabilidad

- **Docker Compose**: Utilizado para orquestar múltiples contenedores, definiendo:
  - Servicios de aplicación
  - Redes
  - Volúmenes para persistencia de datos

- **Configuración de Red**: La aplicación utiliza una red externa de Docker (`deployment_postgres_network`) para comunicarse con la base de datos PostgreSQL.

## Dependencias Críticas

- **Sequelize y PostgreSQL**: La aplicación depende fuertemente de Sequelize como ORM y PostgreSQL como base de datos, lo que condiciona el diseño de los modelos de datos y la interacción con la base de datos.

- **React y React Router**: El frontend está construido sobre React y depende de React Router para la navegación, lo que influye en la estructura de la aplicación y la experiencia de usuario.

- **Componente Centralizador Externo**: La dependencia de un componente externo para autenticación y seguimiento centralizado es crítica para el funcionamiento completo del sistema.

- **Almacenamiento Local (localStorage)**: La aplicación utiliza el almacenamiento local del navegador para mantener el estado de la sesión y permitir el funcionamiento offline.

## Soporte a Drivers Arquitectónicos

### Usabilidad
- **Interfaz Gamificada**: La estructura de componentes en React permite implementar una experiencia gamificada con narrativa espacial.
- **Navegación Fluida**: React Router facilita la navegación entre diferentes secciones de la aplicación sin recargar la página.
- **Feedback Inmediato**: La arquitectura cliente-servidor con API REST permite proporcionar feedback inmediato a los usuarios.

### Seguridad
- **Helmet**: Proporciona protección a nivel de encabezados HTTP.
- **Autenticación por Token**: Implementada a través de la integración con el componente centralizador.
- **Validación de Datos**: Express facilita la validación de datos de entrada en el backend.

### Disponibilidad
- **Funcionamiento Offline**: La arquitectura frontend permite el almacenamiento local de datos para funcionar sin conexión.
- **Sincronización**: Capacidad para sincronizar datos cuando se recupera la conexión.
- **Contenedores Docker**: Proporcionan aislamiento y facilitan la recuperación en caso de fallos.

### Mantenibilidad
- **Arquitectura Limpia**: El backend sigue principios de arquitectura limpia con separación de responsabilidades (controladores, servicios, repositorios, modelos).
- **Componentes Reutilizables**: La estructura de componentes en React facilita la reutilización de código.
- **Documentación API**: Swagger proporciona documentación clara de la API para facilitar la integración y el mantenimiento.
- **Logging Extensivo**: Winston permite un registro detallado para facilitar la depuración y el monitoreo.

### Escalabilidad
- **Contenedores Docker**: Facilitan la escalabilidad horizontal de la aplicación.
- **Separación Frontend/Backend**: Permite escalar cada componente de forma independiente según las necesidades.
- **ORM Sequelize**: Abstrae la complejidad de la base de datos y facilita posibles migraciones o cambios en el esquema.