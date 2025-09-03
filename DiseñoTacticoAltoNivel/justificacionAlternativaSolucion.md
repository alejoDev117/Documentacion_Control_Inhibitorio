# Justificación de Alternativa de Solución - Cogniverse

## 1. Contexto del Problema

El proyecto Cogniverse se desarrolla como una plataforma de entrenamiento cognitivo enfocada en el control inhibitorio para estudiantes. El sistema requiere una arquitectura que soporte:

- Una experiencia gamificada con narrativa espacial para mantener el compromiso de los estudiantes
- Capacidad para funcionar sin conexión a internet (modo offline)
- Almacenamiento y seguimiento del progreso de los estudiantes
- Integración con un componente centralizador externo para autenticación y seguimiento
- Interfaz intuitiva que permita pausar y continuar entrenamientos

El desafío principal consiste en diseñar una arquitectura que balancee la experiencia de usuario interactiva y gamificada con los requisitos técnicos de funcionamiento offline, seguridad y mantenibilidad.

## 2. Alternativas Evaluadas

Considerando el stack tecnológico observado en el código (React, Node.js, Express, PostgreSQL) y las restricciones del proyecto, se evaluaron las siguientes alternativas arquitectónicas:

### 2.1. Aplicación Monolítica Integrada

**Descripción**: Una única aplicación que integra frontend y backend en un solo paquete desplegable.

**Tecnologías**: Node.js con Express sirviendo contenido estático de React.

**Características**:
- Frontend y backend empaquetados juntos
- Comunicación directa entre capas sin API HTTP
- Base de datos embebida o local

### 2.2. Arquitectura Cliente-Servidor con API REST

**Descripción**: Separación clara entre frontend (cliente) y backend (servidor) comunicados a través de una API REST.

**Tecnologías**: React para frontend, Node.js/Express para backend, PostgreSQL como base de datos.

**Características**:
- Separación de responsabilidades entre cliente y servidor
- Comunicación vía API REST con formato JSON
- Patrón MVC en el backend
- ORM (Sequelize) para abstracción de base de datos

### 2.3. Arquitectura Basada en Microservicios

**Descripción**: División del backend en múltiples servicios independientes por dominio funcional.

**Tecnologías**: React para frontend, múltiples servicios Node.js/Express, PostgreSQL con esquemas separados.

**Características**:
- Servicios independientes para gestión de sesiones, niveles, rendimiento, etc.
- API Gateway para unificar el acceso desde el frontend
- Bases de datos potencialmente separadas por servicio

## 3. Criterios de Comparación

Los criterios utilizados para evaluar las alternativas se alinean con los drivers arquitectónicos identificados:

### 3.1. Usabilidad
- Capacidad para implementar una interfaz gamificada fluida
- Soporte para continuidad de sesiones (pausar/continuar)
- Tiempo de respuesta percibido por el usuario

### 3.2. Seguridad
- Implementación de autenticación por token
- Autorización basada en roles
- Protección de datos sensibles

### 3.3. Disponibilidad
- Capacidad para funcionar offline
- Sincronización cuando se recupera la conexión
- Resiliencia ante fallos

### 3.4. Mantenibilidad
- Modularidad del código
- Facilidad para agregar nuevas funcionalidades
- Capacidad de prueba (testability)
- Logging y monitoreo

### 3.5. Complejidad de Implementación
- Esfuerzo de desarrollo requerido
- Curva de aprendizaje para el equipo
- Complejidad operacional

## 4. Alternativa Seleccionada

Tras la evaluación, se seleccionó la **Arquitectura Cliente-Servidor con API REST** (alternativa 2.2) por los siguientes motivos técnicos:

### 4.1. Justificación Técnica

1. **Balance entre complejidad y separación de responsabilidades**:
   - Proporciona una clara separación entre frontend y backend sin la complejidad operacional de los microservicios
   - Permite desarrollo paralelo de componentes frontend y backend por equipos especializados

2. **Adecuación para requisitos de funcionamiento offline**:
   - El cliente React puede funcionar con datos en caché cuando no hay conexión
   - El servidor Express puede ejecutarse localmente en la misma máquina que el cliente
   - PostgreSQL ofrece una base de datos robusta que puede funcionar en modo local

3. **Alineación con patrones establecidos**:
   - El patrón MVC implementado en el backend facilita la organización del código
   - La estructura de componentes en React permite una UI modular y reutilizable
   - El uso de Sequelize como ORM abstrae la complejidad de la base de datos

4. **Escalabilidad adecuada**:
   - Permite escalar horizontalmente el backend si fuera necesario en el futuro
   - La separación en capas (controladores, servicios, repositorios) facilita la extensión

5. **Integración con componente centralizador**:
   - La arquitectura de servicios en el backend facilita la integración con sistemas externos
   - Los tokens de autenticación pueden gestionarse eficientemente en este modelo

## 5. Impacto en el Diseño Táctico

La elección de la arquitectura Cliente-Servidor con API REST influye en el diseño táctico de la siguiente manera:

### 5.1. Impacto en el Backend

1. **Estructura en Capas**:
   - **Controladores**: Manejan las peticiones HTTP, validación de entrada y respuestas
   - **Servicios**: Implementan la lógica de negocio y orquestan operaciones
   - **Repositorios**: Abstraen el acceso a datos y operaciones con la base de datos
   - **Modelos**: Definen la estructura de datos y relaciones

2. **Gestión de Estado**:
   - Sesiones de entrenamiento persistentes en base de datos
   - Tablas de rendimiento para seguimiento del progreso

3. **Integración Externa**:
   - Clientes HTTP para comunicación con el componente centralizador
   - Middleware de autenticación para validación de tokens

### 5.2. Impacto en el Frontend

1. **Arquitectura de Componentes**:
   - Componentes reutilizables para elementos de UI comunes
   - Páginas para las diferentes vistas (menú, planetas, niveles, juegos)
   - Gestión de estado para mantener la continuidad de la experiencia

2. **Comunicación con Backend**:
   - Servicios de API para encapsular llamadas al backend
   - Manejo de estados de carga, error y éxito
   - Caché local para funcionamiento offline

3. **Experiencia de Usuario**:
   - Implementación de la narrativa espacial a través de componentes visuales
   - Sistema de navegación entre planetas y niveles
   - Feedback inmediato durante los ejercicios de control inhibitorio

### 5.3. Impacto en la Interacción Frontend-Backend

1. **Contrato de API**:
   - Definición clara de endpoints REST
   - Formato JSON para intercambio de datos
   - Documentación con Swagger

2. **Flujo de Datos**:
   - Autenticación mediante tokens
   - Carga de configuración de niveles desde el backend
   - Envío de resultados de rendimiento al backend
   - Notificación al componente centralizador

3. **Gestión de Errores**:
   - Manejo consistente de errores en frontend y backend
   - Logging extensivo para facilitar la depuración
   - Feedback apropiado al usuario

La arquitectura seleccionada proporciona un equilibrio óptimo entre la experiencia de usuario requerida y los requisitos técnicos del sistema, permitiendo un desarrollo modular, mantenible y alineado con los drivers arquitectónicos identificados.

[Volver](https://github.com/alejoDev117/Documentacion_Control_Inhibitorio/tree/main)