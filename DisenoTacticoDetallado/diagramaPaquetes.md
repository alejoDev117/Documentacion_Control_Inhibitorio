# Diagrama de Paquetes del Backend - Control Inhibitorio

Este documento presenta el diagrama de paquetes del backend del sistema de Control Inhibitorio, mostrando la estructura y relaciones entre los diferentes módulos que componen el backend de la aplicación.

## Diagrama de Paquetes del Backend

```mermaid
flowchart TB
    %% Main System Components
    ControlInhibitorio["Control Inhibitorio"]
    Backend["Backend"]

    %% Backend Packages
    BE_Clients["clients"]
    BE_Config["config"]
    BE_Controllers["controllers"]
    BE_Models["models"]
    BE_Repositories["repositories"]
    BE_Routes["routes"]
    BE_Services["services"]

    %% System Hierarchy
    ControlInhibitorio --> Backend

    %% Backend Hierarchy
    Backend --> BE_Clients
    Backend --> BE_Config
    Backend --> BE_Controllers
    Backend --> BE_Models
    Backend --> BE_Repositories
    Backend --> BE_Routes
    Backend --> BE_Services

    %% Backend Relationships
    BE_Controllers --> BE_Services
    BE_Services --> BE_Repositories
    BE_Repositories --> BE_Models
    BE_Services --> BE_Clients
    BE_Routes --> BE_Controllers

    %% Styling
    classDef system fill:#f9f,stroke:#333,stroke-width:2px
    classDef backend fill:#bbf,stroke:#333,stroke-width:1px
    classDef be_module fill:#ddf,stroke:#333,stroke-width:1px

    class ControlInhibitorio system
    class Backend backend
    class BE_Clients,BE_Config,BE_Controllers,BE_Models,BE_Repositories,BE_Routes,BE_Services be_module
```

## Descripción de los Paquetes

### Backend

- **clients**: Contiene los clientes para comunicación con servicios externos.
- **config**: Configuraciones del sistema y variables de entorno.
- **controllers**: Controladores que manejan las peticiones HTTP y delegan la lógica de negocio a los servicios.
- **models**: Definiciones de las entidades del dominio y sus relaciones.
- **repositories**: Implementaciones para el acceso y manipulación de datos.
- **routes**: Definición de rutas y endpoints de la API.
- **services**: Implementación de la lógica de negocio.


## Relaciones entre Paquetes

- Los **controllers** utilizan los **services** para ejecutar la lógica de negocio.
- Los **services** utilizan los **repositories** para acceder a los datos.
- Los **repositories** trabajan con los **models** para manipular las entidades.
- Los **services** pueden utilizar **clients** para comunicarse con servicios externos.
- Las **routes** dirigen las peticiones a los **controllers** correspondientes.

[Volver](https://github.com/alejoDev117/Documentacion_Control_Inhibitorio/tree/main)