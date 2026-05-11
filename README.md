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

### 5.3.1. Domain Layer

### 5.3.2. Interface Layer

### 5.3.3. Application Layer

### 5.3.4. Infrastructure Layer

### 5.3.5. Bounded Context Software Architecture Component Level Diagrams

### 5.3.6. Bounded Context Software Architecture Code Level Diagrams

#### 5.3.6.1. Bounded Context Domain Layer Class Diagrams

#### 5.3.6.2. Bounded Context Database Design Diagram



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