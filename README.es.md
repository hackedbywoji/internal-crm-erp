[English](README.md) · **Español**

# CRM y ERP interno para una empresa de tres fundadores

Caso de estudio de [Kortem](https://kortem.es). Hecho con mis dos cofundadores. El repositorio es privado, así que esto cubre qué hace, las decisiones detrás y lo que construí yo.

## El problema

La empresa funcionaba con hojas de cálculo y formularios: un libro de caja de los socios en Excel más un formulario para apuntar movimientos, archivos de Excel separados para los presupuestos y nada que uniera clientes, reuniones y seguimientos. El CRM y ERP lo sustituye por una sola aplicación privada para los tres socios.

## Qué hace

| Área | Qué cubre |
|---|---|
| Panel | Caja disponible, ventas del mes, próximo reparto, presupuestos abiertos, reuniones, clientes que necesitan seguimiento |
| Libro de caja | El libro de caja completo de los socios: resumen, reparto por socio, alta de movimientos |
| Clientes | Embudo de lead a cliente con recordatorios de seguimiento; creación e importación CSV/Excel |
| Calendario | Reuniones, llamadas y tareas, vista semanal, feed `.ics` para Google Calendar e iPhone |
| Presupuestos | Editor por partes (horas x tarifa o precio cerrado), descuentos, conceptos mensuales, cláusulas, estados, propuesta en PDF, «enviar al cliente» |
| Correo | Buzón de la empresa por IMAP, segmentos de leads, responder desde la app, notificaciones push |
| Dominios | Caducidad, registrador y a dónde apunta cada dominio |

## Stack

TypeScript · Next.js · React · Postgres y Auth gestionados · Tailwind · Vitest · Playwright (con comprobaciones de accesibilidad) · Web Push

## Decisiones

| Decisión | Por qué |
|---|---|
| El registro está cerrado; los tres socios se crean a mano | Es una herramienta interna. Nadie puede registrarse por su cuenta. |
| Primer acceso por enlace de correo, luego cada persona fija su propia contraseña | Primer acceso fácil, acceso normal después, y el enlace sigue disponible. |
| Los mensajes de inicio de sesión nunca revelan si un correo pertenece a un socio | Sin enumeración de cuentas. |
| Las reglas de contraseña las impone la capa de autenticación, no solo la interfaz | La regla no se puede saltar desde el cliente. |
| El enlace del correo abre una página de confirmación con un botón «Entrar», y el token se canjea con un POST | Los clientes de correo que abren los enlaces para escanearlos ya no gastan el enlace antes que la persona. |
| El enlace funciona en cualquier dispositivo | La primera versión solo funcionaba en el navegador que lo pidió, y fallaba si el enlace se pedía en un ordenador y se abría en el móvil. |
| Un modo demo con datos inventados | Se pueden diseñar pantallas sin base de datos. |
| Los datos reales nunca entran en el repo; `.gitignore` bloquea las hojas de cálculo; los datos de ejemplo usan solo direcciones reservadas | Los datos del negocio se quedan fuera de Git. |
| Los scripts de importación son idempotentes | Volver a ejecutar una importación nunca duplica filas. |
| Puerta de CI en cada PR: tipos, lint, pruebas, build; comprobación del nombre del PR | Cada persona fusiona su propio PR cuando el CI está en verde, sin guardián. |

## Lo que construí

De mis pull requests fusionados en este repo:

- **Inicio de sesión y acceso:** el paso del primer acceso por enlace a contraseñas propias, y el arreglo que hizo que los enlaces funcionaran en cualquier dispositivo y sobrevivieran a los escáneres de correo.
- **Despliegue:** el Dockerfile para el VPS de la empresa.
- **Datos:** importación idempotente del libro de caja; creación de clientes e importación CSV/Excel; gestión manual de dominios.
- **Notificaciones y correo:** notificaciones push para el correo nuevo y para lo que hace cada socio; el flujo «enviar presupuesto al cliente» que abre el redactor con la propuesta lista.
- **Interfaz:** fotos de perfil, sonidos de interfaz y animaciones.
- **Proceso del equipo:** el PR que fusiona los PR abiertos de los socios y la regla de contribución sobre fusionar con CI en verde.

La prospección comercial y buena parte del trabajo de correo los construyeron sobre todo mis cofundadores.
