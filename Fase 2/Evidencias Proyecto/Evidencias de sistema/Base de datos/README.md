# Evidencias de sistema — Base de datos

Base de datos de SkateClub: **PostgreSQL 15 + PostGIS 3.3**, alojada en **Supabase** y accedida por el backend mediante el pooler de transacciones (`DATABASE_URL`). En local se levanta con `docker compose up -d` desde el repositorio del backend.

## Contenido

| Archivo | Qué muestra |
|---|---|
| `Modelo_Entidad_Relacion_SkateClub.png` | Modelo entidad-relación (PK/FK y cardinalidades), tomado del documento de Diseño y Arquitectura (ITS-SKC-DOC-06). |

## Decisiones del modelo

- Coordenadas de spots (`spots.coordenadas`) y ubicación de usuarios (`usuarios.ubicacion_actual`) como `geometry(point, 4326)`.
- Consultas por proximidad (radar y mapa) con `ST_DWithin` sobre `geography`, apoyadas en índices espaciales **GiST** (Sprint 2): `idx_spots_coordenadas_gist` e `idx_usuarios_ubicacion_gist`.
- Contraseñas con hash **bcrypt** (las cuentas creadas antes de la migración pasan a bcrypt al iniciar sesión); tokens de refresh y de recuperación con expiración y revocación.
- **Row Level Security** activado en las tablas públicas para bloquear el acceso directo por la API REST de Supabase.
- El esquema local se crea con `scripts/init_local_db.py` (también lo usa el pipeline de CI).

## Diccionario de datos (resumen)

| Tabla | Propósito | Claves foráneas |
|---|---|---|
| calificaciones | Calificación (1–5) de un usuario a un spot. | usuario_id, spot_id |
| publicaciones | Publicaciones del feed social. | usuario_id |
| comentarios | Comentarios sobre publicaciones. | publicacion_id, usuario_id |
| mensajes | Mensajería privada entre usuarios. | emisor_id, receptor_id |
| amistades | Solicitudes y relaciones de amistad. | solicitante_id, receptor_id |
| duelos | Duelos S.K.A.T.E. entre usuarios. | desafiante_id, oponente_id |
| eventos | Eventos de la comunidad. | — |
| asistencias | Confirmación de asistencia a eventos. | evento_id, usuario_id |
| insignias / usuario_insignia | Catálogo de insignias y su asignación. | usuario_id, insignia_id |
| recompensas | Catálogo de recompensas canjeables. | — |
| canjes | Canjes de puntos por recompensas y su validación. | usuario_id, recompensa_id, local_id |
| locales_asociados | Locales que validan la entrega de recompensas. | — |

El diccionario completo está en ITS-SKC-DOC-06, sección 5.2.
