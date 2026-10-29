# Startify

Generador de ideas de negocio. El usuario responde un cuestionario de 8 preguntas y recibe, al instante, **3 nombres de marca con logo**, un **presupuesto estimado por rubros** y un **business plan preliminar**.

Es una página estática de un solo archivo (`index.html`): sin dependencias, sin backend y sin registro. Funciona en cualquier navegador moderno, en celular, tablet o escritorio, y también sin conexión.

**Demo:** `https://TU-USUARIO.github.io/TU-REPO/`

## Qué hace

- **Cuestionario tipo wizard** con validación y preguntas condicionales (modalidad de operación según el giro, y diferenciador si el negocio es muy competido).
- **Nombres de marca:** se generan combinando las palabras clave del usuario con reglas fonéticas. El botón «Nuevos nombres» regenera los tres.
- **Logos:** un logo SVG por nombre (estilo flat/minimalista), con forma según el giro y la paleta elegida. Se descargan en SVG, PNG (fondo transparente) y JPG.
- **Presupuesto:** reparto por rubros según el rango del presupuesto de arranque, con tabla, gráfica de barras y una referencia de meses de nómina cubiertos.
- **Business plan:** misión, visión, valores, estudio de mercado simple, modelo de negocio y áreas de oportunidad. El texto se adapta a giro, ubicación, horario, equipo, presupuesto y competencia.
- **Descargas:** PDF (imprimir y «Guardar como PDF»), presupuesto en CSV y plan en TXT.
- **Extras:** modo oscuro (automático o manual), diseño responsivo y accesible (foco visible, navegación con teclado, respeta `prefers-reduced-motion`).

## Cómo usarlo

### En local

Abre `index.html` con doble clic. No necesita servidor.

### En GitHub Pages

1. Crea un repositorio y sube `index.html` (y este `README.md`) a la raíz.
2. Ve a **Settings → Pages**.
3. En **Build and deployment**, elige **Deploy from a branch**, rama `main` y carpeta `/ (root)`.
4. Espera uno o dos minutos. La URL aparece en esa misma pantalla.

## Reparto del presupuesto

| Presupuesto (MXN) | Fijos | Variables | Otros | Materias primas | Admin. y ventas |
| --- | --- | --- | --- | --- | --- |
| Hasta $50,000 | 35% | 10% | 10% | 25% | 20% |
| $50,001 a $200,000 | 30% | 15% | 10% | 25% | 20% |
| $200,001 a $1,000,000 | 25% | 20% | 10% | 25% | 20% |
| Más de $1,000,000 | 20% | 25% | 10% | 25% | 20% |

Para la referencia de nómina se usa un sueldo mensual de $9,000 por persona y el punto medio de cada rango de equipo (3, 13, 35 y 60 personas).

## Personalización

Todo está en el bloque `<script>` de `index.html`:

| Qué cambiar | Dónde |
| --- | --- |
| Giros, paletas, forma del logo, valores y textos por giro | Objeto `G` |
| Opciones de modalidad por giro | Objeto `MODAL` |
| Rangos y porcentajes del presupuesto | Constantes `RANGES` y `RUB` |
| Rangos de equipo y sueldo de referencia | `WORK`, `WN` y el valor `9000` dentro de `plan()` |
| Horarios disponibles | Arreglo `HOR` |
| Colores e interfaz | Variables CSS en `:root` |

## Agregar IA (opcional)

Un sitio estático no puede guardar una API key de forma segura: cualquiera podría verla en el código. Por eso la IA se conecta mediante **tu propio endpoint** (por ejemplo, una función en Cloudflare Workers, Vercel o Netlify) que guarda la clave y llama al modelo.

1. Despliega tu función y copia su URL.
2. Pégala en la constante `AI_ENDPOINT` de `index.html`.

La página enviará un `POST` con este cuerpo:

```json
{
  "answers": { "kw": ["café", "montaña"], "giro": "alimentos", "pres": "150000", "...": "..." },
  "names": ["Nombre1", "Nombre2", "Nombre3"]
}
```

Y espera una respuesta JSON con cualquiera de estos campos (todos opcionales):

```json
{
  "names": ["Nombre1", "Nombre2", "Nombre3"],
  "plan": {
    "mision": "texto",
    "vision": "texto",
    "valores": ["..."],
    "mercado": "texto",
    "modelo": "texto",
    "oport": ["..."],
    "nota": "texto"
  }
}
```

Los campos que mandes reemplazan a los generados por reglas. Si el endpoint falla o está vacío, la página sigue funcionando con la versión basada en reglas.

## Privacidad

Las respuestas se guardan solo en el navegador del usuario (`localStorage`, clave `startify.a`) para poder retomarlas. No se envían a ningún servidor, salvo que configures `AI_ENDPOINT`. El botón «Empezar de nuevo» las borra.

## Limitaciones y siguientes pasos

- Los nombres y textos se generan con reglas; son un punto de partida, no un estudio de mercado real.
- Las cifras son estimaciones orientativas y no sustituyen asesoría financiera, contable ni legal.
- El PDF depende del diálogo de impresión del navegador.
- Pendiente para una segunda fase: cuentas de usuario, pagos, entrega en `.zip`, panel de administración y envío por email (requieren backend).

## Compatibilidad

Navegadores actuales: Chrome, Edge, Firefox y Safari (escritorio, iOS y Android). La exportación a PNG y JPG usa `canvas`; si algún navegador falla, queda disponible la descarga en SVG.
