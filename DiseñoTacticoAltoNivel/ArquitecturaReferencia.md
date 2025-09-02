# Arquitectura de Referencia
![Arquitectura de Referencia](/Imagenes/Arquitectura_Referencia.jpg)

## 1. Interacción del Usuario (User → Web/Http)

### Usuario
Un jugador o Entrenador inicia la interacción con la aplicación, desde un **navegador web**.

### Web/Http
Las solicitudes del usuario viajan a través de la web utilizando el protocolo **HTTP(S)**, para alcanzar el punto de entrada de la aplicación.  
Esta capa se encarga de **recibir y enrutar** las peticiones iniciales hacia los componentes adecuados del sistema.

---

## 2. Procesamiento Centralizado (Web/Http → Back End)

### Back End
Este componente actúa como el **cerebro de la aplicación**, recibiendo todas las solicitudes provenientes de la capa Web/Http.  
Es responsable de procesar la lógica de negocio central, incluyendo:

- **Gestión Tareas**:  
  Inicio,finalización,repetición de los diferentes niveles dentro de Cogniverse.

- **Control del Flujo de Entrenamiento**:  
  Administra el progreso y resultados de una sesión de entrenamiento.

- **Orquestación**:  
  Comunicación con el componente centralizador cuando una sesión es iniciada y cuando se envian los resultados de los cuadros de desempeño.

---

## 3. Persistencia de Datos (Back End → Data Bases)

### Data Bases
Este componente es el **repositorio de información persistente** del sistema.  
El Back End interactúa con él para:

- **Almacenar Datos**:  
  Guarda información como Niveles,Tareas,sesiones y cuandros de desempelo en la sesión de entrenamiento.

- **Recuperar Datos**:  
  Consulta y obtiene la información necesaria para responder al componente centralizador o realizar cálculos internos.

- **Asegurar la Integridad**:  
  Garantiza que los datos se almacenen de manera consistente y fiable, manteniendo la coherencia del sistema.

---

## 4. Lógica Especializada (Back End → Componente Centralizador)

### Componente Centralizador
Este componente representa un **servicio o módulo externo especializado** que encapsula la lógica compleja relacionada con el componente central.

El Back End se comunica con este servicio para:

- Obtener datos específicos de entrenamiento.
- Validar interacciones, como respuestas o acciones del usuario.
- Retonar contenido especializado, como cuadros desempeño y resultados del entrenamiento.

**Línea Punteada de Retorno**:  
Este servicio devuelve los datos o resultados procesados al Componente Centralizador, que los utiliza para:

- Actualizar la base de datos.
- Formular la respuesta final al usuario.

**Línea Continua**
Este servicio devuelve los datos de una sesión al backend, que lso utiliza para:
- Comenzar la sesión de entrenamiento.
- Conocer el progreso de un estudiante durante una sesión.


# Arquetipo de Referencia

![Arquetipo de Referencia](/Imagenes/Arquetipo_Referencia.jpg)

La arquitectura propuesta representa la variable Control Inhibitorio de una aplicación de entrenamiento cognitivo enfocada en entrenar a los estudiantes. Este componente actúa como la variable encargada de entrenar el control inhibitorio y de generar las métricas esperadas para el componente centralizador.

# Componentes y su Funcionamiento

## Users (Usuarios)
Representan a los jugadores y Entrenadores que interactúan con la aplicación.  
Son el punto de origen de todas las solicitudes al sistema.

## Http
Este elemento simboliza la capa de comunicación web.  
Las solicitudes de los usuarios (desde el frontend de React en sus navegadores) se envían mediante el protocolo HTTP(S) al servidor de la aplicación.  
Es el estándar para la interacción cliente-servidor en la web.

## React
En esta parte del flujo, se asume que existe una aplicación frontend desarrollada en React.  
Esta aplicación es la que los usuarios finales utilizan para interactuar (Jugar, Menu, visualización de progreso, etc.).

React se encarga de:

- La interfaz de usuario dinámica y reactiva.
- Enviar solicitudes al backend de Node.js.

## Node.js - Control-Inhibitorio-back-api
Este es el corazón de la variable control inhibitorio y el backend principal.  
Node.js es un entorno de ejecución de JavaScript del lado del servidor que permite construir aplicaciones escalables y de alto rendimiento.  
En esta arquitectura, actúa como el servidor de APIs principal.

### Funcionalidades Centrales
Este componente es responsable de:

- **Gestión del guardado de datos**: Almacena los resultados de cada encuesta y cada cuadro de desempeño, creando una conexión directa con la base de datos.
- **Gestión del Progreso de Variable de Entrenamiento**: Almacena y actualiza el estado de los usuarios en cada sesión de entrenamiento (puntuaciones, avances, etc.).
- **Orquestación de Solicitudes**: Actua de manera conjunta con el `Componente-centralizador-api`, creando una conexión directa entre ambos, para la correcta transmisión de datos.

## PostgreSQL
Es la base de datos relacional utilizada para almacenar la información persistente del sistema.

### Datos que Almacena
Principalmente, guardará:

- Información de cada una de las tareas(duración, nombre, etc).
- Información de cada uno de los niveles(duración,cantidad de estímulos,porcentaje para ganar,etc).
- Información del resultado de cada uno de los estudiantes durante el desarrollo de la sesión de entranmiento en    cada uno de los nieveles.

Su naturaleza relacional es ideal para modelar estas relaciones complejas y asegurar la integridad de los datos.

## Componente-Centralizador-api
Este es un servicio externo o microservicio al que el `Control-Inhibitorio-api` (Node.js) se conecta.

### Función
Este es el servicio encargado de orquestar la lógica central de la aplicación, Asi como también:

- Autenticación/Autorización de los usuarios.
- Gestión de los usuarios(Registro).
- Recolección y administración de los resultados de los cuadros de desempeño.
- Brindar la sesión de entranmiento, y en su defecto el progreso de la variable de un estudiante.

La flecha punteada desde `Control-Inhibitorio-api` de vuelta a `Componente-Centralizador-api` indica que se envian datos a desde el backend:

- Procese.
- Guarde en PostgreSQL.
- Presente al frontend de React.

# Justificación de las Herramientas Elegidas

Las herramientas seleccionadas son las mejores posibles en el contexto de tus necesidades específicas, ofreciendo un equilibrio entre desarrollo ágil, rendimiento, escalabilidad y gestión de datos confiable.  
Cada elección se ha realizado tras considerar alternativas y evaluar su idoneidad para los requerimientos de la **Variable Control Inhibitorio**.

---

## Node.js (para Control-Inhibitorio-api)

- **Rendimiento y Escalabilidad (No Bloqueante)**:  
  Node.js se basa en un modelo de E/S no bloqueante y un bucle de eventos, lo que lo hace extremadamente eficiente para manejar múltiples conexiones concurrentes con poca sobrecarga. Ideal para APIs backend en aplicaciones de juegos.

- **JavaScript End-to-End**:  
  Permite usar JavaScript tanto en frontend (React) como en backend (Node.js), facilitando el intercambio de conocimientos, la reutilización de código y la integración del equipo. Reduce la curva de aprendizaje y acelera el desarrollo.

- **Desarrollo Rápido y Prototipado**:  
  Gracias al ecosistema de `npm` y frameworks como `Express.js`, el desarrollo de APIs es ágil. Node.js acelera el lanzamiento al mercado.


- **Comunidad y Ecosistema Maduro**:  
  Gran comunidad, soporte constante y librerías robustas que aseguran sostenibilidad a largo plazo.

### Alternativa Considerada: Java con Spring Boot
Aunque **Java y Spring Boot** ofrecen robustez y rendimiento para construir microservicios empresariales, se prefirió **Node.js** por:

- Su rapidez de desarrollo.
- Ecosistema JavaScript unificado.
- Modelo asíncrono ideal para manejar concurrencia en tiempo real.
- Menor curva de aprendizaje y tiempo de configuración para este tipo de proyecto.

---

## React

- **Interfaz de Usuario Reactiva y Dinámica**:  
  Permite construir UIs con componentes reutilizables, brindando una experiencia fluida e interactiva. Perfecto para la dinámica de una app de juegos.

- **Rendimiento (Virtual DOM)**:  
  Optimiza las actualizaciones de la UI, lo que resulta en interfaces rápidas y responsivas.

- **Ecosistema y Herramientas**:  
  Comunidad activa con librerías como React Router, Redux o Zustand que agilizan el desarrollo de funcionalidades complejas.

- **Desarrollo Basado en Componentes**:  
  Favorece la modularidad y el mantenimiento del código. Ideal para apps con interfaces que evolucionan constantemente.

### Alternativa Considerada: Angular
Angular ofrece una arquitectura sólida para grandes aplicaciones empresariales, pero React fue elegido por:

- Su mayor flexibilidad.
- Curva de aprendizaje más accesible.
- Ligereza y rapidez en prototipado.
- Mejor adaptación a los requerimientos interactivos de una aplicación de juego.

---

## PostgreSQL

- **Fiabilidad y Consistencia (ACID)**:  
  Garantiza integridad en los datos transaccionales como el progreso y los puntajes de los jugadores.

- **Integridad de Datos**:  
  Soporte para claves primarias/foráneas, esquemas robustos e integridad referencial entre usuarios y variables de entrenamiento.

- **Escalabilidad Horizontal y Vertical**:  
  Soporta tanto aumento de recursos en un solo servidor como técnicas como sharding o réplicas para escalar horizontalmente.

- **Flexibilidad (JSONB)**:  
  Al ser relacional pero con soporte para JSONB, permite almacenar datos semi-estructurados sin perder el control relacional. Perfecto para variables de entrenamiento de estructura cambiante.

- **Comunidad y Ecosistema**:  
  Herramientas maduras, extensiones útiles y amplia documentación para facilitar su operación y crecimiento.

### Alternativa Considerada: MongoDB
MongoDB, como base de datos NoSQL, es ideal para esquemas flexibles y escalabilidad horizontal.  
Sin embargo, PostgreSQL fue preferido por:

- Su consistencia transaccional.
- Su solidez en relaciones entre entidades clave.
- La necesidad crítica de integridad en los datos del usuario y su progreso.

---

## Microservicio/Servicio Externo (Componente-Centralizador-api)

- **Separación de Responsabilidades (Microservicios)**:  
  La lógica cognitiva se mantiene independiente de la variable control inhibitorio, facilitando modularidad, mantenimiento y claridad en los dominios.

- **Escalabilidad Independiente**:  
  Este servicio puede escalar según su demanda sin afectar al resto del sistema. Si se requiere cómputo intensivo, solo él necesita más recursos.

- **Desarrollo Independiente**:  
  Equipos diferentes pueden trabajar en paralelo, incluso usando otras tecnologías si es necesario.

- **Reusabilidad**:  
  Puede integrarse fácilmente en otros módulos o aplicaciones futuras que requieran lógica cognitiva similar.

### Alternativa Considerada: Arquitectura Monolítica
Una arquitectura monolítica podría parecer más sencilla inicialmente, pero se descartó por:

- Mayor dificultad de escalar componentes individualmente.
- Complejidad creciente al mezclar dominios lógicos distintos.
- Menor mantenibilidad a largo plazo frente a un microservicio especializado.



# Plataforma Tecnológica

La elección de las herramientas tecnológicas para este proyecto se fundamenta en la búsqueda de eficiencia en el desarrollo, rendimiento óptimo, escalabilidad y robustez en la gestión de datos y seguridad.

---

## Front End: React

Se ha optado por el framework de JavaScript **React** para la construcción de la capa visual de la aplicación.

- React es una librería de código abierto mantenida por Facebook y una vasta comunidad.
- Ideal para construir interfaces de usuario (UI) dinámicas y de alto rendimiento.
- Su enfoque en el desarrollo basado en componentes permite:
  - Crear módulos reutilizables.
  - Gestionar eficientemente el estado de la aplicación.
  - Facilitar la mantenibilidad del código.
- Utiliza un **Virtual DOM** que optimiza las actualizaciones de la UI, brindando una experiencia fluida y reactiva.
- Su amplio ecosistema y herramientas de desarrollo contribuyen a la eficiencia y calidad del producto final.

---

## Back End: Node.js

La lógica de negocio del proyecto se implementará utilizando **Node.js**, un entorno de ejecución de JavaScript del lado del servidor.

- Se destaca por su arquitectura asíncrona y no bloqueante.
- Eficiente para manejar un gran número de conexiones concurrentes con baja latencia.
- Ideal para construir APIs que sirven a múltiples usuarios simultáneamente (por ejemplo, en una aplicación de juegos).
- Permite mantener un **stack tecnológico homogéneo** (JavaScript/TypeScript) entre frontend y backend:
  - Reutilización de conocimientos.
  - Agilidad en el equipo de desarrollo.
- Junto con frameworks como **Express.js**:
  - Optimiza y acelera el desarrollo de APIs robustas.
  - Fomenta prácticas de código limpio.
  - Promueve estructuras modulares para una mejor mantenibilidad.

---

## Fuente de Datos: PostgreSQL

Se ha seleccionado **PostgreSQL** como la base de datos relacional principal para el almacenamiento de datos del proyecto.

- Garantiza la **integridad de los datos** mediante las propiedades **ACID**:
  - Atomicidad.
  - Consistencia.
  - Aislamiento.
  - Durabilidad.
- Ideal para gestionar información transaccional como:
  - Usuarios.
  - Progreso de los usuarios en el juego.
- Ofrece:
  - Manejo robusto de esquemas.
  - Soporte para claves primarias y foráneas.
  - Índices avanzados para optimizar las consultas.
- Asegura la **fiabilidad y consistencia a largo plazo**, clave para el uso continuo de la aplicación.


[Volver](https://github.com/alejoDev117/Documentacion_Control_Inhibitorio/tree/main)
