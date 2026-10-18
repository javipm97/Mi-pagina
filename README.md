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
