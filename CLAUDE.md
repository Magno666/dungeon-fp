# CLAUDE.md — Proyecto: dungeon crawler en primera persona (fork de Shattered Pixel Dungeon)

## Contexto

Fork de Shattered Pixel Dungeon (Java + libGDX, GPLv3) convertido a
**dungeon crawler en primera persona por turnos y en cuadrícula**, tipo
Legend of Grimrock / Eye of the Beholder, con destino principal
**Android / Google Play**.

Las mecánicas de Shattered se conservan íntegras: turnos, cuadrícula,
identificación de pociones, maldiciones, jefes (Goo, Tengu, DM-300, Rey
Enano, Yog-Dzewa). Lo único que cambia es la capa de presentación:
de vista superior 2D a primera persona 3D.

## Restricción de licencia — no negociable

El código base es GPLv3. Todo el proyecto queda GPLv3 y su código será
público. No incorporar dependencias con licencias incompatibles
(nada propietario, nada CC-BY-NC en assets de código). Los assets de
arte que yo cree son míos y van con licencia aparte, indicada en el repo.

Antes de distribuir: cambiar `appName`, ícono y pantalla de título para
no confundirse con Shattered. Mantener los créditos a Watabou y a
Evan Debenham.

## Regla de arquitectura principal

**La capa de lógica no se toca.** Los paquetes de `actors`, `items`,
`levels`, `mechanics` y `scenes` de lógica son la fuente de verdad y se
usan tal cual. El trabajo está en reemplazar la capa de render.

Si una tarea parece requerir modificar la lógica del juego, **para y
pregúntame**. Casi siempre significa que entendí mal el problema, no
que la lógica esté mal.

## Fases

### Fase 0 — Spike de sensación (Godot 4, desechable)
Proyecto aparte, código de tirar. No entra al repo del fork.
Objetivo: contestar una sola pregunta — ¿se siente bien moverse por
casillas en primera persona en un teléfono?

Contenido mínimo:
- Una sala. Cuatro paredes de color plano. Sin texturas, sin arte.
- Movimiento por casillas con interpolación (ver "Feel" abajo).
- Cámara libre 360° independiente del movimiento.
- Un enemigo que se teletransporta por la sala (prueba de Tengu).
- Export a APK para probar en teléfono real.

No avanzar a Fase 1 hasta que yo diga que se siente bien.
Voy a iterar los números yo, a mano, con el teléfono. Tu trabajo es
dejarme esos números expuestos y fáciles de cambiar.

### Fase 1 — Compilar el fork tal cual
Clonar Shattered, compilarlo sin modificar, correrlo en Android.
Entender la estructura antes de tocar nada.

### Fase 2 — Capa de render 3D en libGDX
libGDX ya trae API 3D, por eso el fork se queda en Java en vez de
portarse a otro motor: así reuso el 100% de la lógica sin traducir
una sola línea.
Reemplazar la escena de mazmorra por render en primera persona,
leyendo el mismo mapa de tiles que ya usa el juego.

### Fase 3 — Arte
Los sprites originales son vista superior y **no sirven**. Todo el arte
se rehace: paredes, pisos, enemigos vistos de frente. Esta fase es mía,
no tuya.

## Feel del movimiento — parámetros a exponer

Estos van en un solo archivo de configuración, con nombres claros, para
que yo los ajuste sin buscar por todo el código:

- `stepDurationMs` — interpolación al avanzar de casilla. Arranca en 180.
- `stepEasing` — curva del desplazamiento. Arranca en ease-out.
- `turnDurationMs` — giro de 90°. Arranca en 150.
- `headBobAmplitude` — arranca muy bajo. Poco marea menos.
- `lookSensitivity` — sensibilidad del dedo derecho.

Reglas duras del control:
- Mirar **nunca** consume turno ni tiene límite de ángulo.
- Moverse y girar sí consumen turno.
- Stick virtual izquierdo = movimiento por casillas.
- Área libre derecha = mirar, estilo COD Mobile / PUBG.

## Cómo quiero que trabajes

- Una tarea a la vez. Nada de refactors grandes no pedidos.
- Antes de escribir código en una tarea nueva, dime el plan en tres
  o cuatro líneas y espera mi visto bueno.
- Nada de dependencias nuevas sin preguntar.
- Si algo no se puede o va a salir mal, dímelo antes de intentarlo.
- Español para explicaciones, inglés para el código y los commits.

## Play Store — pendientes administrativos

No son código, pero bloquean el lanzamiento:
- Cuenta de Google Play Developer con verificación de identidad
- Política de privacidad publicada en una URL
- Cuestionario de clasificación de contenido
- Firma de la app (upload key + Play App Signing)
