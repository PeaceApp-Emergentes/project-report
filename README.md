# Capítulo IV: Strategic-Level Software Design

## 4.1. Strategic-Level Attribute-Driven Design

### 4.1.1. Design Purpose

## 4.1.2. Attribute-Driven Design Inputs

### 4.1.2.1. Primary Functionality (Primary User Stories)

### 4.1.2.2. Quality Attribute Scenarios

### 4.1.2.3. Constraints

## 4.1.3. Architectural Drivers Backlog

## 4.1.4. Architectural Design Decisions

## 4.1.5. Quality Attribute Scenario Refinements
Como resultado del proceso de definición de atributos de calidad para PeaceApp, se han identificado y priorizado escenarios críticos que impactan directamente en las decisiones arquitectónicas del sistema. Estos escenarios están alineados con los principales architectural drivers, tales como la gestión de reportes, la geolocalización, la generación de alertas y la autenticación segura.

Los escenarios refinados permiten vincular los atributos de calidad (disponibilidad, rendimiento, escalabilidad, seguridad, usabilidad y mantenibilidad) con los bounded contexts definidos (IAM, Profiles, Reports, Alerts y Locations), así como con las tácticas arquitectónicas adoptadas (microservicios, JWT, cacheo, balanceo de carga, entre otros).

A continuación, se presentan los escenarios refinados priorizados, los cuales guían las decisiones de diseño del sistema.

SCENARIO 1 – DISPONIBILIDAD
<table style="border-collapse: collapse; width: 100%; font-family: Arial, sans-serif;">
  <thead>
    <tr>
      <th colspan="3" style="border: 1px solid #ccc; padding: 10px; text-align: left;">
        Scenario Refinement for Scenario 1
      </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="border: 1px solid #ccc; padding: 8px; font-weight: bold;">Scenario(s):</td>
      <td colspan="2" style="border: 1px solid #ccc; padding: 8px;">US06, US09, US13</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ccc; padding: 8px; font-weight: bold;">Business Goals:</td>
      <td colspan="2" style="border: 1px solid #ccc; padding: 8px;">Garantizar que los ciudadanos puedan reportar incidentes y recibir alertas en cualquier momento.</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ccc; padding: 8px; font-weight: bold;">Relevant Quality Attributes:</td>
      <td colspan="2" style="border: 1px solid #ccc; padding: 8px;">Disponibilidad, Confiabilidad</td>
    </tr>
    <!-- Scenario Components -->
    <tr>
      <td rowspan="5" style="border: 1px solid #ccc; padding: 8px; font-weight: bold; vertical-align: top;">
        Scenario Components
      </td>
      <td style="border: 1px solid #ccc; padding: 8px; font-weight: bold;">Stimulus:</td>
      <td style="border: 1px solid #ccc; padding: 8px;">Usuario intenta enviar un reporte o recibir una alerta</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ccc; padding: 8px; font-weight: bold;">Stimulus Source:</td>
      <td style="border: 1px solid #ccc; padding: 8px;">Usuario</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ccc; padding: 8px; font-weight: bold;">Artifact:</td>
      <td style="border: 1px solid #ccc; padding: 8px;">API Gateway, Reports Service, Alerts Service</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ccc; padding: 8px; font-weight: bold;">Response:</td>
      <td style="border: 1px solid #ccc; padding: 8px;">El sistema procesa la solicitud sin interrupciones</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ccc; padding: 8px; font-weight: bold;">Response Measure:</td>
      <td style="border: 1px solid #ccc; padding: 8px;">Disponibilidad ≥ 80.0%, sin caídas en operaciones críticas</td>
    </tr>
    <!-- Questions -->
    <tr>
      <td style="border: 1px solid #ccc; padding: 8px; font-weight: bold;">Questions & Issues</td>
      <td colspan="2" style="border: 1px solid #ccc; padding: 8px;">
        <p>¿Se implementará failover entre microservicios?</p>
        <p>¿Cómo se manejará caída del API Gateway?</p>
      </td>
    </tr>
  </tbody>
</table>

SCENARIO 2 – RENDIMIENTO
<table style="border-collapse: collapse; width: 100%; font-family: Arial, sans-serif;">
  <thead>
    <tr>
      <th colspan="3" style="border: 1px solid #ccc; padding: 10px; text-align: left;">
        Scenario Refinement for Scenario 2
      </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="border: 1px solid #ccc; padding: 8px; font-weight: bold;">Scenario(s):</td>
      <td colspan="2" style="border: 1px solid #ccc; padding: 8px;">US06, US07, TS04</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ccc; padding: 8px; font-weight: bold;">Business Goals:</td>
      <td colspan="2" style="border: 1px solid #ccc; padding: 8px;">Procesar reportes de incidentes de manera eficiente, garantizando tiempos de respuesta adecuados incluso bajo carga.</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ccc; padding: 8px; font-weight: bold;">Relevant Quality Attributes:</td>
      <td colspan="2" style="border: 1px solid #ccc; padding: 8px;">Rendimiento</td>
    </tr>
    <!-- Scenario Components -->
    <tr>
      <td rowspan="5" style="border: 1px solid #ccc; padding: 8px; font-weight: bold; vertical-align: top;">
        Scenario Components
      </td>
      <td style="border: 1px solid #ccc; padding: 8px; font-weight: bold;">Stimulus:</td>
      <td style="border: 1px solid #ccc; padding: 8px;">Envío de reporte con imagen y geolocalización</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ccc; padding: 8px; font-weight: bold;">Stimulus Source:</td>
      <td style="border: 1px solid #ccc; padding: 8px;">Usuario</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ccc; padding: 8px; font-weight: bold;">Artifact:</td>
      <td style="border: 1px solid #ccc; padding: 8px;">Reports Service, Locations Service, Base de datos</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ccc; padding: 8px; font-weight: bold;">Response:</td>
      <td style="border: 1px solid #ccc; padding: 8px;">El sistema procesa y almacena el reporte correctamente</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ccc; padding: 8px; font-weight: bold;">Response Measure:</td>
      <td style="border: 1px solid #ccc; padding: 8px;">Tiempo de respuesta ≤ 5 segundos bajo carga moderada (≈1000 usuarios concurrentes)</td>
    </tr>
    <!-- Questions -->
    <tr>
      <td style="border: 1px solid #ccc; padding: 8px; font-weight: bold;">Questions & Issues</td>
      <td colspan="2" style="border: 1px solid #ccc; padding: 8px;">
        <p>¿Las imágenes se almacenan directamente o vía servicio externo (ej. cloud storage)?</p>
        <p>¿Se requiere optimización de consultas para el mapa?</p>
      </td>
    </tr>
  </tbody>
</table>

SCENARIO 3 – SEGURIDAD
<table style="border-collapse: collapse; width: 100%; font-family: Arial, sans-serif;">
  <thead>
    <tr>
      <th colspan="3" style="border: 1px solid #ccc; padding: 10px; text-align: left;">
        Scenario Refinement for Scenario 3
      </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="border: 1px solid #ccc; padding: 8px; font-weight: bold;">Scenario(s):</td>
      <td colspan="2" style="border: 1px solid #ccc; padding: 8px;">US04, US05, TS01</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ccc; padding: 8px; font-weight: bold;">Business Goals:</td>
      <td colspan="2" style="border: 1px solid #ccc; padding: 8px;">Proteger datos de usuarios y accesos</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ccc; padding: 8px; font-weight: bold;">Relevant Quality Attributes:</td>
      <td colspan="2" style="border: 1px solid #ccc; padding: 8px;">Seguridad</td>
    </tr>
    <!-- Scenario Components -->
    <tr>
      <td rowspan="5" style="border: 1px solid #ccc; padding: 8px; font-weight: bold; vertical-align: top;">
        Scenario Components
      </td>
      <td style="border: 1px solid #ccc; padding: 8px; font-weight: bold;">Stimulus:</td>
      <td style="border: 1px solid #ccc; padding: 8px;">Intento de acceso con credenciales inválidas</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ccc; padding: 8px; font-weight: bold;">Stimulus Source:</td>
      <td style="border: 1px solid #ccc; padding: 8px;">Atacante externo</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ccc; padding: 8px; font-weight: bold;">Artifact:</td>
      <td style="border: 1px solid #ccc; padding: 8px;">IAM Service</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ccc; padding: 8px; font-weight: bold;">Response:</td>
      <td style="border: 1px solid #ccc; padding: 8px;">Se bloquea acceso y se registra intento</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ccc; padding: 8px; font-weight: bold;">Response Measure:</td>
      <td style="border: 1px solid #ccc; padding: 8px;">100% accesos no autorizados bloqueados</td>
    </tr>
    <!-- Questions -->
    <tr>
      <td style="border: 1px solid #ccc; padding: 8px; font-weight: bold;">Questions & Issues</td>
      <td colspan="2" style="border: 1px solid #ccc; padding: 8px;">
        <p>¿Se implementará rate limiting?</p>
        <p>¿Se usará refresh token?</p>
      </td>
    </tr>
  </tbody>
</table>

## 4.2. Strategic-Level Domain-Driven Design

### 4.2.1. EventStorming

### 4.2.2. Candidate Context Discovery

### 4.2.3. Domain Message Flows Modeling

### 4.2.4. Bounded Context Canvases

### 4.2.5. Context Mapping
El sistema PeaceApp se estructura en cinco bounded contexts principales: IAM, Profiles, Reports, Alerts y Locations, los cuales colaboran entre sí para cumplir los objetivos del dominio. A continuación, se describen sus relaciones y las decisiones de diseño adoptadas.

IAM ↔ Profiles (Customer/Supplier)

El bounded context IAM depende de Profiles para acceder a información del usuario, como datos personales o preferencias, necesarios durante procesos de autenticación y autorización. En este caso, se adopta el patrón Customer/Supplier, donde IAM actúa como cliente y Profiles como proveedor.

Este patrón se justifica porque IAM requiere que Profiles mantenga un contrato estable, ya que cualquier cambio en la estructura de datos del perfil podría afectar directamente los procesos de autenticación. Por tanto, Profiles debe garantizar cierta estabilidad y compatibilidad hacia IAM.
<hr>

Reports ↔ Profiles (Conformist)

El bounded context Reports necesita acceder a información básica del usuario (por ejemplo, el autor de un reporte). Para ello, consume datos de Profiles utilizando el patrón Conformist.

Se eligió este patrón porque Reports no necesita controlar el modelo de Profiles ni traducirlo, ya que solo consume datos simples. Implementar una capa de anticorrupción (ACL) sería innecesario y aumentaría la complejidad sin aportar valor significativo.
<hr>

Reports ↔ Locations (Conformist)

El bounded context Reports también depende de Locations para asociar coordenadas geográficas a los incidentes reportados. En este caso, se adopta nuevamente el patrón Conformist.

Esto se debe a que el modelo geoespacial definido por Locations es lo suficientemente estándar y estable, por lo que Reports puede adaptarse directamente sin necesidad de transformación. Además, introducir una ACL aquí generaría sobrecarga innecesaria.
<hr>

Alerts ↔ Reports (Customer/Supplier)

El bounded context Alerts depende de Reports para generar notificaciones basadas en incidentes registrados. Se utiliza el patrón Customer/Supplier, donde Alerts es el cliente y Reports el proveedor.

Este patrón se justifica porque Alerts depende fuertemente del modelo de Reports, especialmente en la estructura de los incidentes. Por tanto, Reports debe garantizar estabilidad en su contrato para no afectar la generación de alertas.
<hr>

Alerts ↔ Locations (Conformist)

El bounded context Alerts utiliza datos geográficos proporcionados por Locations para determinar si un usuario se encuentra en una zona de riesgo. Se adopta el patrón Conformist.

Esto se debe a que Alerts solo consume coordenadas y datos espaciales sin necesidad de lógica compleja, por lo que puede adaptarse directamente al modelo de Locations.
<hr>

Alerts ↔ Profiles (Conformist)

El bounded context Alerts también consume información de usuario desde Profiles para personalizar las notificaciones. Se aplica el patrón Conformist.

La elección se debe a que los datos requeridos son simples y no justifican una capa de traducción, manteniendo así la arquitectura más ligera.
<hr>

Locations ↔ Mapbox (Anti-Corruption Layer)

El bounded context Locations se integra con el sistema externo Mapbox para obtener datos geoespaciales y visualización de mapas. En este caso, se aplica el patrón Anti-Corruption Layer (ACL).

Este patrón es fundamental porque permite proteger el dominio interno de cambios o inconsistencias en la API externa. La ACL actúa como una capa de traducción que adapta los datos de Mapbox al modelo interno del sistema, evitando acoplamiento directo.
<hr>

Alerts ↔ WhatsApp API / SMS Gateway (Anti-Corruption Layer)

El bounded context Alerts se comunica con sistemas externos como WhatsApp API y SMS Gateway para enviar notificaciones a los usuarios. Aquí también se aplica el patrón Anti-Corruption Layer (ACL).

Esto se debe a que estos servicios externos tienen interfaces y modelos propios, que no deben afectar el dominio interno. La ACL permite aislar estas dependencias y mantener la independencia del sistema.
<hr>

Clasificación Estratégica de los Bounded Contexts

Siguiendo los principios de Domain-Driven Design, los bounded contexts se clasifican según su importancia en el negocio:

Core Domain: Reports

El bounded context Reports constituye el núcleo del sistema, ya que implementa la funcionalidad principal de la aplicación: el registro y gestión de incidentes de seguridad.

Este contexto aporta el mayor valor al negocio, dado que:

Permite generar información sobre zonas peligrosas
En el mapa se visualizan los reportes de las zonas
Es la base para la generación de alertas

Por ello, se considera el Core Domain, y debe recibir la mayor atención en términos de diseño y evolución.
<hr>

Supporting Domains: Alerts y Locations

Los bounded contexts Alerts y Locations se clasifican como Supporting Domains, ya que complementan al dominio principal:

Alerts permite notificar a los usuarios sobre riesgos
Locations gestiona la información geoespacial

Si bien son importantes, no representan la esencia del negocio, sino que soportan la funcionalidad principal de Reports.
<hr>

Generic Domains: IAM y Profiles

Los bounded contexts IAM y Profiles se clasifican como Generic Domains, ya que implementan funcionalidades comunes que no son específicas del negocio:

IAM gestiona autenticación y autorización
Profiles gestiona datos de usuario

Estos dominios:

Son reutilizables en múltiples sistemas
No diferencian a PeaceApp de otras aplicaciones

Por ello, no requieren un diseño altamente especializado.

![](assets/contextMap-peaceApp.png?raw=true)

## 4.3. Software Architecture

### 4.3.1. Software Architecture System Landscape Diagram

### 4.3.2. Software Architecture Context Level Diagrams

### 4.3.3. Software Architecture Container Level Diagrams

### 4.3.4. Software Architecture Deployment Diagrams
