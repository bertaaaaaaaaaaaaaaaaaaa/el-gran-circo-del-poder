# Registro de pruebas

Las comprobaciones reales se registran aquí. No se declara despliegue público ni pruebas en dispositivos físicos.

## Comprobaciones realizadas
- `node --check` de `server.js`, `game.js` y `public/app.js`: correcto, sin errores de sintaxis.
- `node --test`: pruebas unitarias de estructura de rondas, motor de consecuencias y aislamiento de notas privadas (ver resultado en la salida de ejecución).
- Instalación de dependencias `npm install`: **no completada**, expiró el tiempo de ejecución por restricciones del entorno.
- Pruebas de integración de Socket.IO (crear sala, múltiples navegadores, votos, cierre, CSV y códigos erróneos): **no ejecutadas** porque las dependencias no pudieron instalarse aquí.
- Pruebas en teléfonos o tabletas físicos: **no realizadas**.
- Publicación HTTPS: **no realizada**.
