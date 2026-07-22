# Sistema de Alquiler de Vehículos - Arquitectura de Software

Este repositorio contiene la estructura arquitectónica diseñada tanto para el backend (Spring Boot) como para el frontend (Angular) del proyecto de alquiler de vehículos.

---

## 1. BACKEND: Arquitectura en Capas (Layered Architecture)

Para el diseño del backend en Spring Boot, se ha seleccionado la clásica **Arquitectura en Capas (Layered Architecture)**. Este es uno de los patrones arquitectónicos más utilizados y consolidados en el desarrollo de software empresarial.

### ¿Por qué se eligió esta arquitectura?
*   **Separación Clara de Responsabilidades (Separation of Concerns):** La aplicación se divide en capas horizontales independientes (Presentación, Lógica de Negocio, Acceso a Datos, Modelo), donde cada una tiene un rol técnico único.
*   **Facilidad de Mantenimiento y Aislamiento de Capas:** Si ocurre un cambio en la base de datos, solo se ve afectada la Capa de Acceso a Datos. La Capa de Presentación (los controladores REST) permanece intacta.
*   **Simplicidad y Baja Curva de Aprendizaje:** Al organizar los paquetes de esta manera, cualquier nuevo desarrollador sabe exactamente dónde modificar una consulta (Repository), una regla (Service) o un endpoint (Controller).

### Distribución de Capas en Carpetas (`vehicle-rental-backend`)
1.  **Capa de Transporte (`veh-rent-vo`):** Contiene los Value Objects (VO) y DTOs agrupados por negocio (`customer`, `rent`, `vehicle`).
2.  **Contratos y Modelos (`veh-rent-client`):** Contiene las interfaces de los servicios, repositorios y las entidades físicas de la base de datos (`entity`).
3.  **Implementación de Negocio (`veh-rent-core`):** Aloja las clases de servicio (`services`) que contienen la lógica real de negocio y las implementaciones de los repositorios.
4.  **Capa de Presentación (`veh-rent-services`):** Contiene los controladores REST (`controller`) que exponen los endpoints del API.

---

## 2. FRONTEND: Arquitectura Angular Modular basada en Características (Feature-Oriented)

Para el desarrollo del frontend de la aplicación web responsive, se ha implementado el patrón de **Arquitectura Modular basada en Características / Dominios (Feature-Oriented / Domain-Driven Architecture)**.

Este patrón optimiza la organización del código dividiéndolo por lógica de negocio, promoviendo el aislamiento, el bajo acoplamiento y facilitando la carga perezosa (Lazy Loading) de las pantallas.

### Componentes de la Arquitectura del Frontend

La arquitectura organiza el código del cliente en carpetas según su nivel de reutilización y pertenencia al negocio:

1.  **`app-services/` (Servicios Globales de la Aplicación):**
    Aloja los servicios compartidos de inyección global (Singletons) que gestionan el estado, las integraciones y las utilidades comunes. Se sub-divide en:
    *   `base/`: Lógica base para peticiones HTTP e interceptores.
    *   `common/`: Servicios utilitarios como manejo de almacenamiento local (`localStorage`), cifrado o alertas globales.
    *   `main/`: Servicios de negocio accesibles por múltiples módulos de forma global (ej. autenticación, catálogos paramétricos).
    *   `shared/`: Servicios cruzados del sistema.
2.  **`modules/` (Módulos de Negocio / Lazy Loading):**
    Representa la división del sistema por dominios o casos de uso. Cada carpeta es un módulo autocontenido que se carga bajo demanda (Lazy Loading). Dentro de cada módulo se estructuran las siguientes carpetas:
    *   `components/`: Las vistas y pantallas del negocio (HTML, TS, SCSS).
    *   `modals/`: Las ventanas emergentes/diálogos interactivos propios de ese módulo específico.
3.  **`shared/` (Elementos Compartidos de UI y Modelos):**
    Contiene componentes visuales globales y modelos de datos compartidos:
    *   `modal/`: Diálogos y modales genéricos y reutilizables por toda la app.
    *   `pipe/`: Filtros personalizados para formatear datos (moneda, fechas, etc.).
    *   `vo/`: Value Objects (Interfaces de TypeScript) que mapean exactamente la estructura de datos que envía el backend (DTOs).
4.  **`util/` (Funciones Utilitarias):**
    Funciones lógicas puras ajenas a Angular (ej. formateadores de texto, helpers matemáticos, etc.).
