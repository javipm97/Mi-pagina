# Mi página

Página personal con los resúmenes diarios: correo, ofertas de trabajo, noticias y cambio euro-dólar.

- **Web:** un único `index.html` (sin compilar nada), publicado con GitHub Pages.
- **Datos:** proyecto de Supabase `javipm97's Project` (`mwsapcjzubisdszmwswp`), separado de Mi Rutina.
- **Quién escribe:** las tareas programadas de Claude guardan cada resumen en la tabla `digests`.
- **Quién lee:** solo la cuenta `javipm.97@gmail.com` (lo controla la base de datos con RLS). La clave que aparece en `index.html` es la pública; sin iniciar sesión no se ve nada.

## Tablas

`digests` — un resumen por tipo y día.

| columna | qué es |
|---|---|
| `kind` | `correo`, `empleo`, `noticias` o `cambio` |
| `run_date` | fecha (hora de Chicago) |
| `headline` | titular de una línea |
| `data` | JSON con el contenido (formato abajo) |

`item_marks` — lo que marcas en la página: correos hechos y ofertas guardadas o descartadas.

## Formato de `data`

**correo**
```json
{"resumen": "", "revisados": 0,
 "urgente": [{"asunto": "", "de": "", "detalle": "", "accion": "", "fecha_limite": ""}],
 "importante": [], "otros": [],
 "eventos_anadidos": [{"titulo": "", "cuando": "", "origen": ""}],
 "eventos_pendientes": [{"titulo": "", "motivo": ""}]}
```

**empleo**
```json
{"resumen": "",
 "ofertas": [{"puesto": "", "url": "", "empresa": "", "ciudad": "", "pais": "", "publicada": "",
   "nueva": true, "ruta": "", "marcas": ["ESPAÑOL"], "tiro_largo": false,
   "por_que": "", "punto_flojo": "", "sueldo": "", "condiciones": ""}],
 "linkedin_sin_verificar": [{"puesto": "", "empresa": "", "url": ""}], "notas": ""}
```

**noticias**
```json
{"destacadas": [{"titulo": "", "url": "", "fuente": ""}],
 "secciones": [{"nombre": "", "nota": "", "items": [{"titulo": "", "resumen": "", "url": "", "fuente": "", "de_pago": false}]}],
 "real_madrid_proximo": ""}
```

**cambio**
```json
{"fecha_dato": "", "eur_usd": 0, "usd_eur": 0, "var_1d": 0, "var_7d": 0, "var_30d": 0,
 "media_30": 0, "media_90": 0, "media_365": 0, "min_90": 0, "max_90": 0, "min_365": 0, "max_365": 0,
 "pct_90": 0, "pct_365": 0, "veredicto": "bueno|normal|esperar", "veredicto_texto": "",
 "razon": "", "tendencia": "", "contexto": "", "fuente_nombre": "", "fuente_url": "",
 "serie": [["2026-10-16", 1.0842]]}
```

## Boda (`/boda/`)

Página de solo lectura para que las familias de Javier y Laura sigan la búsqueda de finca. Es un `boda/index.html` suelto, sin compilar.

- **Datos:** tablas `boda_docs`, `boda_acceso` y `boda_intentos` del mismo proyecto de Supabase. Ninguna se puede leer directamente: la página llama a la función `boda_leer(clave)`, que comprueba la clave y devuelve los datos. Tras 10 claves erróneas en 10 minutos se bloquea unos minutos.
- **La clave no está en este repositorio.** Solo se guarda su hash en la base de datos.
- **Actualizar los datos:** los pide Javier a Claude, que vuelve a cargar `boda_docs` desde el panel de la boda.

## Mapas (`/mapas/`)

Una pregunta cada vez sobre estados de EE. UU., Latinoamérica y Centroamérica (ubicar en el mapa, nombrar, capitales, abreviaturas), 10 al día, sin menús ni opciones. Pregunta más lo que se falla. Tiene su propia pestaña en la barra de la página principal.

- **Un único `mapas/index.html`** con los mapas ya dibujados dentro (SVG generado a partir de `world-atlas` y `us-atlas`).
- **Datos:** tabla `mapas_progreso` (una sola fila, `id = 'javi'`, con el progreso en `data`). Solo la lee y escribe la cuenta `javipm.97@gmail.com`. Usa la misma sesión que la página principal; sin sesión funciona igual pero guarda solo en el navegador.

## Casos (`/casos/`)

Drills diarios para entrevistas de consultoría: cálculo mental, márgenes y P&L, lectura de tablas, gráficos, estructuras, market sizing, síntesis y brainstorming. La pantalla de inicio propone 3 al día. Tiene su propia pestaña en la barra de la página principal.

- **`casos/index.html`** con la aplicación y **`casos/data-*.js`** con el banco de 330 drills abiertos, ordenados en tres niveles. Los de cálculo, márgenes, tablas y gráficos se generan con números nuevos cada vez.
- **Datos:** tabla `casos_progreso` (una sola fila, `id = 'javi'`, con el progreso en `data`). Solo la lee y escribe la cuenta `javipm.97@gmail.com`. Usa la misma sesión que la página principal; sin sesión funciona igual pero guarda solo en el navegador.
- **Qué guarda `data`:** `days` (drills por día), `stats` (número y acierto por categoría), `done` (nota de cada drill abierto: 1 flojo, 2 regular, 3 bien), `plan` (los 3 drills de hoy) y `log` (las últimas 400 sesiones, con los fallos de cálculo y, en las 60 más recientes, el texto de la respuesta). Claude lee esta fila para seguir el progreso.

## Escala (`/escala/`)

Juego para grupos, parecido a ito: cada uno recibe en secreto un número del 1 al 100, todos ven la misma carta de tema y hay que bajar las cartas en orden sin decir los números. 3 vidas por partida. No hace falta cuenta: se entra con nombre y el código de 4 letras de la sala (o el enlace `escala/?sala=ABCD`). Tiene acceso directo en la vista «Hoy».

- **Un único `escala/index.html`**, con la lista de temas dentro. Se actualiza cada 1,5 s preguntando a la base de datos.
- **Datos:** tablas `escala_salas` y `escala_jugadores`. Nadie las lee directamente: todo pasa por las funciones `escala_crear`, `escala_unirse`, `escala_estado`, `escala_empezar`, `escala_cambiar_tema`, `escala_jugar`, `escala_pista`, `escala_quitar` y `escala_salir`. Cada navegador se identifica con un token aleatorio guardado en `localStorage`; los números de los demás solo se ven al acabar la ronda.
- Las salas sin actividad en 2 días se borran al crear una nueva. Máximo 12 jugadores por sala.
