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
-**Backend**

El Profile Bounded Context centraliza la gestión de la información de perfil de los usuarios, incluyendo su estructura de dominio, lógica de aplicación, almacenamiento persistente e interfaces expuestas vía HTTP. Su arquitectura facilita tanto el acceso directo desde aplicaciones cliente como la colaboración con otros contextos a través de su fachada de contexto, permitiendo así la reutilización controlada de funciones relacionadas con los perfiles sin romper la encapsulación.

!["Profile Management Component Diagram"](assets/component-backend-profile.png?raw=true)

<div style="page-break-after: always;"></div>

-**WebApp**

El diagrama de componentes del Profile Bounded Context describe la estructura de componentes dedicados a la gestión de perfiles de ciudadanos y municipalidades dentro de PeaceApp. En este contexto, el Citizen Profile Component permite a los ciudadanos visualizar y editar su información personal, mientras que el Municipality Profile Component permite a las municipalidades gestionar su información institucional. Ambos componentes se apoyan en los servicios Citizen Profile Service y Municipality Profile Service, encargados de coordinar las operaciones de negocio y la comunicación con la API RESTful. Asimismo, el componente Profile Assembler se encarga de transformar y mapear los datos entre los modelos del frontend y los DTOs utilizados por la API, estableciendo una arquitectura desacoplada, mantenible y escalable que facilita la evolución de las funcionalidades relacionadas con la gestión de perfiles.

!["Profile Management WebApp Component Diagram"](assets/component-webApp-profile.png?raw=true)

<div style="page-break-after: always;"></div>

-**MobileApp**

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

### 5.4.1. Domain Layer

### 5.4.2. Interface Layer

### 5.4.3. Application Layer

### 5.4.4. Infrastructure Layer

### 5.4.5. Bounded Context Software Architecture Component Level Diagrams

### 5.4.6. Bounded Context Software Architecture Code Level Diagrams

#### 5.4.6.1. Bounded Context Domain Layer Class Diagrams

#### 5.4.6.2. Bounded Context Database Design Diagram



## 5.5. Bounded Context: Alert

### 5.5.1. Domain Layer

### 5.5.2. Interface Layer

### 5.5.3. Application Layer

### 5.5.4. Infrastructure Layer

### 5.5.5. Bounded Context Software Architecture Component Level Diagrams

### 5.5.6. Bounded Context Software Architecture Code Level Diagrams

#### 5.5.6.1. Bounded Context Domain Layer Class Diagrams

#### 5.5.6.2. Bounded Context Database Design Diagram