# Event Venues Platform & DevOps Laboratory

Plataforma integral de descubrimiento y gestión de salones de eventos, que conecta clientes finales, administradores de salones y proveedores (socios) en un solo ecosistema. 

Este repositorio funciona, además, como **Laboratorio personal de CI/CD** para demostrar la implementación de un ciclo de vida de software robusto sobre una arquitectura políglota.

---

## 🎯 El Problema y la Solución
El mercado actual de eventos está fragmentado:
*   **Los clientes** pierden tiempo buscando salones y coordinando por separado proveedores (DJs, decoradores) que no conocen.
*   **Los administradores de salones** gestionan disponibilidad, contratos y cláusulas en hojas de cálculo o WhatsApp, lo que genera sobreventas y pérdida de trazabilidad legal.

**Nuestra solución:** Un ecosistema que une el *Marketplace* (descubrimiento para el cliente) con el *Back-office* (gestión de calendarios, contratos y red de socios para el administrador), todo construido bajo estándares rigurosos de ingeniería y DevOps.

---

## 🛠️ Stack Tecnológico

Este proyecto abarca un ecosistema políglota para demostrar versatilidad y mejores prácticas por dominio:

- **Backend:** Go (servicios cloud-native de alto rendimiento)
- **Frontend Web:** React (panel de administración y gestión)
- **Móvil:** Kotlin + Kotlin Multiplatform (KMP) (aplicación para el cliente final)
- **Automatización / Scripts:** Python
- **Legacy / Enterprise:** Java (soporte y lectura de sistemas heredados)

---

## 🏛️ Arquitectura y Diseño de Dominio (DDD)

El sistema está diseñado bajo los principios de **Domain-Driven Design (DDD)** y **Arquitectura Hexagonal**, separando estrictamente la lógica de negocio de la infraestructura.

### Contextos Delimitados (Bounded Contexts)
1.  **Marketplace (Descubrimiento):** API pública para búsqueda de salones y paquetes.
2.  **Gestión Operativa (Back-office):** Panel para administradores (calendarios, contratos).
3.  **Red de Socios:** Portal de postulación y gestión para proveedores.
4.  **Identidad:** Autenticación y control de accesos basado en roles (RBAC).

### Agregados Principales del Dominio
*   **Venue (Salón):** Entidad raíz con ubicación, capacidad y paquetes (`Package`).
*   **Booking (Reserva):** Manejo de disponibilidad mediante `TimeSlots` (mañana, tarde, noche) y bloqueos distribuidos para evitar el *Double Booking*.
*   **Contract (Contrato):** Generación dinámica con `Clauses` (cláusulas) reutilizables y firmas inmutables.
*   **Partner (Socio):** Proveedores externos validados por el administrador del salón.

### Decisiones Arquitectónicas Clave (ADRs)
Todas las decisiones técnicas importantes se documentan en `docs/adr/`. Algunas de las más críticas son:
*   **Go + PostgreSQL + Redis:** Para garantizar transacciones ACID y bloqueos distribuidos (evitar doble reserva).
*   **Arquitectura Hexagonal:** Para mantener la lógica de negocio pura y altamente testeable.
*   **Auditoría Inmutable (Outbox Pattern):** Para trazabilidad legal de contratos y cláusulas.

---

## 🚀 El Laboratorio de CI/CD

El objetivo de este repositorio no es solo construir la aplicación, sino **automatizar el proceso de entrega con calidad**. 

### Objetivos del laboratorio
1.  Pipeline de CI para Go (lint, test, vet, build, docker).
2.  Pipeline de CI para React (lint, test, build).
3.  Pipeline de CI para Kotlin/KMP (gradle build, test).
4.  Pipeline de CD con Docker + GitHub Actions.
5.  Documentar decisiones de arquitectura (ADRs) y métricas DORA.

### Tecnologías de CI/CD
- **GitHub Actions** (Orquestador principal)
- **Docker / Docker Compose** (Empaquetado reproducible)
- *(Futuro)* Kubernetes, Terraform, Prometheus/Grafana.

### Flujo de Ramas y Ambientes
Seguimos un modelo de promoción estricto:
*   `feature/*` -> PR a `develop` (CI: Lint, Test, Build)
*   `develop` -> PR a `QA` (CI + Pruebas de Integración)
*   `QA` -> PR a `main` (Aprobación manual + Despliegue a PRODUCTION)

---

## 📁 Estructura del Repositorio

```text
devops-laboratory/
├── .github/
│   └── workflows/         # Pipelines de CI/CD (ci-go.yml, ci-react.yml, etc.)
├── services/
│   └── venues-api/        # Servicio Backend en Go (Core Domain)
├── apps/
│   ├── web-admin/         # Frontend React (Back-office)
│   └── mobile-kmp/        # App Kotlin Multiplatform (Cliente final)
├── automation/
│   └── python/            # Scripts de automatización y pruebas E2E
├── docs/
│   ├── adr/               # Architecture Decision Records
│   ├── domain-model.md    # Modelado de Entidades y Bounded Contexts
│   └── requirements.md    # Matriz de Requerimientos y Trazabilidad
├── infra/
│   └── docker/            # Dockerfiles y docker-compose
├── .gitignore
└── README.md