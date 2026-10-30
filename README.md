# Gen · Asistente de rediseño de prendas con IA

Trabajo Final Integrador · Diplomatura en IA Aplicada a Entornos Digitales de Gestión · FCE-UBA · Cohorte 2026

**Autora:** Mariel Herbón

---

## Elegí este tema porque...

**Igriega** es una marca de ropa que creé hace dos años junto a mi hija. **Gen** es una cápsula dentro de Igriega, especializada en suprareciclaje. La "Y" de Igriega representa el camino de la marca; la otra pata de la "Y" es la historia que trae cada prenda usada. Las dos se unen en un solo camino: Gen.

Gen nace de una preocupación concreta: el impacto del *fast fashion*. Este modelo produce ropa en grandes volúmenes, con ciclos cada vez más cortos y a precios bajos, lo que empuja a comprar mucho y usar poco. El resultado es una enorme cantidad de prendas que se descartan cuando todavía podrían seguir en uso y que terminan como residuos textiles, además del agua, la energía y las materias primas que cada prenda nueva consume.

El suprareciclaje propone otra salida: en lugar de tirar, crear algo nuevo a partir de una prenda que ya existe. No se trata de arreglar o restaurar la prenda original, sino de transformarla en una pieza distinta, que conserva su material y su historia pero tiene un diseño propio. Gen busca, con este trabajo, sumar a la concientización sobre el impacto del fast fashion y mostrar que hay una alternativa concreta.

Pero ese camino tiene un obstáculo: cada prenda es distinta, y cada pedido pide una consulta personalizada. Ahí es donde la IA agrega valor: en el primer paso de esa consulta.

## Qué hace

Un workflow de **n8n** que recibe una prenda por formulario y le devuelve a la clienta, por mail, **tres propuestas de transformación** (baja, media y alta intervención), cada una con su texto y una imagen de referencia.

## Para qué sirve

- **Ahorrar tiempo en la consulta.** La persona completa un formulario, sube las fotos de su prenda y recibe por mail tres opciones de transformación. Elige la que más le gusta (o mezcla elementos de varias) y recién entonces el equipo de Gen continúa con esa elección, sin tener que armar las propuestas una por una desde cero.
- **Mantener un lenguaje de diseño coherente.** La identidad de la marca (ADN y recursos constructivos) está definida en el prompt, así que todas las propuestas hablan el mismo idioma.
- **Dar una devolución visual y rápida.** Cada opción llega con una imagen de referencia, para que quien consulta pueda imaginar el resultado antes de decidir.
- **Dedicar el tiempo del equipo al taller.** La etapa previa de ideación queda resuelta y el trabajo se concentra en la pieza elegida.

## Cómo funciona

1. **Formulario** (n8n Form Trigger): datos de la clienta y de la prenda, y tres fotos (frente, espalda, interior/etiqueta). Incluye consentimiento de uso de datos.
2. **Análisis de las tres fotos** (GPT-5.1 con visión): descripción técnica de cada vista y clasificación del textil.
3. **Unión de análisis** (Merge + Code): se consolidan en un solo informe de la prenda.
4. **Generación de propuestas** (GPT-4.1): tres opciones con nombre, concepto, qué se conserva, qué se modifica, recursos constructivos y resultado visual esperado.
5. **Separación de opciones** (Code): se divide el texto en baja, media y alta.
6. **Imágenes de referencia** (gpt-image-2, en paralelo): una por propuesta, a partir de la foto frontal.
7. **Armado y envío** (Code + Gmail): mail HTML con las tres propuestas y las imágenes adjuntas, con copia oculta (BCC) al equipo.

## Modelos y herramientas de IA usados

| Etapa | Herramienta |
|---|---|
| Orquestación | n8n |
| Análisis de fotos | GPT-5.1 (visión) |
| Propuestas de diseño | GPT-4.1 |
| Imágenes de referencia | gpt-image-2 |
| Envío | Gmail (OAuth2) |

## Cómo se usa

**Opción A: probar el formulario publicado**

URL: `https://igriega.app.n8n.cloud/form/e59ff510-5495-41ce-b206-7e4182447ff2` (activo hasta 15 de Noviembre)

1. Completá los datos y subí las tres fotos.
2. Aceptá el consentimiento y enviá.
3. En pocos minutos llega el mail con las tres propuestas y las imágenes.

**Opción B: importar el workflow en tu propio n8n**

1. En n8n: *Create workflow* → menú ⋯ → *Import from file* → elegí `workflow/gen-workflow.json`.
2. Configurá tus propias credenciales de **OpenAI** y **Gmail** (el archivo no las incluye).
3. En el nodo *Send a message*, reemplazá el BCC por tu mail.
4. Ejecutá el workflow, abrí la URL de prueba del formulario y envialo.

## Estructura del repositorio

```
├── README.md
├── workflow/
│   └── gen-workflow.json
├── capturas/
└── informe/
    └── informe.pdf
```

## Nivel de ambición

**Nivel 1: implementación funcional**, probada de punta a punta.

## Decisiones de diseño y límites

- **Los precios quedan fuera de la IA.** Los define el equipo de Gen por separado, una vez que la clienta elige.
- **La clienta elige.** El mail es una propuesta; las imágenes son una referencia visual, no un resultado exacto.
- **Las imágenes se generan solo desde la foto frontal.** La espalda y el interior se usan en el análisis y en el texto.
- **El mail sale sin revisión previa.** El equipo recibe una copia (BCC), pero no hay una aprobación antes del envío. Es una mejora pendiente.
- **Las tres fotos son obligatorias.** Admitir solo el frente quedó como mejora futura.

## Privacidad

El formulario pide consentimiento explícito. Las fotos y los datos se usan solo para elaborar las propuestas y contactar a la clienta.
