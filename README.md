# Capítulo IV: Strategic-Level Software Design

## 4.1. Strategic-Level Attribute-Driven Design

### 4.1.1. Design Purpose
El propósito del diseño arquitectónico de PeaceApp es proporcionar una base estructurada, segura, escalable y mantenible que soporte de manera eficiente la gestión de reportes ciudadanos de incidentes en tiempo real. A través del uso de principios de diseño como la separación de responsabilidades, el uso de patrones arquitectónicos adecuados y la definición de capas claras, se busca facilitar la implementación coherente de funcionalidades esenciales orientadas a la seguridad ciudadana, garantizando un desarrollo modular y alineado con las necesidades reales del dominio urbano. Esta arquitectura permitirá al equipo de desarrollo incorporar nuevas características, escalar servicios críticos como notificaciones en tiempo real o geolocalización, integrar sistemas externos (por ejemplo, autoridades locales o servicios de emergencia), y mantener una alta calidad en la experiencia de usuario, incluso ante cambios en el entorno tecnológico o en los requerimientos sociales.

Para este proceso de diseño, nos basaremos en los siguientes objetivos y principios:

- Escalabilidad y Flexibilidad: La arquitectura debe adaptarse a cambios futuros, permitiendo la integración de nuevas funcionalidades y la expansión del sistema sin comprometer rendimiento o estabilidad.
- Modularidad y Reusabilidad: Fomentar un diseño modular que permita la reutilización de componentes y la incorporación de nuevas funcionalidades sin afectar el sistema existente.
- Desacoplamiento: Reducir dependencias entre componentes, facilitando actualizaciones, mantenimiento y resolución de problemas.
- Seguridad y Protección de Datos: Garantizar la protección de la información sensible de los usuarios mediante mecanismos de seguridad robustos y políticas de privacidad estrictas.
- Interoperabilidad: Asegurar que PeaceApp pueda interactuar con plataformas externas, dispositivos móviles y servicios de terceros, especialmente con autoridades y sistemas de emergencia.
- Mantenibilidad y Actualización Continua: Diseñar con visión a largo plazo para facilitar el mantenimiento y la evolución constante del sistema ante nuevas necesidades sociales o tecnológicas.
- Alta Disponibilidad y Confiabilidad: Garantizar que el sistema esté operativo de manera continua, incluso en contextos de alta demanda o emergencias.
- Experiencia de Usuario Óptima: Proporcionar una interacción fluida, intuitiva y confiable que incentive la participación ciudadana y eleve los niveles de confianza en la aplicación.


## 4.1.2. Attribute-Driven Design Inputs

### 4.1.2.1. Primary Functionality (Primary User Stories)
A continuación, se detallan las funciones esenciales (user stories) que influyen de manera directa en la organización del sistema y orientan las decisiones del diseño arquitectónico de PeaceApp.

| User Story ID | Título | Descripción |
|---------------|----------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| US04          | Registro de Usuarios | Como usuario, quiero poder registrarme en la aplicación, para acceder a las funcionalidades de PeaceApp.|
| US05          | Iniciar Sesión       | Como usuario registrado, quiero poder iniciar sesión con mi correo y contraseña, para acceder a mi cuenta.|
| US06          | Generar Reporte de Incidentes  | Como usuario, quiero poder generar reportes de incidentes de seguridad, para contribuir a la actualización del mapa de calor.|
| US07          | Adjuntar Evidencia al Reporte  | Como usuario, quiero poder adjuntar fotos o videos al reporte, para dar mayor credibilidad y detalle al incidente reportado.|
| US08          | Visualización de Reportes        | Como ciudadano, quiero poder ver los reportes de otros usuarios sobre incidentes ocurridos en la zona, para estar al tanto de los eventos de seguridad.|
| US09          | Recibir Alertas de Zonas de Riesgo | Como ciudadano, quiero recibir alertas si me acerco a una zona de alto riesgo, para tomar las precauciones necesarias.|
| US10          | Compartir Ubicación con Contactos en la Aplicación Móvil | Como usuario de la aplicación móvil, quiero poder compartir mi ubicación con mis contactos cercanos, para que puedan monitorear mi trayecto y estar alertas ante cualquier peligro.|
| US13          | Acceder a Mapa con Reportes          | Como usuario, quiero poder ver un mapa interactivo con los reportes de incidentes en mi área, para tomar decisiones informadas sobre mi seguridad.|
| US15          | Filtrar Reportes         | Como usuario, quiero poder filtrar los reportes para ver todos los reportes o solo los que yo he creado, para gestionar mejor la información relevante según mis intereses.|
| US16          | Buscar Ubicación en el Mapa         | Como usuario, quiero poder explorar reportes de seguridad en diferentes zonas del mapa, para tomar decisiones informadas sobre mis desplazamientos.|

### 4.1.2.2. Quality Attribute Scenarios
Los atributos de calidad determinados para PeaceApp son los que se detallan a continuación:

**a. Disponibilidad:**

Este atributo se trata de la capacidad del sistema para estar disponible y operativo cuando se necesita.
En nuestra solución, esto significa que los usuarios podrán acceder a la aplicación en cualquier momento para reportar incidentes de seguridad o recibir alertas en tiempo real.
La facilidad con la que se puede garantizar esta disponibilidad depende de una infraestructura confiable, redundante y con monitoreo constante.
Una alta disponibilidad transmite confianza al ciudadano, asegurando que la plataforma siempre esté lista para su uso sin interrupciones inesperadas, especialmente en situaciones críticas.

**b. Rendimiento:**

Este atributo se trata de la capacidad del sistema para responder con rapidez y eficiencia bajo distintas cargas de trabajo.
En nuestra solución, buscamos que los reportes de incidentes, el envío de alertas y las consultas de mapas se procesen en tiempo mínimo, evitando retrasos que puedan afectar la seguridad del usuario.
La facilidad con la que se mantiene un buen rendimiento se logra mediante una arquitectura optimizada, balanceo de carga y pruebas de estrés que validen la eficiencia del sistema.
Un alto rendimiento mejora la experiencia de los usuarios, reduciendo la frustración y asegurando que la información crítica llegue de forma oportuna.

**c. Escalabilidad:**

Este atributo se trata de la capacidad del sistema para crecer y adaptarse a un aumento de usuarios, datos o transacciones.
En nuestra solución, esto significa que la aplicación puede ampliarse fácilmente para atender a más distritos o ciudades, soportando un mayor volumen de alertas y usuarios sin perder eficiencia.
La facilidad con la que se logra esta escalabilidad proviene de una arquitectura modular y distribuida, diseñada desde el inicio para soportar crecimiento.
Un sistema escalable asegura la continuidad del servicio y la adopción masiva sin necesidad de rediseños costosos.

**d. Mantenibilidad:**

Este atributo se trata de la capacidad del sistema para ser modificado fácilmente cuando se necesitan cambios o mejoras.
En nuestra solución, el código será limpio, modular y bien documentado, lo cual facilita la detección y corrección de errores, así como la integración de nuevas funcionalidades (ejemplo: chat en tiempo real con autoridades).
La facilidad con la que se mantiene y mejora el sistema reduce los tiempos de desarrollo, evita errores nuevos y permite adaptarse a las necesidades cambiantes de la comunidad.
Una alta mantenibilidad asegura la evolución constante de PeaceApp con bajo riesgo y esfuerzo.

**e. Usabilidad:**

Este atributo se trata de la capacidad del sistema para ser entendido y utilizado con facilidad por los usuarios finales.
En nuestra solución, nos enfocamos en una interfaz clara, intuitiva y accesible, que permita a los ciudadanos reportar incidentes o consultar alertas de manera rápida incluso en momentos de emergencia.
La facilidad con la que un usuario interactúa con la aplicación depende de un diseño UX/UI bien estructurado, retroalimentación visual clara y soporte para accesibilidad.
Una alta usabilidad fomenta la adopción de la plataforma, reduce la curva de aprendizaje y genera confianza en los ciudadanos para seguir utilizándola.

**f. Seguridad:**

Este atributo se trata de la capacidad del sistema para proteger la confidencialidad, integridad y disponibilidad de la información.
En nuestra solución, esto implica implementar autenticación robusta, encriptación de datos y control de acceso según roles (usuario, autoridad, administrador).
La facilidad con la que se garantiza la seguridad depende de políticas claras de protección de datos y del uso de estándares internacionales (ejemplo: OWASP).
Una alta seguridad incrementa la confianza de los usuarios y asegura que la información sensible no sea vulnerada ni utilizada de forma indebida.

**a. Disponibilidad:**

| **Elemento**            | **Detalle**                                                                 |
|--------------------------|-----------------------------------------------------------------------------|
| **Escenario**           | Un ciudadano necesita reportar un incidente en la calle a través de la app. |
| **Fuente de Estímulo**  | Ciudadano que usa la app                                                    |
| **Estímulo**            | Intenta acceder a la app para reportar un incidente en la calle             |
| **Medioambiente**       | El sistema se encuentra bajo condiciones normales de operación              |
| **Artefacto**           | Servidor y frontend de PeaceApp                                             |
| **Respuesta**           | El sistema responde y permite el registro del incidente sin caída del servicio |
| **Medida de Respuesta** | Tiempo de disponibilidad ≥ 99.5% mensual                                    |

**b. Rendimiento:**

| **Elemento**            | **Detalle**                                                                 |
|--------------------------|-----------------------------------------------------------------------------|
| **Escenario**           | Un usuario envía un reporte con foto y geolocalización en horario concurrido. |
| **Fuente de Estímulo**  | Usuario enviando un reporte                                                 |
| **Estímulo**            | Envío de reporte de incidente con foto y geolocalización                    |
| **Medioambiente**       | Sistema con 1000 usuarios concurrentes                                      |
| **Artefacto**           | API de backend y base de datos                                              |
| **Respuesta**           | Procesa y guarda el reporte en la base de datos                             |
| **Medida de Respuesta** | Tiempo de respuesta ≤ 3 segundos                                            |

**c. Escalabilidad:**

| **Elemento**            | **Detalle**                                                                 |
|--------------------------|-----------------------------------------------------------------------------|
| **Escenario**           | El servicio se expande a nuevos distritos y la cantidad de usuarios se duplica. |
| **Fuente de Estímulo**  | Municipalidad que amplía la cobertura                                       |
| **Estímulo**            | Aumento de usuarios al extender el servicio a nuevos distritos              |
| **Medioambiente**       | Sistema en crecimiento exponencial de usuarios y transacciones              |
| **Artefacto**           | Arquitectura distribuida en la nube                                         |
| **Respuesta**           | Se despliegan nuevas instancias y balanceadores automáticamente             |
| **Medida de Respuesta** | El sistema soporta +10,000 usuarios sin degradación notable del rendimiento |

**d. Mantenibilidad:**

| **Elemento**            | **Detalle**                                                                 |
|--------------------------|-----------------------------------------------------------------------------|
| **Escenario**           | El equipo de desarrollo necesita agregar un módulo de comunicación con autoridades. |
| **Fuente de Estímulo**  | Equipo de desarrollo                                                        |
| **Estímulo**            | Necesidad de agregar un nuevo módulo de comunicación directa con autoridades |
| **Medioambiente**       | Sistema en operación con versiones previas estables                         |
| **Artefacto**           | Código fuente modular de PeaceApp                                           |
| **Respuesta**           | Se implementa el nuevo módulo sin afectar funcionalidades existentes        |
| **Medida de Respuesta** | Cambios implementados en ≤ 2 sprints sin errores críticos en producción     |

**e. Usabilidad:**

| **Elemento**            | **Detalle**                                                                 |
|--------------------------|-----------------------------------------------------------------------------|
| **Escenario**           | Un ciudadano bajo estrés necesita presionar el botón de emergencia.          |
| **Fuente de Estímulo**  | Ciudadano que reporta un incidente                                          |
| **Estímulo**            | Dificultad para ubicar el botón de “Emergencia”                             |
| **Medioambiente**       | App utilizada en situación de estrés (robo, accidente, etc.)                |
| **Artefacto**           | Interfaz gráfica del usuario (UI/UX)                                        |
| **Respuesta**           | Se muestra un botón visible y accesible en la pantalla principal            |
| **Medida de Respuesta** | 90% de usuarios logra reportar en ≤ 5 segundos durante pruebas de usabilidad |

**f. Seguridad:**

| **Elemento**            | **Detalle**                                                                 |
|--------------------------|-----------------------------------------------------------------------------|
| **Escenario**           | Un atacante intenta ingresar al sistema usando credenciales robadas.        |
| **Fuente de Estímulo**  | Atacante externo                                                            |
| **Estímulo**            | Intenta acceder a la base de datos con credenciales robadas                 |
| **Medioambiente**       | Bajo ataque de fuerza bruta en horario crítico                              |
| **Artefacto**           | Sistema de autenticación y base de datos                                    |
| **Respuesta**           | Bloquea el intento, activa alertas y mantiene la integridad de los datos    |
| **Medida de Respuesta** | 100% de accesos no autorizados bloqueados; logs generados y auditados en < 1min |

### 4.1.2.3. Constraints
La tabla a continuación muestra las limitaciones que deben considerarse dentro del desarrollo de PeaceApp.

| **ID**   | **Constraint** | **Restricción** | **Impacto** |
|----------|----------------|-----------------|-------------|
| **CON-01** | Conectividad Urbana Variable | En muchas zonas urbanas la conectividad móvil puede ser inestable o intermitente. | La aplicación debe permitir funcionamiento básico offline (ej. registro temporal de incidentes) y sincronización automática cuando haya conexión. |
| **CON-02** | Presupuesto y Recursos | El proyecto cuenta con recursos financieros y humanos limitados. | Se priorizará el uso de tecnologías open source y librerías con soporte comunitario/comercial, enfocándose en un MVP funcional antes de ampliar funcionalidades. |
| **CON-03** | Cumplimiento Normativo en Protección de Datos | PeaceApp debe cumplir con la Ley de Protección de Datos Personales en Perú (Ley N.° 29733) y normativas internacionales de privacidad. | Requiere incorporar cifrado de datos, autenticación segura, consentimiento explícito de usuarios y auditoría de accesos. |
| **CON-04** | Integración con Infraestructura Municipal y Policial | La aplicación busca colaborar con entidades municipales y policiales que tienen sus propios sistemas de información. | Es necesario desarrollar APIs estandarizadas y protocolos de intercambio de datos, garantizando compatibilidad e interoperabilidad. |
| **CON-05** | Diversidad de Dispositivos | Los usuarios utilizarán teléfonos móviles de diferentes gamas (alta, media, baja). | La aplicación debe ser ligera, optimizada en rendimiento y compatible con Android/iOS en versiones aún usadas en el mercado. |
| **CON-06** | Tiempo de Entrega | El proyecto debe contar con un prototipo funcional para ser validado en pruebas piloto en un plazo corto. | Se debe adoptar un enfoque ágil (Scrum/Lean UX), entregando un MVP con las funcionalidades críticas: alertas, geolocalización y mapa de incidentes. |
| **CON-07** | Sostenibilidad a Largo Plazo | La continuidad del sistema depende de su mantenimiento y financiamiento posterior. | Influirá en la elección de arquitecturas escalables y de bajo mantenimiento, así como en la búsqueda de convenios con municipalidades y ONGs. |

## 4.1.3. Architectural Drivers Backlog

| Driver ID | Título | Descripción | Importancia (Alta/Media/Baja) | Complejidad Técnica (Alta/Media/Baja) |
|----------|--------|-------------|-------------------------------|---------------------------------------|
| DR-01 | Escalabilidad | Capacidad del sistema para soportar el crecimiento de usuarios ciudadanos, usuarios municipales, reportes, emergencias y consultas concurrentes sin degradar significativamente el rendimiento. | Alta | Alta |
| DR-02 | Disponibilidad | Capacidad de la solución para mantenerse operativa en funcionalidades críticas como visualización de reportes, envío de emergencias y acceso al dashboard municipal, incluso ante fallos parciales. | Alta | Alta |
| DR-03 | Mantenibilidad | Facilidad para modificar, extender y corregir el sistema, incorporando nuevas funcionalidades como IA, validación de evidencia o nuevos módulos municipales sin afectar gravemente el resto de la solución. | Alta | Media |
| DR-04 | Despliegue continuo | Capacidad del sistema para permitir actualizaciones frecuentes y controladas de sus componentes, reduciendo el impacto de cambios en producción y facilitando pipelines de integración y entrega continua. | Media | Media |
| DR-05 | Alineación con DDD | Capacidad de estructurar la solución en bounded contexts claros, como Users, Reports, Alerts, Emergencies, Municipal Management e Intelligent Services, manteniendo cohesión de dominio y bajo acoplamiento. | Alta | Alta |
| DR-06 | Seguridad | Capacidad del sistema para proteger datos sensibles, gestionar autenticación y autorización mediante JWT y roles diferenciados, y asegurar el acceso controlado a reportes, evidencia y funciones municipales. | Alta | Alta |
| DR-07 | Experiencia de usuario | Capacidad de ofrecer una interacción rápida, clara y confiable tanto para ciudadanos como para usuarios municipales, especialmente en funciones críticas como reportes, alertas, chatbot y emergencias. | Alta | Media |
| DR-08 | Integración | Capacidad para interoperar con servicios externos y componentes especializados, como API Gateway, Mapbox, servicios de notificación, message broker y microservicios de IA para NLP y clasificación de imágenes. | Alta | Alta |
| DR-09 | Portabilidad | Capacidad de reutilizar la lógica de negocio y los servicios en distintos clientes, como aplicación web, aplicación móvil y dashboard municipal, manteniendo consistencia funcional. | Media | Media |
| DR-10 | Trazabilidad | Capacidad de registrar y rastrear eventos relevantes del sistema, como creación de reportes, cambios de estado, envío de emergencias, validaciones automáticas y acciones municipales, para auditoría y seguimiento. | Alta | Media |

## 4.1.4. Architectural Design Decisions

| Driver ID | Título | Monolito Modular (DDD) - Pro | Monolito Modular (DDD) - Con | Microservicios - Pro | Microservicios - Con |
|----------|--------|------------------------------|------------------------------|---------------------|---------------------|
| DR-01 | Escalabilidad | Permite escalar el sistema como una sola unidad y mantener simplicidad inicial. | Obliga a escalar todo el sistema aunque solo crezcan módulos específicos como reportes o emergencias. | Permite escalar de forma independiente módulos como Reports, Emergencies, Alerts o IA. | Incrementa la complejidad operativa y de infraestructura. |
| DR-02 | Disponibilidad | Un solo despliegue facilita administración y monitoreo centralizado. | Un fallo crítico puede comprometer toda la aplicación. | Aísla fallos entre servicios y reduce el impacto global de errores. | Requiere mecanismos adicionales de tolerancia a fallos y observabilidad distribuida. |
| DR-03 | Mantenibilidad | La modularización interna facilita ordenar el código en etapas tempranas. | A medida que el sistema crece, los cambios cruzados aumentan el acoplamiento. | Cada servicio puede evolucionar de forma independiente según su dominio. | Exige una gobernanza fuerte para mantener coherencia entre servicios. |
| DR-04 | Despliegue continuo | El pipeline es más simple al existir una sola unidad desplegable. | Un cambio menor obliga a desplegar todo el sistema. | Permite desplegar servicios específicos sin afectar el resto de la plataforma. | Hace más compleja la configuración de CI/CD y pruebas integradas. |
| DR-05 | Alineación con DDD | Permite representar bounded contexts como módulos internos. | Los límites de contexto pueden degradarse con el tiempo. | Cada microservicio puede representar un bounded context con mayor claridad. | Obliga a manejar coordinación entre contextos y consistencia eventual. |
| DR-06 | Seguridad | Facilita centralizar autenticación y control de acceso en una sola aplicación. | Una vulnerabilidad puede exponer una superficie mayor del sistema. | Permite aplicar aislamiento por servicio, roles y políticas específicas por dominio. | Requiere distribuir controles de seguridad, tokens y validaciones entre varios servicios. |
| DR-07 | Experiencia de usuario | Favorece una experiencia consistente al depender de una sola base backend. | Puede dificultar optimizar por separado funciones críticas como chatbot o emergencias. | Permite optimizar servicios críticos de manera independiente según el tipo de interacción. | Mayor latencia potencial por comunicación entre servicios. |
| DR-08 | Integración | Simplifica la exposición de APIs externas desde un solo backend. | La integración con múltiples servicios externos puede volver más rígida la arquitectura. | Facilita integrar componentes especializados como IA, mensajería, mapas y notificaciones. | Aumenta la cantidad de contratos, endpoints y dependencias a mantener. |
| DR-09 | Portabilidad | Backend central reutilizable para varios clientes con menor esfuerzo inicial. | Puede concentrar demasiadas responsabilidades y crecer de forma rígida. | Permite exponer servicios reutilizables para web, móvil y dashboard municipal. | Puede requerir coordinación adicional entre clientes y servicios. |
| DR-10 | Trazabilidad | Logging centralizado más simple de implementar al inicio. | Puede dificultar distinguir eventos por dominio cuando el sistema crece. | Permite trazabilidad detallada por servicio y por flujo de negocio. | Requiere herramientas adicionales para correlacionar eventos interservicio. |

La arquitectura de microservicios es la opción más adecuada para PeaceApp, debido a que:

- Permite escalar de manera independiente los módulos críticos del sistema, como reportes, emergencias, alertas, gestión municipal y servicios inteligentes basados en IA.
- Favorece una mejor alineación con DDD, representando bounded contexts claros y reduciendo el acoplamiento entre dominios.
- Facilita la integración con componentes externos y especializados, como Mapbox, servicios de notificación, message broker y microservicios de chatbot y clasificación de imágenes.
- Mejora la mantenibilidad y evolución del sistema, permitiendo incorporar nuevas funcionalidades sin afectar toda la solución.
- Se adapta mejor al crecimiento progresivo del producto, especialmente al considerar dos segmentos principales: ciudadanos y municipalidades.

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
