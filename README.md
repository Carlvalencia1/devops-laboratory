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

## 🏛️ Arquitectura de Software

El backend está construido siguiendo los principios de **Domain-Driven Design (DDD)**, **Arquitectura Hexagonal (Ports & Adapters)** y **Clean Architecture**, empaquetados en un **Monolito Modular**.

### ¿Por qué un Monolito Modular?
En lugar de microservicios prematuros, usamos un monolito dividido en módulos estrictos. Esto nos da:
1. **Alta cohesión:** Cada Bounded Context (Salones, Reservas, Contratos) vive en su propia carpeta.
2. **Bajo acoplamiento:** Las dependencias apuntan hacia el dominio. La infraestructura depende de la aplicación, no al revés.
3. **Facilidad de extracción:** Si un módulo crece demasiado, puede convertirse en un microservicio sin reescribir su lógica.

### Estructura de Capas (Regla de Dependencia)
*   **Domain (Dominio):** Entidades, Objetos de Valor, Reglas de Negocio e Interfaces (Puertos). No tiene dependencias externas (ni frameworks, ni bases de datos).
*   **Application (Aplicación):** Casos de uso. Orquesta el flujo entre el dominio y los adaptadores.
*   **Infrastructure (Infraestructura):** Adaptadores de entrada (HTTP/REST) y salida (PostgreSQL, Redis). Implementan las interfaces del dominio.

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
│   └── venues-api/        # Servicio Backend en Go (Monolito Modular)
│       ├── cmd/           # Puntos de entrada
│       ├── internal/      # Código privado
│       │   ├── domain/    # Entidades, Value Objects y Puertos
│       │   ├── application/ # Casos de Uso
│       │   └── infrastructure/ # Adaptadores (HTTP, DB)
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