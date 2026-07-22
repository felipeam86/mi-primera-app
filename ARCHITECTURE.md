# ARCHITECTURE.md

**Este documento es el estándar de oro del repo**: describe qué existe, cómo
fluye y qué decisiones se tomaron. Todo cambio que altere algo descrito aquí
DEBE actualizar este archivo en el mismo cambio. Código y documento en
desacuerdo = cambio incompleto.

## Estado actual

Solo documentos. El código de la app no existe todavía: nace en la primera
conversación entre el dueño y Claude, guiada por la entrevista del README.

## Decisiones tomadas de antemano

Valen hasta que una necesidad real demuestre lo contrario:

- **Cloudflare como casa.** Un Worker, plan gratis, desplegado con wrangler.
- **Un archivo de lógica.** Sin framework, sin router, sin bundler. Se divide
  solo cuando duela de verdad.
- **Escala personal.** Un puñado de usuarios conocidos. Nada de cuentas de
  usuario, pagos ni multi-tenancy; si algún día se necesitan, ese es otro
  proyecto.

## El menú de Cloudflare — solo cuando se necesite

Piezas simples y probadas a escala personal. Ninguna se agrega por
adelantado: cada una entra cuando la app la pida, y al entrar se documenta
aquí. Es el primer lugar donde mirar, no una jaula: si algo de afuera es
claramente mejor para el caso, Claude lo propone y se decide en conversación.

- **Guardar datos → D1.** Base de datos SQL simple. Para listas, registros,
  historial — casi todo lo que una app personal guarda.
- **Hacer algo solo, a horas fijas → Cron Triggers.** Resúmenes cada mañana,
  recordatorios cada noche, revisiones periódicas.
- **Mandar correos → Email Routing.** Avisos e invitaciones de calendario
  desde tu propio dominio, sin contratar un servicio de correo.
- **Una pantalla privada → Cloudflare Access.** Pone un código por email
  delante de tus páginas. Es la alternativa a construir login y contraseñas:
  no se construye login, se usa Access.
- **Dirección propia → dominio en Cloudflare** (~$10 USD/año). Necesario
  para Access y para el correo; además tu app deja de tener una URL prestada.
- **Guardar archivos → R2.** Fotos, PDFs, documentos. Solo si la app maneja
  archivos de verdad; los datos normales van en D1.

## Lo que este documento describirá cuando la app exista

Claude crea y mantiene estas secciones a medida que se vuelvan reales: mapa
del repo, rutas, modelo de datos, tareas programadas (crons), secretos (por
nombre, nunca su valor), integraciones, y toda decisión nueva con su porqué.
