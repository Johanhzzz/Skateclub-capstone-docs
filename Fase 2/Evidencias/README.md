# Evidencias — Fase 2 (Informe de Avance)

Evidencias que respaldan el *Informe de Avance Fase 2* (`../Grupal/Informe_Avance_Fase2_SkateClub_v2.0.docx`). Fecha de corte: 12-10-2026.

## Capturas (`capturas/`)

| Archivo | Qué muestra | Historia |
|---|---|---|
| `01_app_login.jpg` | Pantalla de inicio de sesión en skateclub.cl | HDU-03 |
| `02_app_perfil_HDU-05.jpg` | Perfil skater: avatar, puntos y racha | HDU-05 |
| `03_app_perfil_editar_HDU-05.jpg` | Insignias, amigos, búsqueda y edición de perfil | HDU-05 |
| `04_app_mapa_sin_sesion.jpg` | Mapa público e invitación a iniciar sesión | — |
| `05_jira_backlog_sprint1.jpg` | Backlog de Jira durante el Sprint 1 (antes del cierre) | EP-01 |
| `06_jira_evidencia_HDU-03.png` | Comentario de verificación en SCRUM-13 | HDU-03 |
| `07_jira_sprint1_cerrado.png` | Sprint 1 cerrado: historias finalizadas y responsables | Sprint 1 |
| `08_jira_tablero_sprint2.png` | Tablero del Sprint 2 (13-10 a 18-10) | Sprint 2 |

## Pruebas (`pruebas/`)

`pytest_backend_2026-10-12.txt`: salida completa de la suite del backend, ejecutada con Docker (PostgreSQL 15 + PostGIS 3.3) mediante:

```bash
docker compose up -d
docker compose exec -w /app backend python3 -m pytest tests -v
```

Resultado: **35 casos — 34 aprobados, 1 omitido, 0 fallidos**.

## Jira

Cada historia del Sprint 1 (SCRUM-11 a SCRUM-15 y SCRUM-32 a SCRUM-34) tiene un comentario de verificación con criterios de aceptación, commits y pruebas: https://skateclub-dev.atlassian.net (espacio GrindOps).
