# Clínica Sonrisa Clara

Demo completa de reservas odontológicas. Node 24+ (SQLite nativo) y Express, frontend estático sin compilación.

## Ejecutar

```sh
npm install
npm start
```

Abrir http://localhost:3848. `PORT` cambia el puerto; `DB_PATH` cambia el archivo SQLite (por defecto `server/clinic.sqlite`). Los cuatro servicios se crean automáticamente. La agenda empieza vacía.

## Funciones

- Reserva pública: nombre, teléfono, servicio, fecha y hora; estado pendiente.
- Agenda: filtros por fecha/estado, confirmar, cancelar con confirmación, actualizar.
- Bloqueo y desbloqueo de horarios. No permite reservar ni bloquear un horario ocupado.
- Horarios de demostración: lunes a viernes 09–13 y 14–18, cada 30 minutos. No representan horarios comerciales reales.
- SQLite conserva reservas y bloqueos entre reinicios. Índices únicos impiden duplicar reservas activas.

## Frontend en Pages

Publicar el contenido de `public/` como raíz del sitio (por ejemplo, artefacto de GitHub Pages). Los recursos y las llamadas API usan rutas relativas y funcionan en subdirectorios. Sin API, se activa automáticamente el modo demo con localStorage; la interfaz muestra dónde guarda los datos. Para probar localmente sin backend: `python3 -m http.server 3849 --directory public`.

El modo se elige al iniciar. Si una API conectada falla durante una operación, se muestra el error para evitar simular que una reserva fue guardada en el servidor. Recargar sin API activa el modo local. Los dos almacenes son independientes y no se sincronizan. LocalStorage requiere almacenamiento permitido por el navegador.

## API

| Método | Ruta | Uso |
|---|---|---|
| GET | /api/services | Servicios |
| GET / POST | /api/appointments | Listar / reservar (`name, phone, serviceId, date, time`) |
| PATCH | /api/appointments/:id | Estado `confirmed` o `cancelled` |
| GET / POST | /api/blocks | Listar / bloquear (`date, time, reason`) |
| DELETE | /api/blocks/:id | Desbloquear |

Fechas ISO `YYYY-MM-DD`; horas `HH:mm`. Errores 400 por datos inválidos y 409 por conflictos. Referencia del día actual: UTC.

## Verificación

`npm test`: integración con SQLite en memoria: servicios, validación, doble reserva, confirmación, cancelación, bloqueo y desbloqueo. Con el servidor iniciado, `node qa/browser-test.cjs` prueba el modo local y genera capturas a 1280 y 390 en `qa/`. Se verificó también en Chromium el flujo local completo y persistencia, sin errores JS ni desborde horizontal.

## Alcance de demo

No realiza reservas reales ni envía mensajes. Administración y endpoints abiertos deliberadamente para la demo: **no usar con datos reales**. Antes de producción se necesitan autenticación y autorización administrativas, protección de datos, reglas comerciales y zona horaria del consultorio. `noindex,nofollow` en HTML y cabecera; no es control de acceso. No se publicó ni se hizo git push.

Fotos ilustrativas de Unsplash, descargadas como PNG. No identifican instalaciones, pacientes ni profesionales reales del consultorio:
- `hero-sillon.png`: https://images.unsplash.com/photo-1629909613654-28e377c37b09 (ID del recurso: photo-1629909613654-28e377c37b09).
- `recepción.png`: recurso photo-1629909615184-74f495363b67.
- `sonrisa.png`: recurso photo-1438761681033-6461ffad8d80.
- `equipo.png`: recurso photo-1622253692010-333f2da6031d.

Fuentes externas: Google Fonts, con fallback sans-serif. Todo el comportamiento de la app funciona sin fuentes externas.
