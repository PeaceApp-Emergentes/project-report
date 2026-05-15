# Capítulo V: Tactical-Level Software Design

## 5.1. Bounded Context: IAM

### 5.1.1. Domain Layer

### 5.1.2. Interface Layer

### 5.1.3. Application Layer

### 5.1.4. Infrastructure Layer

### 5.1.5. Bounded Context Software Architecture Component Level Diagrams

### 5.1.6. Bounded Context Software Architecture Code Level Diagrams

#### 5.1.6.1. Bounded Context Domain Layer Class Diagrams

#### 5.1.6.2. Bounded Context Database Design Diagram



## 5.2. Bounded Context: Profile

El Bounded Context de **Profile** es responsable de la gestión de perfiles de los usuarios que interactúan con el sistema PeaceApp. En particular, maneja los perfiles de **Ciudadanos (Citizens)** y **Municipalidades (Municipalities)**. Este contexto permite registrar nuevos perfiles y obtener información de los mismos mediante su userId. Las entidades principales son Citizen y Municipality, y su estructura está diseñada para asegurar la unicidad de identificadores clave como DNI, correo electrónico institucional y número de teléfono.

### 5.2.1. Domain Layer

La capa de dominio encapsula las entidades centrales del sistema de perfiles y contiene la lógica de validación de atributos mediante objetos de valor. Las entidades principales son Citizen y Municipality, las cuales heredan de un agregado raíz auditable. Se utilizan objetos de valor como Phone, Dni y InstitutionalEmail para encapsular lógica específica y validación.

**Aggregate:** Citizen

**Descripción:** Representa el perfil de un ciudadano registrado en PeaceApp.

|**Atributo**|**Descripción**|**Tipo**|
| :-: | :-: | :-: |
|fullName|Nombre completo del ciudadano|String|
|city|Ciudad de residencia|String|
|district|Distrito de residencia|String|
|userId|ID del usuario (referencia a IAM)|Long|
|dni|Documento nacional de identidad|Dni|
|phone|Número de teléfono del ciudadano|Phone|

**Método**

|**Método**|**Descripción**|
| :-: | :-: |
|Citizen(CreateCitizenCommand command, Long userId)|Constructor que crea un perfil de ciudadano.|
|String getDni()|Devuelve el número de DNI del ciudadano.|
|String getPhone()|Devuelve el número telefónico del ciudadano.|

**Aggregate:** Municipality

**Descripción:** Representa el perfil de una municipalidad registrada en PeaceApp.

|**Atributo**|**Descripción**|**Tipo**|
| :-: | :-: | :-: |
|municipalityName|Nombre de la municipalidad|String|
|city|Ciudad donde opera la municipalidad|String|
|district|Distrito de operación|String|
|institutionalEmail|Correo institucional de la municipalidad|InstitutionalEmail|
|userId|ID del usuario (referencia a IAM)|Long|
|phone|Número telefónico institucional|Phone|

**Método**

|**Clase**|**Método**|**Descripción**|
| :-: | :-: | :-: |
|Municipality|Municipality(CreateMunicipalityCommand command, Long userId)|Constructor que crea un perfil de municipalidad.|
|Municipality|String getPhone()|Devuelve el número telefónico institucional.|
|Municipality|String getInstitutionalEmail()|Devuelve el correo institucional de la municipalidad.|

**Value Objects**

**Dni**

**Descripción:**
Representa el Documento Nacional de Identidad (DNI) de un ciudadano. Asegura que el valor sea un número positivo de exactamente 8 dígitos.

**Atributos:**

|**Nombre**|**Tipo**|**Descripción**|
| :- | :- | :- |
|dni|String|Número de DNI con exactamente 8 dígitos|

**Métodos:**

|**Nombre**|**Descripción**|
| :- | :- |
|Dni(String dni)|Constructor que valida que el valor tenga exactamente 8 dígitos.|
|Dni()|Constructor por defecto que asigna "00000000".|

**Phone**

**Descripción:**
Representa un número de teléfono válido, de exactamente 9 dígitos.

**Atributos:**

|**Nombre**|**Tipo**|**Descripción**|
| :- | :- | :- |
|phone|String|Número telefónico con exactamente 9 dígitos|

**Métodos:**

|**Nombre**|**Descripción**|
| :- | :- |
|Phone(String phone)|Constructor que valida que el valor tenga exactamente 9 dígitos.|
|Phone()|Constructor por defecto que asigna "000000000".|

**InstitutionalEmail**

**Descripción:**
Representa un correo electrónico institucional válido perteneciente a una municipalidad.

**Atributos:**

|**Nombre**|**Tipo**|**Descripción**|
| :- | :- | :- |
|email|String|Correo institucional válido de la municipalidad|

**Métodos:**

|**Nombre**|**Descripción**|
| :- | :- |
|InstitutionalEmail(String email)|Constructor que valida el formato correcto del correo institucional.|
|InstitutionalEmail()|Constructor por defecto que asigna "municipality@peaceapp.com".|

**Domain Services**

Los Domain Services en este contexto son interfaces que definen operaciones de negocio relacionadas con los aggregates Citizen y Municipality. Permiten separar las reglas de negocio que no pertenecen directamente a una entidad o value object.

**CitizenCommandService**

**Descripción:**
Interfaz que define operaciones de negocio relacionadas con la creación de un Citizen.

**Métodos:**

|**Nombre**|**Descripción**|
| :- | :- |
|Optional<Citizen> handle(CreateCitizenCommand command, Long userId)|Procesa el comando de creación de un ciudadano, asociándolo a un User existente mediante su userId.|

**CitizenQueryService**

**Descripción:**
Interfaz que permite consultar información relacionada con un Citizen.

**Métodos:**

|**Nombre**|**Descripción**|
| :- | :- |
|Optional<Citizen> handle(GetCitizenByUserIdQuery query)|Obtiene un Citizen asociado a un User mediante su userId.|

**MunicipalityCommandService**

**Descripción:**
Interfaz que define operaciones de negocio relacionadas con la creación de una Municipality.

**Métodos:**

|**Nombre**|**Descripción**|
| :- | :- |
|Optional<Municipality> handle(CreateMunicipalityCommand command, Long userId)|Procesa el comando de creación de una municipalidad, asociándola a un User existente mediante su userId.|

**MunicipalityQueryService**

**Descripción:**
Interfaz que permite consultar información relacionada con una Municipality.

**Métodos:**

|**Nombre**|**Descripción**|
| :- | :- |
|Optional<Municipality> handle(GetMunicipalityByUserIdQuery query)|Obtiene una Municipality asociada a un User mediante su userId.|

### 5.2.2. Interface Layer

Esta capa actúa como punto de entrada para consultas externas relacionadas con los perfiles. A través de los controladores REST, los clientes pueden consultar el perfil de un ciudadano o una municipalidad por su userId.

**Controlador: ProfilesController**

**Descripción:** Gestiona las consultas de perfiles de usuarios.

|**Método**|**Descripción**|**HTTP**|**Respuesta**|
| :-: | :-: | :-: | :-: |
|getCitizenProfile(Long userId)|Devuelve el perfil de un ciudadano por su userId|GET /profiles/citizen/{userId}|Recurso del ciudadano|
|getMunicipalityProfile(Long userId)|Devuelve el perfil de una municipalidad por su userId|GET /profiles/municipality/{userId}|Recurso de la municipalidad|

### 5.2.3. Application Layer

Esta capa contiene la lógica de aplicación, incluyendo la validación de unicidad para campos clave y el manejo de comandos y consultas. Coordina la creación y recuperación de perfiles utilizando servicios específicos para cada tipo de usuario.

**Clase: CitizenCommandServiceImpl**

**Descripción:** Gestiona los comandos para la creación de ciudadanos.

|**Método**|**Descripción**|
| :-: | :-: |
|handle(CreateCitizenCommand)|Crea un nuevo perfil de ciudadano, validando unicidad de dni, phone y userId.|

**Clase: MunicipalityCommandServiceImpl**

**Descripción:** Gestiona los comandos para la creación de municipalidades.

|**Método**|**Descripción**|
| :-: | :-: |
|handle(CreateMunicipalityCommand)|Crea un nuevo perfil de municipalidad, validando institutionalEmail, phone y userId.|

**Clase: CitizenQueryServiceImpl**

**Descripción:** Gestiona consultas sobre ciudadanos.

|**Método**|**Descripción**|
| :-: | :-: |
|handle(GetCitizenByUserIdQuery)|Recupera un ciudadano a partir de su userId.|

**Clase: MunicipalityQueryServiceImpl**

**Descripción:** Gestiona consultas sobre municipalidades.

|**Método**|**Descripción**|
| :-: | :-: |
|handle(GetMunicipalityByUserIdQuery)|Recupera una municipalidad por su userId.|

### 5.2.4. Infrastructure Layer

La capa de infraestructura proporciona la implementación de persistencia para los perfiles, permitiendo operaciones CRUD y búsquedas específicas. Los repositorios se basan en Spring Data JPA.

**Repositorio: CitizenRepository**

**Descripción:** Administra la persistencia de la entidad Citizen.

|**Método**|**Descripción**|
| :-: | :-: |
|findCitizenByUserId(Long)|Recupera un ciudadano por su userId.|
|existsByDni_Dni(String)|Verifica si existe un ciudadano con un DNI dado.|
|existsByPhone_Phone(String)|Verifica si existe un ciudadano con un teléfono dado.|
|existsByUserId(Long)|Verifica si existe un ciudadano con un userId dado.|

**Repositorio: MunicipalityRepository**

**Descripción:** Administra la persistencia de la entidad Municipality.

|**Método**|**Descripción**|
| :-: | :-: |
|findMunicipalityByUserId(Long)|Recupera una municipalidad por su userId.|
|existsByInstitutionalEmail_Email(String)|Verifica si existe una municipalidad con un correo institucional dado.|
|existsByPhone_Phone(String)|Verifica si existe una municipalidad con un teléfono dado.|
|existsByUserId(Long)|Verifica si existe una municipalidad con un userId dado.|

<div style="page-break-after: always;"></div>




### 5.2.5. Bounded Context Software Architecture Component Level Diagrams
**Backend**

El Profile Bounded Context centraliza la gestión de la información de perfil de los usuarios, incluyendo su estructura de dominio, lógica de aplicación, almacenamiento persistente e interfaces expuestas vía HTTP. Su arquitectura facilita tanto el acceso directo desde aplicaciones cliente como la colaboración con otros contextos a través de su fachada de contexto, permitiendo así la reutilización controlada de funciones relacionadas con los perfiles sin romper la encapsulación.

!["Profile Management Component Diagram"](assets/component-backend-profile.png?raw=true)

<div style="page-break-after: always;"></div>

**WebApp**

El diagrama de componentes del Profile Bounded Context describe la estructura de componentes dedicados a la gestión de perfiles de ciudadanos y municipalidades dentro de PeaceApp. En este contexto, el Citizen Profile Component permite a los ciudadanos visualizar y editar su información personal, mientras que el Municipality Profile Component permite a las municipalidades gestionar su información institucional. Ambos componentes se apoyan en los servicios Citizen Profile Service y Municipality Profile Service, encargados de coordinar las operaciones de negocio y la comunicación con la API RESTful. Asimismo, el componente Profile Assembler se encarga de transformar y mapear los datos entre los modelos del frontend y los DTOs utilizados por la API, estableciendo una arquitectura desacoplada, mantenible y escalable que facilita la evolución de las funcionalidades relacionadas con la gestión de perfiles.

!["Profile Management WebApp Component Diagram"](assets/component-webApp-profile.png?raw=true)

<div style="page-break-after: always;"></div>

**MobileApp**

El diagrama de componentes del bounded context Profile muestra los componentes encargados de la gestión de perfiles de ciudadanos y municipalidades dentro de la aplicación móvil PeaceApp. El contexto incluye los componentes Citizen Profile Widget y Municipality Profile Widget, responsables de permitir la visualización y edición de la información personal e institucional de los usuarios desde la aplicación móvil. Asimismo, los servicios Citizen Profile Service y Municipality Profile Service centralizan la lógica de negocio relacionada con la gestión de perfiles, coordinando las operaciones de validación, recuperación y actualización de datos. El componente Profile Assembler se encarga de transformar y adaptar los datos entre las estructuras provenientes del backend y los modelos utilizados en la aplicación móvil. El flujo principal inicia desde los widgets de perfil hacia los servicios correspondientes, los cuales utilizan el ensamblador para procesar la información y gestionar las solicitudes hacia la API RESTful, permitiendo una administración de perfiles eficiente, desacoplada y mantenible.

!["Profile Management MobileApp Component Diagram"](assets/component-backend-mobileApp-profile.png?raw=true)

<div style="page-break-after: always;"></div>

### 5.2.6. Bounded Context Software Architecture Code Level Diagrams

#### 5.2.6.1. Bounded Context Domain Layer Class Diagrams

El diagrama de clases muestra la relación entre las entidades Citizen y Municipality, así como los objetos de valor asociados a ellas.

!["Profile Management Class Diagram"](assets/PeaceApp-Profile-ClassDiagram.png?raw=true)

#### 5.2.6.2. Bounded Context Database Design Diagram

El diagrama de base muestra las tablas citizens y municipalities, así como la relación entre estas.

!["Profile Management Database Diagram"](assets/PeaceApp-DatabaseDiagram-Profile.png?raw=true)

## 5.3. Bounded Context: Location
El bounded context **Location** pertenece al sistema PeaceApp y se encarga de gestionar las ubicaciones relacionadas con los reportes. Su función principal es registrar la latitud y longitud de los reportes aprobados, listar las ubicaciones guardadas, detectar zonas peligrosas y eliminar ubicaciones cuando un reporte es rechazado o eliminado.

### 5.3.1. Domain Layer
La capa de dominio contiene la lógica central del bounded context Location. En este caso, el aggregate principal es Location, el cual representa una coordenada geográfica asociada a un reporte. Esta entidad hereda de AuditableAbstractAggregateRoot, por lo que también cuenta con atributos comunes como id, createdAt y updatedAt.

**Aggregate:** Location
**Descripciòn:** Representa una ubicación geográfica almacenada por el sistema.

| Atributo  | Descripcion                                                                            | Tipo   |
|-----------|----------------------------------------------------------------------------------------|--------|
| id        | Identificador único de la ubicación. Es heredado desde AuditableAbstractAggregateRoot. | Long   |
| latitude  | Latitud de la ubicación registrada.                                                    | String |
| longitude | Longitud de la ubicación registrada.                                                   | String |
| idReport  | Identificador del reporte asociado a la ubicación.                                     | Long   |
| createdAt | Fecha de creación del registro. Es heredado desde la clase auditable.                  | Date   |
| updatedAt | Fecha de última modificación del registro. Es heredado desde la clase auditable.       | Date   |

| Atributo                                                     | Descripcion                                                                                   |
|--------------------------------------------------------------|-----------------------------------------------------------------------------------------------|
| Location()                                                   | Constructor vacío requerido por JPA para crear instancias de la entidad.                      |
| Location(String aLatitude, String aLongitude, Long idReport) | Constructor que permite crear una ubicación con latitud, longitud e identificador de reporte. |
| Location(String aLatitude, String aLongitude, Long idReport) | Constructor vacío requerido por JPA para crear instancias de la entidad.                      |
| getLatitude()                                                | Obtiene la latitud registrada.                                                                |
| setLatitude(String latitude)                                 | Actualiza la latitud de la ubicación.                                                         |
| getLongitude()                                               | Obtiene la longitud registrada.                                                               |
| setLongitude(String longitude)                               | Actualiza la longitud de la ubicación.                                                        |
| getIdReport()                                                | Obtiene el identificador del reporte asociado.                                                |
| setIdReport(Long idReport)                                   | Actualiza el identificador del reporte asociado .                                             |
| getId()                                                      | Obtiene el identificador único heredado.                                                      |
| setId(Long id)                                               | Asigna el identificador único heredado.                                                       |

---
**Domain Services**

**Commands**
Los comandos representan intenciones de cambio dentro del bounded context. En este caso, se usan para crear ubicaciones y eliminar ubicaciones asociadas a un reporte.

### CreateLocationCommand

| Atributo  | Descripción                            | Tipo   |
|-----------|----------------------------------------|--------|
| latitude  | Latitud de la ubicación a registrar.   | String |
| longitude | Longitud de la ubicación a registrar.  | String |
| idReport  | Identificador del reporte relacionado. | Long   |

| Método                | Descripción                                          |
|-----------------------|------------------------------------------------------|
| CreateLocationCommand | Valida que `latitude` no sea nula o vacía.           |
| CreateLocationCommand | Valida que `longitude` no sea nula o vacía.          |
| CreateLocationCommand | Valida que `idReport` no sea nulo y sea mayor que 0. |

### DeleteAllLocationsByIdReportCommand

| Atributo | Descripción                                                   | Tipo |
|----------|---------------------------------------------------------------|------|
| idReport | Identificador del reporte cuyas ubicaciones serán eliminadas. | Long |

| Método                              | Descripción                                          |
|-------------------------------------|------------------------------------------------------|
| DeleteAllLocationsByIdReportCommand | Valida que `idReport` no sea nulo y sea mayor que 0. |

---

## Queries

Las queries representan solicitudes de lectura de información dentro del bounded context.

### GetAllLocationsQuery

| Query                | Descripción                                                          |
|----------------------|----------------------------------------------------------------------|
| GetAllLocationsQuery | Solicita la lista completa de ubicaciones almacenadas en el sistema. |

### GetDangerousLocationsQuery

| Atributo   | Descripción                                                               | Tipo   |
|------------|---------------------------------------------------------------------------|--------|
| minReports | Cantidad mínima de reportes para considerar una ubicación como peligrosa. | int    |

| Método                     | Descripción                                    |
|----------------------------|------------------------------------------------|
| GetDangerousLocationsQuery | Valida que `minReports` sea mayor o igual a 1. |

**Observación:** En el código actual, el parámetro `minReports` se valida en la query, pero no se utiliza directamente en el método del repositorio. Actualmente, el repositorio agrupa las ubicaciones por latitud y longitud y las ordena por cantidad de reportes, pero no aplica un filtro mínimo.

---

## Value Objects

En el código fuente revisado no se identifican Value Objects implementados explícitamente. Actualmente, la latitud y longitud se manejan como atributos `String` dentro del aggregate `Location`.

Sin embargo, a nivel de diseño táctico, se podría considerar el siguiente Value Object para mejorar la expresividad del dominio.

**Nombre:** `Coordinates`

**Descripción:** Representa las coordenadas geográficas de una ubicación. Este Value Object permitiría agrupar la latitud y longitud en un solo concepto del dominio, evitando que se manejen como valores separados sin semántica propia.

| Nombre    | Tipo   | Descripción               |
|-----------|--------|---------------------------|
| latitude  | String | Latitud de la ubicación.  |
| longitude | String | Longitud de la ubicación. |

| Nombre                                         | Descripción                                             |
|------------------------------------------------|---------------------------------------------------------|
| Coordinates(String latitude, String longitude) | Constructor que valida y crea un objeto de coordenadas. |
| latitude()                                     | Retorna la latitud.                                     |
| longitude()                                    | Retorna la longitud.                                    |

---

## Domain Services

Los servicios de dominio definen las operaciones principales del bounded context. En el proyecto se identifican dos interfaces principales: `LocationCommandService` y `LocationQueryService`.

### LocationCommandService

**Descripción:** Servicio de dominio encargado de definir las operaciones de escritura para las ubicaciones.

| Método                                              | Descripción                                                             |
|-----------------------------------------------------|-------------------------------------------------------------------------|
| handle(CreateLocationCommand command)               | Procesa la creación de una nueva ubicación a partir de un comando.      |
| handle(DeleteAllLocationsByIdReportCommand command) | Procesa la eliminación de todas las ubicaciones asociadas a un reporte. |

### LocationQueryService

**Descripción:** Servicio de dominio encargado de definir las operaciones de consulta sobre las ubicaciones.

| Método                                   | Descripción                                                                       |
|------------------------------------------|-----------------------------------------------------------------------------------|
| handle(GetAllLocationsQuery query)       | Obtiene todas las ubicaciones registradas.                                        |
| handle(GetDangerousLocationsQuery query) | Obtiene las ubicaciones agrupadas y ordenadas por cantidad de reportes asociados. |

---

### 5.3.2. Interface Layer

La capa de interfaz expone los endpoints REST del bounded context **Location**. Esta capa recibe solicitudes HTTP, transforma los recursos recibidos en comandos o queries y delega la lógica a los servicios correspondientes.

## Controller: LocationController

**Descripción:** El controlador `LocationController` expone las operaciones REST para crear, consultar y eliminar ubicaciones. Se encuentra bajo la ruta base:

| Método | Descripción | HTTP | Respuesta |
|--------|-------------|------|-----------|
| create(CreateLocationResource body) | Crea una nueva ubicación asociada a un reporte. | POST `/api/v1/locations` | 201 Created con `LocationResource` o 400 Bad Request |
| getAll() | Obtiene todas las ubicaciones registradas. | GET `/api/v1/locations` | 200 OK con lista de ubicaciones o 204 No Content |
| getDangerousLocations(int quantityReports) | Obtiene ubicaciones consideradas peligrosas según la cantidad de reportes. | GET `/api/v1/locations/dangerous?quantityReports=5` | 200 OK con lista de ubicaciones o 204 No Content |
| deleteAllByReportId(Long reportId) | Elimina todas las ubicaciones asociadas a un reporte específico. | DELETE `/api/v1/locations/report/{reportId}` | 200 OK, 400 Bad Request o 500 Internal Server Error |

---

## Resources

### CreateLocationResource

**Descripción:** Recurso utilizado para recibir los datos necesarios al crear una ubicación.

| Atributo   | Tipo   | Descripción                         |
|------------|--------|-------------------------------------|
| latitude   | String | Latitud de la ubicación.            |
| longitude  | String | Longitud de la ubicación.           |
| idReport   | Long   | Identificador del reporte asociado. |

### LocationResource

**Descripción:** Recurso utilizado para devolver información de una ubicación al cliente.

| Atributo   | Tipo   | Descripción                         |
|------------|--------|-------------------------------------|
| id         | Long   | Identificador de la ubicación.      |
| latitude   | String | Latitud registrada.                 |
| longitude  | String | Longitud registrada.                |
| idReport   | Long   | Identificador del reporte asociado. |

---

## Assemblers

| Clase                                      | Descripción                                                          |
|--------------------------------------------|----------------------------------------------------------------------|
| CreateLocationCommandFromResourceAssembler | Convierte un `CreateLocationResource` en un `CreateLocationCommand`. |
| LocationResourceFromEntityAssembler        | Convierte una entidad `Location` en un `LocationResource`.           |

---

### 5.3.3. Application Layer

La capa de aplicación contiene la lógica de aplicación del bounded context. En este caso, coordina el uso de repositorios, comandos, consultas y servicios externos. También se encarga de ejecutar operaciones transaccionales y conectar la lógica de dominio con la infraestructura.

## Command Services Implementation

### Clase: LocationCommandServiceImpl

**Descripción:** Implementa las operaciones de escritura definidas por `LocationCommandService`. Utiliza `LocationRepository` para persistir nuevas ubicaciones o eliminar ubicaciones asociadas a un reporte.

| Método                                              | Descripción                                                                                                                                                                 |
|-----------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| handle(CreateLocationCommand command)               | Crea una nueva instancia de `Location` usando la latitud, longitud e `idReport` recibidos en el comando. Luego la guarda en la base de datos mediante `LocationRepository`. |
| handle(DeleteAllLocationsByIdReportCommand command) | Elimina todas las ubicaciones asociadas al `idReport` recibido en el comando.                                                                                               |

---

## Query Services Implementation

### Clase: LocationQueryServiceImpl

**Descripción:** Implementa las operaciones de lectura definidas por `LocationQueryService`. Utiliza el repositorio para consultar ubicaciones y transformar resultados agrupados en objetos `Location`.

| Método                                   | Descripción                                                                                                                                               |
|------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| handle(GetAllLocationsQuery query)       | Retorna todas las ubicaciones almacenadas usando `locationRepository.findAll()`.                                                                          |
| handle(GetDangerousLocationsQuery query) | Obtiene ubicaciones agrupadas por latitud y longitud, ordenadas por cantidad de reportes. Luego transforma cada resultado en una instancia de `Location`. |

---

## Outbound Service

### Clase: ExternalReportService

**Descripción:** Servicio de aplicación encargado de comunicarse con el bounded context de Reports mediante `ReportServiceClient`. Se utiliza principalmente cuando el microservicio recibe un evento de reporte aprobado y necesita consultar los datos completos del reporte.

| Método                    | Descripción                                                                         |
|---------------------------|-------------------------------------------------------------------------------------|
| existsById(Long reportId) | Verifica si existe un reporte con el identificador indicado.                        |
| fetchById(Long reportId)  | Obtiene la información completa de un reporte desde el servicio externo de Reports. |

---

## Event Listeners

El bounded context **Location** también consume eventos mediante RabbitMQ. Esto permite que reaccione de forma asíncrona ante cambios ocurridos en el bounded context de Reports.

| Listener                    | Evento              | Descripción                                                                                                                              |
|-----------------------------|---------------------|------------------------------------------------------------------------------------------------------------------------------------------|
| ReportApprovedEventListener | ReportApprovedEvent | Cuando un reporte es aprobado, consulta el reporte mediante `ExternalReportService` y crea una ubicación si existen coordenadas válidas. |
| ReportDeletedEventListener  | ReportDeletedEvent  | Cuando un reporte es eliminado, elimina todas las ubicaciones asociadas a ese reporte.                                                   |
| ReportRejectedEventListener | ReportRejectedEvent | Cuando un reporte es rechazado, elimina todas las ubicaciones asociadas a ese reporte.                                                   |

---

### 5.3.4. Infrastructure Layer

La capa de infraestructura proporciona la implementación de persistencia y comunicación externa. En este bounded context, se utiliza **Spring Data JPA** para la persistencia de ubicaciones en MySQL, **OpenFeign** para la comunicación con Reports y **RabbitMQ** para la recepción de eventos.

## Repositorio: LocationRepository

**Descripción:** Repositorio basado en Spring Data JPA encargado de realizar operaciones CRUD sobre la entidad `Location`. Extiende de `JpaRepository<Location, Long>`, por lo que hereda métodos como `save`, `findAll`, `findById`, `deleteById`, entre otros.

| Método                             | Descripción                                                                                                                 |
|------------------------------------|-----------------------------------------------------------------------------------------------------------------------------|
| save(Location location)            | Guarda una nueva ubicación o actualiza una existente. Método heredado de `JpaRepository`.                                   |
| findAll()                          | Obtiene todas las ubicaciones almacenadas. Método heredado de `JpaRepository`.                                              |
| findById(Long id)                  | Busca una ubicación por su identificador. Método heredado de `JpaRepository`.                                               |
| deleteById(Long id)                | Elimina una ubicación por su identificador. Método heredado de `JpaRepository`.                                             |
| findDangerousLocations()           | Agrupa ubicaciones por latitud y longitud, cuenta la cantidad de reportes asociados y ordena el resultado de mayor a menor. |
| deleteAllByIdReport(Long idReport) | Elimina todas las ubicaciones asociadas a un reporte específico.                                                            |

---

## External Client: ReportServiceClient

**Descripción:** Cliente Feign utilizado para comunicarse con el bounded context de Reports. Permite consultar la existencia de un reporte y obtener sus datos.

| Método                       | Descripción                                              |
|------------------------------|----------------------------------------------------------|
| reportExists(Long reportId)  | Verifica si un reporte existe en el servicio de Reports. |
| getReportById(Long reportId) | Obtiene la información de un reporte específico.         |

---

## Fallback: ReportServiceClientFallback

**Descripción:** Clase de respaldo usada cuando el servicio de Reports no está disponible. Permite evitar fallos directos en la comunicación entre microservicios.

| Método                       | Descripción                                                                               |
|------------------------------|-------------------------------------------------------------------------------------------|
| reportExists(Long reportId)  | Retorna una respuesta alternativa cuando no se puede verificar la existencia del reporte. |
| getReportById(Long reportId) | Retorna una respuesta alternativa cuando no se puede obtener el reporte.                  |

### 5.3.5. Bounded Context Software Architecture Component Level Diagrams

**Backend**

Este diagrama muestra la arquitectura interna del microservicio Location Service. Se observa cómo el controlador REST recibe solicitudes, cómo los listeners consumen eventos desde RabbitMQ, cómo los servicios de aplicación coordinan comandos y consultas, y cómo el repositorio persiste información en la base de datos MySQL.

![backend-locationLocationBackendComponents-dark.png](assets/backend-locationLocationBackendComponents-dark.png)

**Web App**
Este diagrama representa cómo una aplicación web consume las funcionalidades del bounded context Location. La Web App no accede directamente a la base de datos, sino que utiliza el API Gateway para consultar ubicaciones o zonas peligrosas expuestas por el microservicio Location.

![web-app-locationLocationWebAppComponents-dark.png](assets/web-app-locationLocationWebAppComponents-dark.png)

**Mobile App**
Este diagrama muestra cómo una aplicación móvil consume el bounded context Location. La Mobile App puede mostrar ubicaciones en un mapa y consultar zonas peligrosas mediante peticiones HTTP hacia el API Gateway.

![mobile-c4-locationLocationMobileAppComponents-dark.png](assets/mobile-c4-locationLocationMobileAppComponents-dark.png)

### 5.3.6. Bounded Context Software Architecture Code Level Diagrams

#### 5.3.6.1. Bounded Context Domain Layer Class Diagrams
El siguiente diagrama representa las clases principales de la capa de dominio del bounded context Location. Se muestra el aggregate Location, los comandos, las queries y los servicios de dominio que definen las operaciones de escritura y lectura.

![class-diagram-location.png](assets/class-diagram-location.png)
#### 5.3.6.2. Bounded Context Database Design Diagram
El bounded context Location utiliza una tabla principal llamada locations. Esta tabla almacena las coordenadas geográficas asociadas a reportes. Además, incluye campos de auditoría heredados desde **AuditableAbstractAggregateRoot**.

![database-diagram.png](assets/database-diagram.png)

## 5.4. Bounded Context: Report

El Bounded Context de **Report** es el núcleo principal del negocio en PeaceApp, ya que gestiona la información ingresada por los ciudadanos al reportar incidentes. Este contexto contiene los datos clave del sistema, como el tipo de reporte, descripción, evidencia, ubicación, estado y usuario asociado. Además, otros contextos dependen de él: **Location** utiliza sus coordenadas para mostrar reportes en el mapa, mientras que **Alert** genera avisos a partir de reportes cercanos. Por ello, Report es la fuente principal de información operativa del sistema.

### 5.4.1. Domain Layer


La capa de dominio del bounded context Report encapsula la lógica principal del negocio relacionada con la gestión de reportes de incidentes. La entidad principal es Report, la cual representa la información registrada por un ciudadano, incluyendo su descripción, tipo, ubicación, evidencia y estado dentro del flujo de revisión.

**Aggregate:** Report

**Descripción:** Representa un reporte de incidente creado por un usuario dentro de PeaceApp.

|**Atributo**|**Descripción**|**Tipo**|
| :-: | :-: | :-: |
|id|Identificador único del reporte|Long|
|title|Título breve del reporte|String|
|description|Descripción detallada del incidente|String|
|location|Descripción textual de la ubicación del incidente|String|
|type|Tipo de incidente reportado|ReportType|
|userId|ID del usuario que creó el reporte|Long|
|imageUrl|URL de la imagen o evidencia asociada al reporte|String|
|latitude|Latitud donde ocurrió el incidente|String|
|longitude|Longitud donde ocurrió el incidente|String|
|state|Estado actual del reporte|ReportState|
|rejectionReason|Motivo de rechazo del reporte, en caso corresponda|String|

**Método**

|**Método**|**Descripción**|
| :-: | :-: |
|Report(String title, String description, String location, ReportType type, Long userId, String imageUrl, String latitude, String longitude)|Constructor que crea un nuevo reporte e inicializa su estado como PENDING.|
|void markInReview()|Cambia el estado del reporte de PENDING a IN_REVIEW.|
|void approve()|Aprueba un reporte que se encuentra en revisión y cambia su estado a APPROVED.|
|void reject(String reason)|Rechaza un reporte que se encuentra en revisión y registra el motivo del rechazo.|

**Value Objects**

**ReportState**

**Descripción:**
Representa el estado actual de un reporte dentro del proceso de revisión.

|**Valor**|**Descripción**|
| :- | :- |
|PENDING|El reporte fue creado y está pendiente de revisión.|
|IN_REVIEW|El reporte está siendo revisado.|
|APPROVED|El reporte fue aprobado y puede ser utilizado por otros contextos del sistema.|
|REJECTED|El reporte fue rechazado y no será considerado como válido.|

**ReportType**

**Descripción:**  
Representa el tipo de incidente registrado en el reporte.

|**Valor**|**Descripción**|
| :- | :- |
|ROBBERY|Reporte relacionado con robo o intento de robo.|
|ACCIDENT|Reporte relacionado con un accidente.|
|DARK_AREA|Reporte relacionado con una zona oscura o con baja iluminación.|
|HARASSMENT|Reporte relacionado con acoso.|
|OTHER|Reporte relacionado con otro tipo de incidente.|

**Domain Services**

Los Domain Services en este contexto son interfaces que definen operaciones de negocio relacionadas con el aggregate **Report**. Permiten separar las operaciones de comando y consulta, manteniendo organizada la lógica del bounded context.

**ReportCommandService**

**Descripción:**  
Interfaz que define las operaciones de escritura relacionadas con la gestión de reportes.

**Métodos:**

|**Nombre**|**Descripción**|
| :- | :- |
|Optional<Report> handle(CreateReportCommand command)|Procesa la creación de un nuevo reporte.|
|void handle(DeleteReportByIdCommand command)|Procesa la eliminación de un reporte mediante su identificador.|
|void handle(MarkReportInReviewCommand command)|Cambia el estado de un reporte a IN_REVIEW.|
|void handle(ApproveReportCommand command)|Aprueba un reporte que se encuentra en revisión.|
|void handle(RejectReportCommand command)|Rechaza un reporte y registra el motivo correspondiente.|

**ReportQueryService**

**Descripción:**  
Interfaz que define las operaciones de lectura relacionadas con los reportes.

**Métodos:**

|**Nombre**|**Descripción**|
| :- | :- |
|Optional<Report> handle(GetReportByIdQuery query)|Obtiene un reporte mediante su identificador.|
|List<Report> handle(GetReportsByUserIdQuery query)|Obtiene los reportes asociados a un usuario específico.|
|List<Report> handle(GetAllReportsQuery query)|Obtiene todos los reportes registrados en el sistema.|
|List<Report> handle(GetPublicReportsQuery query)|Obtiene los reportes públicos disponibles para consulta.|

**Domain Events**

Los eventos de dominio permiten comunicar cambios importantes ocurridos en el bounded context **Report** hacia otros contextos del sistema, como **Location** y **Alert**.

|**Evento**|**Descripción**|
| :- | :- |
|ReportCreatedEvent|Se genera cuando un nuevo reporte es creado.|
|ReportApprovedEvent|Se genera cuando un reporte es aprobado.|
|ReportRejectedEvent|Se genera cuando un reporte es rechazado.|
|ReportDeletedEvent|Se genera cuando un reporte es eliminado.|

### 5.4.2. Interface Layer

Esta capa actúa como punto de entrada para las operaciones externas relacionadas con los reportes de incidentes. A través del controlador REST, los clientes pueden crear reportes, consultar reportes existentes, cambiar su estado dentro del flujo de revisión y eliminar reportes cuando corresponda.

**Controlador: ReportController**

**Descripción:** Gestiona las operaciones REST relacionadas con la creación, consulta, revisión, aprobación, rechazo y eliminación de reportes.

|**Método**|**Descripción**|**HTTP**|**Respuesta**|
| :-: | :-: | :-: | :-: |
|reportExists(Long id)|Verifica si existe un reporte mediante su identificador.|GET /api/v1/reports/{id}/exists|Boolean|
|getPublicReports()|Devuelve los reportes públicos aprobados.|GET /api/v1/reports/public|Lista de ReportResource o 204 No Content|
|createReport(CreateReportResource resource)|Crea un nuevo reporte de incidente.|POST /api/v1/reports|201 Created con ReportResource o error|
|getReportById(Long id)|Devuelve un reporte específico mediante su identificador.|GET /api/v1/reports/{id}|ReportResource o 404 Not Found|
|markReportInReview(Long id)|Cambia el estado de un reporte a IN_REVIEW.|PUT /api/v1/reports/{id}/review|ReportResource actualizado o error|
|approveReport(Long id)|Aprueba un reporte que se encuentra en revisión.|PUT /api/v1/reports/{id}/approve|ReportResource actualizado o error|
|rejectReport(Long id, RejectReportResource resource)|Rechaza un reporte e incluye el motivo del rechazo.|PUT /api/v1/reports/{id}/reject|ReportResource actualizado o error|
|getReportsByUserId(Long userId)|Devuelve los reportes creados por un usuario específico.|GET /api/v1/reports/user/{userId}|Lista de ReportResource o 404 Not Found|
|getAllReports()|Devuelve todos los reportes registrados en el sistema.|GET /api/v1/reports|Lista de ReportResource o 204 No Content|
|deleteReportById(Long id)|Elimina un reporte mediante su identificador.|DELETE /api/v1/reports/{id}|Mensaje de confirmación o error|

### 5.4.3. Application Layer

Esta capa contiene la lógica de aplicación del bounded context **Report**. Se encarga de coordinar la creación, consulta, eliminación y actualización de estados de los reportes, utilizando los repositorios, servicios externos y publicación de eventos hacia otros bounded contexts del sistema.

**Clase: ReportCommandServiceImpl**

**Descripción:** Gestiona los comandos relacionados con la creación, eliminación y cambio de estado de los reportes. Además, valida la existencia del usuario antes de crear un reporte y publica eventos cuando ocurren cambios importantes.

|**Método**|**Descripción**|
| :-: | :-: |
|handle(CreateReportCommand)|Crea un nuevo reporte, valida que el usuario exista, inicializa el estado como PENDING, guarda el reporte y publica un ReportCreatedEvent.|
|handle(DeleteReportByIdCommand)|Elimina un reporte mediante su identificador y publica un ReportDeletedEvent.|
|handle(MarkReportInReviewCommand)|Cambia el estado de un reporte de PENDING a IN_REVIEW.|
|handle(ApproveReportCommand)|Actualiza el estado de un reporte a APPROVED y publica un ReportApprovedEvent.|
|handle(RejectReportCommand)|Rechaza un reporte, registra el motivo del rechazo y publica un ReportRejectedEvent.|

**Clase: ReportQueryServiceImpl**

**Descripción:** Gestiona las consultas relacionadas con los reportes registrados en el sistema.

|**Método**|**Descripción**|
| :-: | :-: |
|handle(GetReportByIdQuery)|Recupera un reporte mediante su identificador.|
|handle(GetReportsByUserIdQuery)|Recupera todos los reportes asociados a un usuario específico.|
|handle(GetAllReportsQuery)|Recupera todos los reportes registrados en el sistema.|
|handle(GetPublicReportsQuery)|Recupera los reportes públicos aprobados, filtrando aquellos con estado APPROVED.|

**Clase: ExternalUserService**

**Descripción:** Servicio de aplicación encargado de comunicarse con el bounded context de IAM para validar y obtener información de usuarios antes de crear reportes.

|**Método**|**Descripción**|
| :-: | :-: |
|existsById(Long userId)|Verifica si existe un usuario con el identificador indicado.|
|fetchById(Long userId)|Obtiene la información de un usuario específico desde el servicio externo de usuarios.|

### 5.4.4. Infrastructure Layer

La capa de infraestructura proporciona la implementación de persistencia, comunicación externa y mensajería para el bounded context **Report**. En este contexto se utiliza Spring Data JPA para la gestión de reportes, OpenFeign para la comunicación con el servicio de usuarios y RabbitMQ para la publicación de eventos hacia otros bounded contexts.

**Repositorio: ReportRepository**

**Descripción:** Administra la persistencia de la entidad Report.

|**Método**|**Descripción**|
| :-: | :-: |
|findAllByUserId(Long userId)|Recupera todos los reportes creados por un usuario específico.|
|findById(long reportId)|Recupera un reporte mediante su identificador.|
|findAllByState(ReportState state)|Recupera todos los reportes que coinciden con un estado determinado.|

**External Client: UserServiceClient**

**Descripción:** Cliente Feign utilizado para comunicarse con el bounded context de IAM. Permite validar la existencia de un usuario y obtener su información antes de crear un reporte.

|**Método**|**Descripción**|
| :-: | :-: |
|getUserById(Long id)|Obtiene la información de un usuario mediante su identificador.|
|userExists(Long id)|Verifica si existe un usuario con el identificador indicado.|

**DTO: UserDto**

**Descripción:** Representa la información recibida desde el servicio externo de usuarios.

|**Atributo**|**Descripción**|**Tipo**|
| :-: | :-: | :-: |
|id|Identificador del usuario|Long|
|name|Nombre del usuario|String|
|lastname|Apellido del usuario|String|
|email|Correo electrónico del usuario|String|
|phonenumber|Número telefónico del usuario|String|
|userId|Identificador adicional del usuario|String|
|profileImage|Imagen de perfil del usuario|String|

**Fallback: UserServiceClientFallback**

**Descripción:** Clase de respaldo utilizada cuando el servicio de usuarios no se encuentra disponible. Permite evitar fallos directos en la comunicación entre microservicios.

|**Método**|**Descripción**|
| :-: | :-: |
|getUserById(Long id)|Retorna información por defecto cuando no se puede obtener el usuario.|
|userExists(Long id)|Retorna false cuando no se puede verificar la existencia del usuario.|

**Messaging: ReportEventPublisher**

**Descripción:** Servicio encargado de publicar eventos relacionados con los reportes mediante RabbitMQ. Estos eventos permiten que otros bounded contexts reaccionen ante la creación, aprobación, rechazo o eliminación de reportes.

|**Método**|**Descripción**|
| :-: | :-: |
|publishReportCreated(ReportCreatedEvent event)|Publica un evento cuando un reporte es creado.|
|publishReportApproved(ReportApprovedEvent event)|Publica un evento cuando un reporte es aprobado.|
|publishReportRejected(ReportRejectedEvent event)|Publica un evento cuando un reporte es rechazado.|
|publishReportDeleted(ReportDeletedEvent event)|Publica un evento cuando un reporte es eliminado.|

<div style="page-break-after: always;"></div>

### 5.4.5. Bounded Context Software Architecture Component Level Diagrams

**Backend**

El Report Bounded Context centraliza la gestión de los reportes de incidentes dentro de PeaceApp, incluyendo su creación, consulta, revisión, aprobación, rechazo y eliminación. Este contexto contiene la información principal del negocio, ya que los reportes generados por los ciudadanos sirven como base para otros bounded contexts. Su arquitectura separa las responsabilidades entre la capa de interfaz, aplicación, dominio e infraestructura, permitiendo una gestión ordenada de la lógica de negocio y la persistencia de datos. Además, mediante el Report Event Publisher, este contexto publica eventos en RabbitMQ para que otros contextos, como Location y Alert, puedan reaccionar ante cambios importantes en los reportes.

!["Report Management Component Diagram"](assets/component-backend-report.png?raw=true)

<div style="page-break-after: always;"></div>

**WebApp**

El diagrama de componentes del Report Bounded Context en la WebApp muestra los componentes encargados de la visualización y gestión de reportes desde la aplicación web de PeaceApp. El Report Management Component permite listar y administrar los reportes registrados, mientras que el Report Detail Component permite revisar la información detallada de un incidente, incluyendo su ubicación, evidencia, estado y motivo de rechazo si corresponde. Asimismo, el Report Review Component permite realizar acciones de revisión, aprobación o rechazo de reportes. Estos componentes se apoyan en el Report Service, encargado de coordinar la comunicación con la API RESTful, y en el Report Assembler, responsable de transformar los datos entre los modelos del frontend y los DTOs del backend.

!["Report Management WebApp Component Diagram"](assets/component-webApp-report.png?raw=true)

<div style="page-break-after: always;"></div>

**MobileApp**

El diagrama de componentes del Report Bounded Context en la MobileApp muestra los componentes utilizados por los ciudadanos para interactuar con los reportes desde la aplicación móvil. El Create Report Widget permite registrar nuevos incidentes mediante un formulario con información como título, descripción, tipo de reporte, evidencia y ubicación. El Report List Widget y el Report Detail Widget permiten visualizar los reportes creados por el usuario y consultar su estado dentro del flujo de revisión. Además, el Public Reports Widget permite visualizar reportes aprobados disponibles para la comunidad. Todos estos componentes se comunican con el Report Service, el cual gestiona las operaciones de reporte y utiliza el Report Assembler para transformar los datos recibidos o enviados hacia la API RESTful.

!["Report Management MobileApp Component Diagram"](assets/component-backend-mobileApp-report.png?raw=true)

<div style="page-break-after: always;"></div>

### 5.4.6. Bounded Context Software Architecture Code Level Diagrams

#### 5.4.6.1. Bounded Context Domain Layer Class Diagrams

El siguiente diagrama representa las clases principales de la capa de dominio del bounded context **Report**. Se muestra el aggregate **Report**, el cual hereda de **AuditableAbstractAggregateRoot** e incorpora los atributos y comportamientos necesarios para la gestión de reportes de incidentes. Asimismo, se incluyen las enumeraciones **ReportState** y **ReportType**, que permiten modelar el estado del reporte y la clasificación del incidente dentro del dominio.

![class-diagram-report.png](assets/class-diagram-report.png)

#### 5.4.6.2. Bounded Context Database Design Diagram

El bounded context **Report** utiliza una tabla principal llamada **reports**. Esta tabla almacena la información central de los reportes de incidentes, incluyendo título, descripción, tipo, ubicación, coordenadas, evidencia, estado y motivo de rechazo. Además, incluye campos de auditoría como **created_at** y **updated_at**. El campo **id_user** representa una referencia lógica al usuario que creó el reporte, perteneciente a otro bounded context del sistema.

![database-diagram-report.png](assets/database-diagram-report.png)

## 5.5. Bounded Context: Alert

### 5.5.1. Domain Layer
La capa de dominio encapsula la lógica central relacionada con la gestión de advertencias preventivas y señales de auxilio dentro de PeaceApp. La entidad principal es el aggregate `Alert`, que representa tanto las notificaciones generadas por el sistema (cuando un ciudadano entra a una zona de riesgo) como las emergencias emitidas voluntariamente por los usuarios a través del botón de pánico. Se utilizan enumeraciones (Value Objects) como `AlertType` y `AlertState` para definir la naturaleza y el ciclo de vida de la alerta.

**Aggregate:** Alert

**Descripción:** Representa una alerta preventiva o de emergencia registrada en el sistema.

|**Atributo**|**Descripción**|**Tipo**|
| :-: | :-: | :-: |
|id|Identificador único de la alerta|Long|
|type|Clasificación de la alerta (preventiva o emergencia)|AlertType|
|description|Descripción contextual de la alerta o emergencia|String|
|latitude|Latitud de la ubicación del usuario al momento de la alerta|String|
|longitude|Longitud de la ubicación del usuario al momento de la alerta|String|
|state|Estado actual de la atención de la alerta|AlertState|
|userId|ID del usuario ciudadano involucrado (referencia a IAM)|Long|
|reportId|ID del reporte asociado que originó la alerta (opcional, solo para preventivas)|Long|

**Método**

|**Método**|**Descripción**|
| :-: | :-: |
|Alert(AlertType type, String description, String latitude, String longitude, Long userId, Long reportId)|Constructor para inicializar una nueva alerta, estableciendo su estado inicial en ACTIVE.|
|void markAsAttended()|Cambia el estado de la alerta de ACTIVE a ATTENDED cuando la municipalidad toma el caso.|
|void markAsResolved()|Actualiza el estado a RESOLVED una vez que la situación de riesgo o emergencia ha concluido.|

**Value Objects**

**AlertState**

**Descripción:**
Representa el estado actual de la alerta dentro del flujo de atención de la municipalidad.

|**Valor**|**Descripción**|
| :- | :- |
|ACTIVE|La alerta ha sido generada y requiere atención o visibilidad inmediata.|
|ATTENDED|Un operador de la municipalidad ha visto la emergencia y está en proceso de gestionarla.|
|RESOLVED|La situación de emergencia o riesgo ha sido neutralizada o el evento ha concluido.|

**AlertType**

**Descripción:** Clasifica la naturaleza de la alerta generada en el sistema.

|**Valor**|**Descripción**|
| :- | :- |
|PREVENTIVE|Alerta generada automáticamente por el sistema cuando un usuario se acerca a un reporte peligroso.|
|EMERGENCY|Señal de auxilio enviada explícitamente por el ciudadano mediante el botón de emergencia.|

**Domain Services**

Los Domain Services en este contexto definen las operaciones de negocio que no pertenecen estrictamente a una sola entidad, dividiendo claramente la escritura (Commands) y la lectura (Queries) del sistema de alertas.

**AlertCommandService**

**Descripción:** Interfaz que define las operaciones de escritura relacionadas con la generación y gestión del ciclo de vida de las alertas.

**Métodos:**

|**Nombre**|**Descripción**|
| :- | :- |
|Optional<Alert> handle(CreatePreventiveAlertCommand command)|Procesa la creación de una advertencia automática por proximidad a una zona de riesgo.|
|Optional<Alert> handle(CreateEmergencyAlertCommand command)|Procesa la recepción de una señal de auxilio emitida por el botón de pánico de un ciudadano.|
|void handle(MarkAlertAsAttendedCommand command)|Actualiza el estado de una alerta a ATTENDED por parte de la municipalidad.|
|void handle(MarkAlertAsResolvedCommand command)|Finaliza el ciclo de vida de la alerta cambiándola a RESOLVED.|

**AlertQueryService**

**Descripción:** Interfaz que define las operaciones de lectura para consultar el historial o el estado en vivo de las alertas.

**Métodos:**

|**Nombre**|**Descripción**|
| :- | :- |
|Optional<Alert> handle(GetAlertByIdQuery query)|Recupera los detalles de una alerta específica mediante su identificador.|
|List<Alert> handle(GetAlertsByUserIdQuery query)|Obtiene el historial de alertas asociadas a un ciudadano específico.|
|List<Alert> handle(GetActiveEmergenciesQuery query)|Recupera todas las emergencias activas para mostrarlas en el dashboard municipal en tiempo real.|

**Domain Events**

Eventos clave que permiten al Bounded Context de Alert comunicar acciones relevantes a otros componentes de la arquitectura de PeaceApp de forma asíncrona.

|**Evento**|**Descripción**|
| :- | :- |
|EmergencyTriggeredEvent|Se emite cuando un ciudadano utiliza el botón de emergencia. Puede ser escuchado por el Notification Gateway para enviar SMS/WhatsApp.|
|AlertStateChangedEvent|Se genera cuando una municipalidad actualiza el estado de una alerta (ej. de ACTIVE a ATTENDED).|

### 5.5.2. Interface Layer

### 5.5.3. Application Layer

### 5.5.4. Infrastructure Layer

### 5.5.5. Bounded Context Software Architecture Component Level Diagrams

**Backend**

Es el motor transaccional y analítico de las alertas. Se encarga de procesar las señales de auxilio, evaluar reglas de negocio (como calcular la proximidad del usuario a zonas de riesgo apoyándose en el servicio de Locations y Reports), y gestionar los cambios de estado de las emergencias. Además, actúa como orquestador para despachar notificaciones hacia el exterior mediante pasarelas como WhatsApp y SMS.

![AlertBackendComponents.png](assets/AlertBackendComponents.png)

<div style="page-break-after: always;"></div>

**WebApp**

Proporciona a los gestores de seguridad municipal un dashboard interactivo donde pueden monitorear las emergencias activas sobre un mapa en tiempo real. Desde aquí, las autoridades pueden gestionar el ciclo de vida de cada alerta (por ejemplo, marcarla como "en proceso" o "atendida"), asegurando un seguimiento adecuado.

![AlertWebComponents.png](assets/AlertWebComponents.png)

<div style="page-break-after: always;"></div>

**MobileApp**
Su objetivo principal es brindar al ciudadano una interfaz rápida y accesible para enviar alertas de emergencia inmediatas junto con su geolocalización. Adicionalmente, funciona como un receptor activo que muestra notificaciones preventivas en tiempo real cuando el usuario ingresa a una zona catalogada como peligrosa.
![AlertMobileComponents.png](assets/AlertMobileComponents.png)

<div style="page-break-after: always;"></div>

### 5.5.6. Bounded Context Software Architecture Code Level Diagrams

#### 5.5.6.1. Bounded Context Domain Layer Class Diagrams

#### 5.5.6.2. Bounded Context Database Design Diagram
