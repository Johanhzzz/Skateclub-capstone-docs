# SkateClub 🛹

Plataforma social y colaborativa para la comunidad skater: mapa colaborativo de spots georreferenciados, red social (feed y mensajería) y una capa de gamificación (duelos, puntos, insignias y recompensas).

Desarrollado por **itería SYSTEMS** (estudio *GrindOps*) como proyecto **CAPSTONE (PTY4614)** de Ingeniería en Informática, Duoc UC — sede Plaza Norte.

> **Estado:** en desarrollo · arquitectura modular por dominios (ADR 0001).

---

## 📋 Descripción del sistema

SkateClub resuelve la falta de una plataforma digital especializada para la comunidad skater, centralizando en un solo lugar:

- **Descubrimiento de spots:** mapa colaborativo con spots georreferenciados, búsqueda, calificaciones y comentarios.
- **Comunidad:** feed social, mensajería privada, amistades y perfiles skater.
- **Gamificación:** duelos S.K.A.T.E., puntos, insignias, ranking y un minijuego.
- **Recompensas:** canje de puntos por premios validados en locales asociados mediante código QR.
- **Administración:** panel para gestionar contenido, usuarios, eventos y recompensas.

Dirigido a skaters que buscan dónde patinar, conectar con otros y competir dentro de la comunidad.

---

## 👥 Integrantes y roles

| Integrante | Rol | Responsabilidades |
|---|---|---|
| **Johan Hernández Correa** | Tech Lead · Backend | Arquitectura, backend (FastAPI), base de datos (PostgreSQL/PostGIS), seguridad y despliegue. |
| **Álvaro Herrera Robles** | Product Owner · Frontend | Cliente Flutter, experiencia de usuario, diseño de producto y relación con la comunidad. |

---

## 🔄 Metodología de trabajo

Marco **Scrum** con gestión en **Jira**, complementado con prácticas **DevOps**.

- **Artefactos:** Product Backlog e historias de usuario priorizadas (MoSCoW), Sprint Backlog y *Definition of Done*.
- **Ceremonias:** Sprint Planning, Daily Scrum, Refinement, Sprint Review y Retrospective.
- **Trazabilidad:** cada cambio se asocia a una historia/tarea en Jira → rama → commit → Pull Request.

---

## 🧱 Stack tecnológico

| Capa | Tecnología |
|---|---|
| **Frontend** | Flutter (Android · Web · base para iOS) |
| **Backend** | FastAPI (Python), autenticación JWT (python-jose) + bcrypt |
| **Base de datos** | PostgreSQL + PostGIS |
| **Almacenamiento** | Supabase Storage |
| **Infraestructura** | Vercel (frontend web y API) · Supabase (PostgreSQL y Storage) · Docker (entorno local) |
| **Integraciones** | Google Gemini (IA) · Gmail SMTP (verificación) · Google Sign-In |
| **Gestión / QA** | Jira · GitHub · GitHub Actions · pytest · Locust · flutter test |

---

## 🏗️ Arquitectura de la solución

Arquitectura **cliente-servidor**. El cliente Flutter se comunica vía HTTP/JSON con una API REST en FastAPI, organizada por dominios bajo el patrón **Router → Service → Repository** (objetivo del ADR 0001). La persistencia usa PostgreSQL con PostGIS para geolocalización.

```
Cliente Flutter  ──HTTPS/JSON──►  API FastAPI (Router → Service → Repository)  ──SQL──►  PostgreSQL + PostGIS
                                          │
                                          ├─► Supabase Storage (archivos)
                                          ├─► Google Gemini (IA)
                                          └─► Gmail SMTP (verificación)
```

El detalle (diagramas UML, DER y contratos de API) está en `docs/ITS-SKC-DOC-06 — Diseño y Arquitectura Técnica`.

---

## 📁 Estructura del repositorio

```
skateclub/
├── backend/            # API FastAPI (routers, services, repositories)
│   ├── app/
│   ├── tests/
│   ├── Dockerfile
│   └── requirements.txt
├── frontend/           # Cliente Flutter (Android / Web)
│   └── lib/
├── docs/               # Documentación itería SYSTEMS (DOC-01 .. DOC-10)
├── docker-compose.yml
├── .env.example
└── README.md
```

---

## 🚀 Despliegue local (Docker)

> Estos pasos aplican al **repositorio de código del backend** (no a este repositorio de documentación). La guía completa está en ITS-SKC-DOC-09 — Manual Técnico y Guía de Despliegue v1.1.

**Prerrequisitos:** Docker Engine 24+, Docker Compose v2, Flutter 3.38.5, Git.

```bash
cp .env.example .env                    # variables locales (sin credenciales reales en el repo)
docker compose up --build -d            # PostgreSQL 15 + PostGIS 3.3 y la API; crea el esquema
curl http://localhost:8000/api/ping     # verificar la API (Swagger en /docs)
docker compose exec -w /app backend python3 -m pytest tests -v --ignore=tests/rendimiento
```

Cliente Flutter (repositorio del frontend): `flutter pub get && flutter run -d chrome`.

---

## 🌿 Estrategia de ramas y commits

**Ramas (GitFlow simplificado):**

- `main` — código estable en producción.
- `develop` — integración de funcionalidades.
- `feature/US-XX-descripcion` — una rama por historia de usuario.
- `fix/…` — corrección de bugs.

**Commits convencionales** ([Conventional Commits](https://www.conventionalcommits.org/)):

```
feat: agrega canje de recompensas con validación por QR
fix: corrige 401 en endpoints sensibles sin token
docs: actualiza el diagrama de arquitectura
test: agrega pruebas negativas de autorización admin
```

**Reglas de Pull Request:** todo PR ejecuta validaciones automáticas (import/lint/test en backend, analyze/test en frontend); un PR con fallos no se fusiona.

---

## 📚 Documentación del proyecto

La documentación formal (itería SYSTEMS) vive en `docs/`:

| Código | Documento |
|---|---|
| ITS-SKC-DOC-01 | Documento de Visión del Producto |
| ITS-SKC-DOC-02 | Definición del Proyecto APT |
| ITS-SKC-DOC-03 | Especificación de Requerimientos de Software (ERS) |
| ITS-SKC-DOC-04 | Product Backlog e Historias de Usuario |
| ITS-SKC-DOC-05 | Plan de Sprints |
| ITS-SKC-DOC-06 | Diseño y Arquitectura Técnica (UML, DER, API) |
| ITS-SKC-DOC-07 | Plan de Pruebas y QA |
| ITS-SKC-DOC-08 | Requisitos No Funcionales (RNF) |
| ITS-SKC-DOC-09 | Manual Técnico y Guía de Despliegue (Docker) |
| ITS-SKC-DOC-10 | UX/UI Kit & Brand Guidelines |
| — | Carta Gantt (Excel) |

---

## 📄 Licencia

Proyecto académico desarrollado con fines educativos en el marco de CAPSTONE — Duoc UC.
© 2026 itería SYSTEMS · GrindOps.
