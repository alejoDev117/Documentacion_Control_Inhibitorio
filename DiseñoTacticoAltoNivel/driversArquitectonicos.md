# Diseño Táctico de Alto Nivel - Cogniverse

## 1. Drivers Arquitectónicos

### 1.1 Requisitos de Calidad

#### Usabilidad
- **Interfaz Gamificada**: El sistema implementa una narrativa espacial (Cogniverse) con planetas y niveles para mejorar la experiencia de usuario y el compromiso de los estudiantes.
- **Continuidad de Sesiones**: Capacidad para pausar y continuar entrenamientos desde el punto exacto donde se dejaron.

#### Seguridad
- **Autenticación por Token**: Acceso protegido mediante tokens enviados por el componente centralizador.
- **Autorización por Roles**: Control de acceso basado en roles (Estudiante, Entrenador).

#### Disponibilidad
- **Funcionamiento Offline**: El sistema debe poder ejecutarse en un PC sin conexión a internet, de manera local.

#### Mantenibilidad
- **Código Modular**: Organización en componentes claramente separados tanto en frontend como en backend.
- **Logging Extensivo**: Registro de acciones, advertencias y errores en controladores y servicios.

### 1.2 Restricciones Tecnológicas

- **Frontend**: React con React Router para navegación entre componentes.
- **Backend**: Node.js con Express como framework web.
- **Base de Datos**: PostgreSQL con Sequelize como ORM.
- **API**: RESTful con métodos estándar (POST, GET, PUT) e intercambio de datos en formato JSON.
- **Documentación API**: Swagger para documentación interactiva.

### 1.3 Decisiones Clave

- **Arquitectura Cliente-Servidor**: Separación clara entre frontend (React) y backend (Node.js/Express).
- **Patrón MVC en Backend**: Organización en Modelos, Controladores y Servicios.
- **Componentes Reutilizables**: Estructura modular en frontend para reutilización de componentes.
- **Persistencia con ORM**: Uso de Sequelize para abstraer la interacción con la base de datos.
- **Integración con Componente Centralizador**: Comunicación con sistema externo para autenticación y seguimiento.

## 2. Diagramas de Arquitectura

### 2.1 Arquitectura General del Sistema

```mermaid
flowchart TB
    subgraph "Frontend (React)"
        UI[Interfaz de Usuario]
        Routes[React Router]
        Components[Componentes React]
        Pages[Páginas]
        State[Estado de la Aplicación]
    end

    subgraph "Backend (Node.js/Express)"
        API[API REST]
        Controllers[Controladores]
        Services[Servicios]
        Repositories[Repositorios]
        Models[Modelos]
    end

    subgraph "Persistencia"
        DB[(PostgreSQL)]
    end

    subgraph "Componente Centralizador"
        Auth[Autenticación]
        Tracking[Seguimiento]
    end

    UI --> Routes
    Routes --> Pages
    Pages --> Components
    Components --> State

    UI -- HTTP Requests --> API
    API --> Controllers
    Controllers --> Services
    Services --> Repositories
    Repositories --> Models
    Models --> DB

    Services -- API Calls --> Auth
    Services -- Envío de Resultados --> Tracking
```

### 2.2 Flujo de Datos Principal

```mermaid
sequenceDiagram
    actor Estudiante
    participant Frontend
    participant Backend
    participant DB
    participant Centralizador

    Estudiante->>Frontend: Inicia sesión (con token)
    Frontend->>Backend: Valida token
    Backend->>Centralizador: Verifica autenticación
    Centralizador-->>Backend: Confirma autenticación
    Backend-->>Frontend: Sesión autorizada
    
    Estudiante->>Frontend: Selecciona tarea/nivel
    Frontend->>Backend: Solicita datos de nivel
    Backend->>DB: Consulta configuración
    DB-->>Backend: Retorna configuración
    Backend-->>Frontend: Envía datos de nivel
    
    Estudiante->>Frontend: Completa nivel
    Frontend->>Backend: Envía resultados
    Backend->>DB: Guarda cuadro de desempeño
    Backend->>Centralizador: Notifica resultados
    Centralizador-->>Backend: Confirma recepción
    Backend-->>Frontend: Confirma guardado
    Frontend-->>Estudiante: Muestra siguiente nivel
```

### 2.3 Arquitectura del Frontend

```mermaid
flowchart TB
    subgraph "Estructura de Componentes"
        App[App.jsx]
        Router[React Router]
        
        subgraph "Páginas"
            Menu[Menu.jsx]
            Planetas[Planetas.jsx]
            Niveles[Selección de Niveles]
            Juegos[Componentes de Juego]
        end
        
        subgraph "Componentes Compartidos"
            UI[Componentes UI]
            MusicaFondo[MusicaFondo.jsx]
            SessionHandler[Gestión de Sesión]
        end
    end
    
    App --> Router
    Router --> Menu
    Menu --> Planetas
    Planetas --> Niveles
    Niveles --> Juegos
    
    App --> MusicaFondo
    App --> SessionHandler
    Juegos --> UI
```

### 2.4 Arquitectura del Backend

```mermaid
flowchart TB
    subgraph "Estructura del Backend"
        Server[server.js]
        Routes[routes/index.js]
        
        subgraph "Controladores"
            PerformanceController[performanceTableController.js]
            SessionController[trainingSessionController.js]
            LevelController[levelController.js]
        end
        
        subgraph "Servicios"
            PerformanceService[performanceTableService.js]
            SessionService[trainingSessionService.js]
            LevelService[levelService.js]
        end
        
        subgraph "Modelos"
            PerformanceModel[performanceTable.js]
            SessionModel[trainingSession.js]
            LevelModel[level.js]
            TaskModel[task.js]
        end
        
        subgraph "Configuración"
            DB[sequelize.js]
            Logger[logger.js]
        end
    end
    
    Server --> Routes
    Routes --> PerformanceController & SessionController & LevelController
    
    PerformanceController --> PerformanceService
    SessionController --> SessionService
    LevelController --> LevelService
    
    PerformanceService & SessionService & LevelService --> PerformanceModel & SessionModel & LevelModel & TaskModel
    
    PerformanceModel & SessionModel & LevelModel & TaskModel --> DB
    PerformanceController & SessionController & LevelController --> Logger
```

### 2.5 Modelo de Dominio

```mermaid
classDiagram
    class TrainingSession {
        UUID id
        String student
        String documentId
        DateTime startTime
        DateTime endTime
    }
    
    class PerformanceTable {
        UUID id
        UUID session
        UUID level
        int correctAnswers
        int errors
        int brainsObtained
        boolean passed
        double reactionTime
        String playedTime
        DateTime startTime
        DateTime endTime
        String studentDocumentNumber
    }
    
    class Level {
        UUID id
        UUID task
        String name
        String description
        int difficulty
    }
    
    class Task {
        UUID id
        UUID taskType
        String name
        String description
    }
    
    class TaskType {
        UUID id
        String name
        String description
    }
    
    TrainingSession "1" -- "many" PerformanceTable : contiene
    Level "1" -- "many" PerformanceTable : registra
    Task "1" -- "many" Level : contiene
    TaskType "1" -- "many" Task : clasifica
```

## 3. Justificación de Diseño

### 3.1 Alineación con Drivers Arquitectónicos

#### Usabilidad
La arquitectura implementa una clara separación entre la lógica de presentación (frontend) y la lógica de negocio (backend), permitiendo una experiencia de usuario fluida y gamificada. La estructura de componentes en React facilita la creación de interfaces interactivas y la navegación entre diferentes "planetas" y "niveles" del juego.

#### Seguridad
El sistema implementa autenticación basada en tokens y autorización por roles, con validación en el backend. La comunicación con el componente centralizador para la autenticación asegura que solo usuarios autorizados puedan acceder al sistema.

#### Disponibilidad
La arquitectura permite el funcionamiento offline al tener una base de datos local y un servidor que puede ejecutarse en la misma máquina que el cliente. Esto satisface el requisito de poder ejecutar el sistema sin conexión a internet.

#### Mantenibilidad
La estructura modular tanto en frontend (componentes React) como en backend (controladores, servicios, repositorios) facilita el mantenimiento y la extensión del sistema. El uso de patrones como MVC en el backend y la separación de responsabilidades mejora la mantenibilidad del código.

### 3.2 Decisiones Tecnológicas

- **React + Node.js**: Esta combinación permite un desarrollo ágil y modular, con una clara separación entre frontend y backend.
- **PostgreSQL + Sequelize**: Proporciona una base de datos robusta con un ORM que facilita la interacción y mantiene la integridad de los datos.
- **Express**: Framework ligero que facilita la creación de APIs RESTful con rutas claras y middleware para funcionalidades como autenticación y logging.
- **Swagger**: Mejora la documentación y facilita la prueba de la API, lo que es crucial para el mantenimiento y la extensión del sistema.

### 3.3 Consideraciones de Escalabilidad

La arquitectura está diseñada para soportar múltiples usuarios y sesiones de entrenamiento simultáneas. La separación en capas (controladores, servicios, repositorios) permite escalar horizontalmente el backend si fuera necesario en el futuro. El frontend basado en componentes facilita la adición de nuevas funcionalidades sin afectar las existentes.

### 3.4 Integración con Componente Centralizador

La arquitectura contempla la comunicación con un componente centralizador externo para autenticación y seguimiento. Esta integración se realiza a través de servicios específicos en el backend, manteniendo la separación de responsabilidades y facilitando posibles cambios en la integración en el futuro.