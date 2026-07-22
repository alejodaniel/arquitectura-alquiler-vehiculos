# Sistema de Alquiler de Vehículos - Arquitectura de Software

Este repositorio contiene la estructura arquitectónica diseñada tanto para el backend (Spring Boot) como para el frontend (Angular) del proyecto de alquiler de vehículos.

---

## 1. BACKEND: Arquitectura en Capas (Layered Architecture)

Para el diseño del backend en Spring Boot, se ha seleccionado la **Arquitectura en Capas (Layered Architecture)**.

### ¿Por qué se eligió esta arquitectura?

*   **Separación Clara de Responsabilidades:** La aplicación se divide en capas horizontales independientes (Presentación, Lógica de Negocio, Acceso a Datos), donde cada una tiene un rol técnico único.
*   **Facilidad de Mantenimiento y Aislamiento de Capas:** Si ocurre un cambio en la base de datos, solo se ve afectada la Capa de Acceso a Datos. La Capa de Presentación (los controladores REST) permanece intacta.
*   **Simplicidad y Baja Curva de Aprendizaje:** Al organizar los paquetes de esta manera, cualquier nuevo desarrollador sabe exactamente dónde modificar una consulta (Repository), una regla (Service) o un endpoint (Controller).

--------------------------------------------------------------------------------------------------------------------------------------------------

## 2. FRONTEND: Arquitectura Angular Modular basada en Características (Feature-Oriented)

Para el desarrollo del frontend de la aplicación web, se ha implementado el patrón de **Arquitectura Modular basada en Características / Dominios (Feature-Oriented / Domain-Driven Architecture)**.

Este patrón optimiza la organización del código dividiéndolo por lógica de negocio, promoviendo el aislamiento, el bajo acoplamiento y facilitando la carga perezosa (Lazy Loading) de las pantallas.

### ¿Por qué se eligió esta arquitectura para el Frontend? (Justificación Técnica)

*   **Modularidad por Dominio de Negocio:** Se agrupa directamente por los dominios funcionales del negocio (`auth`, `customer`, `vehicle`, `rent`, `reports`).

*   **Lazy Loading (Carga Perezosa de Pantallas):** Angular compila cada módulo funcional de forma independiente. Esto permite que el navegador del usuario descargue únicamente el código necesario para la pantalla que está visitando (por ejemplo, el catálogo de vehículos o el flujo de reserva).

*   **Bajo Acoplamiento:** Evita la contaminación de código entre perfiles de usuario. La lógica de cara al cliente (`reservation-flow`) está completamente separada de las herramientas del empleado (`rental-contract`, `return-inspection`) y de los reportes del administrador (`admin-dashboard`), de forma que los cambios en un rol no afectan a los demás.

*   **Gestión del Estado y Servicios Globals (`app-services/`):** Centraliza el flujo de datos del backend mediante una capa dedicada de servicios globales reutilizables. Esto evita que los componentes de la interfaz de usuario realicen peticiones directas y promueve una arquitectura limpia y testeable.

*   **Tipado Fuerte Compartido (`shared/vo/`):** El uso de interfaces de TypeScript (Value Objects) compartidas dentro de la capa común asegura que el frontend maneje exactamente los mismos modelos de datos y firmas que el backend (DTOs), previniendo errores de compatibilidad de datos al consumir la API.

### Distribución de Carpetas del Frontend (`vehicle-rental-frontend`)

El esqueleto completo de carpetas y archivos clave del frontend está estructurado de la siguiente forma bajo `src/app`:

```text
vehicle-rental-frontend/
└── src/
    ├── assets/                       (Recursos estáticos globales)
    │   ├── images/
    │   └── styles/
    │
    └── app/                          (Código principal Angular)
        ├── app-services/             (Servicios singleton de inyección global)
        │   ├── base/
        │   ├── common/
        │   ├── shared/
        │   └── main/                 (Servicios del negocio compartidos)
        │       ├── auth/
        │       │   └── auth.service.ts
        │       ├── customer/
        │       │   └── customer.service.ts
        │       ├── rent/
        │       │   └── rent.service.ts
        │       ├── reports/
        │       │   └── reports.service.ts
        │       └── vehicle/
        │           └── vehicle.service.ts
        │
        ├── shared/                   (Recursos compartidos y modelos VO)
        │   ├── modal/
        │   ├── pipe/
        │   │   └── date-format.pipe.ts (Filtro para formato de fechas en vistas)
        │   └── vo/                   (Interfaces de contratos del backend)
        │       ├── customer.vo.ts
        │       ├── vehicle.vo.ts
        │       ├── reservation.vo.ts
        │       ├── rental.vo.ts
        │       ├── payment.vo.ts
        │       ├── rate.vo.ts
        │       └── report.vo.ts
        │
        ├── modules/                  (Módulos de negocio asociados a casos de uso)
        │   ├── auth/                 (Módulo de Ingreso y Registro)
        │   │   ├── modals/
        │   │   └── components/
        │   │       ├── login/
        │   │       └── register/
        │   │
        │   ├── customer/             (Módulo de Clientes y Empleados)
        │   │   ├── modals/
        │   │   └── components/
        │   │       ├── customer-profile/
        │   │       └── employee-list/
        │   │
        │   ├── vehicle/              (Módulo de Catálogo e Inventario)
        │   │   ├── modals/
        │   │   └── components/
        │   │       ├── vehicle-catalog/
        │   │       ├── vehicle-details/
        │   │       └── vehicle-management/
        │   │
        │   ├── rent/                 (Módulo de Alquileres, Reservas y Devoluciones)
        │   │   ├── modals/
        │   │   └── components/
        │   │       ├── reservation-flow/
        │   │       ├── rental-contract/
        │   │       ├── return-inspection/
        │   │       └── payment-gateway/
        │   │
        │   └── reports/              (Módulo de Dashboard Administrativo y Tarifas)
        │       ├── modals/
        │       └── components/
        │           ├── admin-dashboard/
        │           └── rate-management/
        │
        └── util/                     (Scripts de soporte puro y utilidades lógicas)
```
