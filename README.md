# El Gran Circo del Poder

Simulación multijugador de liderazgo, completamente ficticia y satírica. Los romances y las conductas narradas son inventados; no son afirmaciones sobre personas reales.

## Requisitos e instalación

Node.js 20 o superior y npm. Descomprimir el ZIP, abrir una terminal en la carpeta del proyecto y ejecutar:

```bash
npm install
npm run dev
```

Abrir `http://localhost:3000`. En producción: `npm start`. Ejecutar pruebas: `npm test`. Copiar `.env.example` a `.env` si se desean variables locales (Node no carga automáticamente `.env` en esta configuración; definirlas en el entorno del proceso o usar `node --env-file=.env server.js`).

## Uso

El anfitrión crea una sala y comparte el código o la URL de invitación. Cada participante abre la URL en su navegador y se une con nombre o alias. Se requieren entre 2 y 6 participantes (por defecto 3); se asignan roles por orden de entrada. El anfitrión inicia la partida, puede pausarla, cierra cada votación y avanza entre seis actos. Las respuestas se registran en el servidor y no se pueden repetir. Los empates favorecen la opción que aparece primero: regla simple pero arbitraria que el facilitador debe explicar. Tras el último acto se muestran las métricas y los informes. El anfitrión rellena la rúbrica y exporta un CSV. El informe de jugador no contiene rúbricas privadas.

## Arquitectura

Servidor Express + Socket.IO y frontend HTML/CSS/JS sin compilación. Estado de salas exclusivamente en memoria del proceso. Las credenciales de anfitrión y jugador son tokens aleatorios mantenidos en `sessionStorage` del navegador. La sala se recupera tras una reconexión en la misma pestaña. Las operaciones privilegiadas se autorizan en el servidor. Las métricas no se transmiten durante las rondas. La app no incluye chat: la negociación se realiza presencialmente o mediante un canal externo elegido por el facilitador.

## Despliegue HTTPS

1. Crear un repositorio privado o público sin archivos `.env` ni secretos y subir el proyecto.
2. En Render, Railway o un proveedor equivalente, crear un **servicio web Node.js persistente** a partir del repositorio. No desplegarlo como sitio estático.
3. Comando de instalación: `npm install`; comando de arranque: `npm start`; variable `NODE_ENV=production`; usar el puerto `PORT` que proporcione el proveedor. Verificar `/health`.
4. Usar el dominio HTTPS asignado por el proveedor o configurar un dominio propio con DNS según las instrucciones del proveedor. El mismo origen HTTPS sirve frontend y Socket.IO mediante WSS; el proveedor debe soportar WebSockets.
5. Mantener **una sola instancia** del servidor. El estado en memoria no se comparte entre réplicas; para escalar, añadir Redis, adaptador Socket.IO y almacenamiento persistente antes de utilizar varias instancias.
6. Probar desde varios teléfonos con conexiones diferentes antes de un taller. Algunos planes gratuitos duermen instancias o eliminan procesos; elegir un servicio sin suspensión para talleres.

**Costes:** Node.js y las dependencias son gratuitos; hosting, dominio y servicios opcionales como Redis pueden tener costes variables según proveedor y plan. No se ha contratado ni configurado ningún servicio externo ni dominio.

## Privacidad y limitaciones

Datos almacenados: código de sala, alias, rol, estado de conexión, decisiones, rúbricas y comentarios del facilitador. Todo permanece en la memoria del servidor hasta que el anfitrión pulsa eliminar, el proceso reinicia o transcurren 8 horas desde la creación (configurable con `ROOM_TTL_HOURS`). No se usa una base de datos, analítica ni cookies de seguimiento. Los CSV exportados quedan bajo responsabilidad del anfitrión y deben eliminarse según la política del taller. Los participantes no reciben comentarios privados; el anfitrión debe mantener en secreto su token. Los datos se pierden si el servidor se reinicia. No usar nombres reales si no son necesarios.

Las observaciones son una **rúbrica formativa no validada psicométricamente**. Una dimensión no evaluable no cuenta en la media. La nota 1 no implica falta de capacidad; registra únicamente una conducta no observada o insuficiente en esta actividad. No se deducen rasgos psicológicos ni competencias generales de las opciones de juego.

## Advertencias para producción

Versión funcional de base, no infraestructura empresarial: sin autenticación de cuentas, persistencia, límites avanzados de solicitudes ni recuperación del anfitrión si pierde su token. El servidor no aplica una cuenta atrás automática; el anfitrión gestiona los tiempos de negociación. Los roles se asignan por orden y el máximo es seis. Si se requiere uso simultáneo masivo, auditoría formal, GDPR organizativo, respaldo de datos o despliegue de múltiples instancias, realizar endurecimiento y revisión de seguridad adicionales.

## Verificación

Consultar `PRUEBAS.md` para los resultados verificables en el entorno de generación. Las pruebas con teléfonos físicos y un dominio HTTPS real quedan pendientes.
