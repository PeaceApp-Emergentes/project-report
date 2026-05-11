# Capítulo VI: Solution UX Design

## 6.1. Style Guidelines

En esta sección se definen las normas de estilo que servirán como base para el desarrollo del
producto desde cero. Estas directrices establecerán un marco común para la elección de tipografías,
tamaños y la paleta de colores, facilitando así el diseño de PeaceApp desde las primeras etapas
del prototipado. Serán una herramienta clave para todo el equipo, ya que proporcionarán una guía
clara sobre cómo aplicar los distintos componentes del diseño en cada área y sección de la
plataforma. Esto permitirá optimizar el tiempo de trabajo y asegurar una imagen visual coherente
en todo PeaceApp.

### 6.1.1. General Style Guidelines

**Branding:** Nuestro logo refleja de manera sencilla y memorable el
espíritu de PeaceApp. Con el nombre de la aplicación acompañado por un
símbolo que evoca un camino seguro y la inicial de \"PeaceApp\",
buscamos que los usuarios asocien rápidamente nuestra marca con su
propósito: guiarlos hacia un entorno más seguro. Queremos que nuestro
logo sea fácil de recordar, al igual que la seguridad que ofrecemos en
cada trayecto.

![](assets/PeaceAPP.jpg?raw=true)

**Tipografía:** Nuestro logotipo posee la fuente "Lora", el cual refleja
un estilo simple y moderno. Buscando promover un ambiente innovador e
interactivo de nuestra aplicación. En adición, y con respecto a nuestra
aplicación tanto la palabra "Peace" como "App", gozan de las mismas
características de formato. Se usa Poppins en el resto de la aplicación.

![](assets/Tipografia.png?raw=true)

**Colores:** Continuando con el objetivo de brindar una imagen que
influya principalmente la confianza y seguridad. Hemos decidido optar
por una paleta de colores que transmiten calma, seguridad, estabilidad y
profesionalismo. Por lo que serán tonos de azules hasta llegar a blanco.
Esta combinación no solo manifiesta el objetivo de nuestro proyecto,
sino también una atmosfera de serenidad y sofisticación. A continuación,
se muestra la paleta de colores que se usarán para desarrollar nuestra
aplicación.

<img src="assets/Colores.png?raw=true" width="300" height="400" />

### 6.1.2. Web, Mobile & Devices Style Guidelines

Para la versión web, se sigue un enfoque de diseño responsivo, adoptando el patrón visual en forma de “Z” para guiar la atención del usuario. Se prioriza la simplicidad, con fondos de color único y uso puntual de imágenes representativas por sección. Los botones utilizan tonos contrastantes dentro de la paleta azul para diferenciar acciones clave. Las pantallas emergentes oscurecen el fondo y emplean variantes de azul con mayor intensidad para destacar acciones importantes y asegurar la atención del usuario antes de continuar con la navegación.

Para dispositivos Android, adoptamos las recomendaciones de Material Design de Google. Este enfoque promueve una interfaz adaptable y visualmente clara, con especial énfasis en la jerarquía de elementos, animaciones suaves y uso eficiente del color. Se implementan componentes nativos como floating action buttons, snackbars y cards para asegurar familiaridad y coherencia. También se aprovechan las capacidades del sistema como servicios de ubicación, notificaciones push y navegación por gestos, garantizando una experiencia moderna, consistente y funcional en toda la plataforma.

Para dispositivos iOS, seguimos las recomendaciones de la Human Interface Guidelines (HIG) de Apple. El diseño se enfoca en la simplicidad, la legibilidad y la eficiencia del espacio. Se prioriza el uso de componentes nativos como tab bars, modals y gestos multitáctiles, garantizando una experiencia fluida y coherente con el ecosistema Apple. Además, se cuida el uso del safe area para asegurar una visualización óptima en dispositivos con diferentes tamaños de pantalla y elementos como el notch. Se promueve una navegación intuitiva, con transiciones suaves y retroalimentación clara para cada acción.


## 6.2. Information Architecture

### 6.2.1. Organization Systems

El sistema de organización se centrará en proporcionar la mejor experiencia al usuario en cuanto a la navegación y uso de las funcionalidades de seguridad. Nuestra plataforma está diseñada para que los usuarios puedan navegar, reportar incidentes y monitorear la seguridad de manera segura y cómoda.

> **Categorización de la Información:**

- Mapa de Seguridad: Categorizado por niveles de seguridad (alto, medio, bajo) y tipos de incidentes (robos, agresiones, etc.).

- Denuncias y Reportes: Categorizado por tipo de crimen y nivel de urgencia para facilitar la gestión y respuesta rápida.

> **Filtros y Búsqueda:**

- Filtros en el Mapa: Permiten a los usuarios filtrar incidentes por tipo de crimen, fecha y hora, y nivel de riesgo.

- Búsqueda Avanzada: Opción de búsqueda avanzada en el mapa y en los reportes para encontrar información específica o zonas críticas.

> **Interfaz de Usuario Intuitiva:**

- Menú Principal: Navegación clara con acceso rápido a las secciones principales como Mapa, Denuncia, Registro, y Contacto.

- Submenús Contextuales: Dentro de las secciones principales, submenús contextuales que guían a los usuarios a funcionalidades específicas, como diferentes tipos de incidentes en el Mapa o herramientas de denuncia.

> **Funcionalidades Específicas:**

- Información Detallada de Incidentes: Páginas individuales de incidentes que muestran descripciones completas, ubicación precisa y opciones de seguimiento.

### 6.2.2. Labeling Systems

Consideramos que la mejor opción para el desarrollo de nuestra plataforma será a través de un sistema de etiquetado. Utilizaremos etiquetas claras para describir cada funcionalidad y característica.

Ejemplos de etiquetas incluirán:

- Mapa de calor (Interactivo y a tiempo real)

- Denunciar (Denuncias a crímenes o incidentes)

- Información de Incidentes

- Registro de Usuarios

- Compartir Ubicación en Tiempo Real

### 6.2.3. Searching Systems

El sistema de búsqueda permitirá a los usuarios encontrar y filtrar información relevante sobre incidentes de seguridad en sus áreas de interés. Emplearemos diferentes formas de búsqueda:

> **Búsqueda por Incidente:**

- Los usuarios pueden buscar incidentes específicos como \"robos en Miraflores\" o \"agresiones en San Isidro\".

- Se proporcionan opciones de búsqueda avanzada para filtrar por fecha, hora, tipo de crimen, y nivel de riesgo.

> **Búsqueda por Ubicación:**

- Los usuarios pueden buscar incidentes por ubicación específica, como \"incidentes en el centro de Lima\".

- Se ofrecen filtros para seleccionar múltiples ubicaciones y tipos de incidentes a la vez. (esto lo quitan si quieren, son ideas q yo saque)

> **Búsqueda por Nivel de Seguridad:**

- Los usuarios pueden buscar zonas según su nivel de seguridad, como \"zonas de alto riesgo\" o \"áreas seguras en Lima\".

- La plataforma sugiere las zonas más seguras y permite explorar áreas con diferentes niveles de riesgo.

> **Búsqueda Avanzada:**

- Se ofrece una búsqueda avanzada que permite a los usuarios combinar múltiples criterios de búsqueda, como \"robos en Miraflores durante la noche\".

- Los resultados de la búsqueda avanzada se presentan en una lista organizada por relevancia y se pueden refinar aún más utilizando filtros adicionales.

> **Búsqueda por Fecha y Hora:**

- Los usuarios pueden buscar incidentes dentro de un rango de fechas y horas específicas.

- Se muestra información sobre la frecuencia de incidentes en determinados horarios, facilitando la toma de decisiones informadas.


### 6.2.4. SEO Tags and Meta Tags

Nuestros SEO Tags y Meta Tags, o, dicho de otra forma, las etiquetas clave que representarán el contenido de nuestra aplicación presentado tanto en nuestra aplicación web como en aplicación móvil serán:

> Landing Page:

- Title: PeaceApp

- Description: PeaceApp - Oficial Landing Page

- Keywords: Seguridad, Accidentes, Incidentes, Policía.

- Authors: PeaceApp Team

> Web application:

- Title: PeaceApp

- Description: PeaceApp - Oficial Web Site

- Keywords: Seguridad, Accidentes, Incidentes, Policía.

- Authors: PeaceApp Team

### 6.2.5. Navigation Systems

El sistema de navegación de PeaceApp debe proporcionar una experiencia fluida y fácil de usar para los usuarios, permitiéndoles encontrar rápidamente la información que buscan. Se describe de la siguiente forma:

> **Menú Principal:**

- Un menú principal ubicado en la parte superior izquierda (tanto con el logo como con solo el icono del logo), de cada página que incluye enlaces a las secciones clave como Mapa, Denuncia, Registro y Contacto.

- Cada elemento del menú principal está etiquetado de manera clara y concisa para facilitar la navegación.

> **Navegación Contextual:**

- Dentro de cada sección principal, se proporciona una navegación contextual que muestra submenús o enlaces relacionados con la sección actual, como diferentes categorías en el Mapa o herramientas de denuncia en la sección de Denuncia.

> **Botones de Acción Destacados:**

- En las páginas de Inicio y Mapa, se destacan botones de acción para dirigir a los usuarios a las funcionalidades principales de la plataforma.

- Estos botones de acción tienen un diseño llamativo y están estratégicamente ubicados para captar la atención de los usuarios.

> **Búsqueda y Filtros Visibles:**

- La barra de búsqueda y los filtros de navegación son visibles en todas las páginas para que los usuarios puedan buscar información específica o filtrar resultados según sus preferencias.

- Los filtros se presentan de manera clara y se pueden ajustar fácilmente para refinar los resultados de búsqueda.

> **Flujo de Navegación Intuitivo:**

- Se establece un flujo de navegación intuitivo y lógico que guía a los usuarios a través de las diferentes funcionalidades, desde la exploración del mapa hasta la denuncia de crímenes y el monitoreo de seguridad.

- Se utilizan llamadas a la acción claras y señales visuales para indicar el progreso y las acciones que los usuarios deben realizar en  cada paso del proceso de navegación.

## 6.3. Landing Page UI Design

### 6.3.1. Landing Page Wireframe

Utilizando la herramienta de diseño Figma, creamos la estructura base de la Landing Page.

Enlace al Landing Page Wireframe: <https://tinyurl.com/ybtsb4c6>

![](assets/LandingPageWireframe.png?raw=true)

### 6.3.2. Landing Page Mock-up

Teniendo el wireframe, realizamos una representación más realista de la Landing Page.

Enlace al Landing Page Mock-up: <https://tinyurl.com/ymunn2vn>

> ![](assets/LandingPageMock-up.png)


## 6.4. Applications UX/UI Design

### 6.4.1. Applications Wireframes

### 6.4.2. Applications Wireflow Diagrams

### 6.4.3. Applications Mock-ups

### 6.4.4. Applications User Flow Diagrams


## 6.5. Applications Prototyping