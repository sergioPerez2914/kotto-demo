# kotto — sitio

Landing page de **kotto**, infraestructura de cobro recurrente para Venezuela. La demo interactiva (pagador, mensual/anual, conciliación) vive embebida como modal dentro de la misma página — se abre tocando el teléfono del hero.

```
index.html      → landing completa, con la demo integrada
evolucion.html  → muestra del Acto 2: pago único (tienda de ropa) y B2B conciliado (boutique → mayorista)
```

Un solo archivo estático, sin dependencias ni build. Se abre directo en el navegador.

---

## Despliegue con control de versiones

La idea: **GitHub guarda el historial, el host publica solo con cada push.** Se configura una vez.

### 1. Repositorio

```bash
cd kotto-site
git init
git add .
git commit -m "Landing de kotto"
git branch -M main
```

Crea un repo vacío en GitHub (sin README ni .gitignore, para que no choque) y conéctalo:

```bash
git remote add origin https://github.com/sergioPerez2914/kotto-demo.git
git push -u origin main
```

### 2. Publicación automática — elige una

**Cloudflare Pages** (recomendado: rápido, dominio limpio, sin límite de ancho de banda)
1. dash.cloudflare.com → Workers & Pages → Create → Pages → Connect to Git
2. Elige el repo. **Framework preset:** None. **Build command:** vacío. **Output directory:** `/`
3. Deploy → te da `https://kotto-site.pages.dev`

**Netlify** (igual de simple, buena UI)
1. app.netlify.com → Add new site → Import an existing project → GitHub
2. Build command vacío, publish directory `.`
3. Te da `https://algo-aleatorio.netlify.app` — renómbralo en Site settings → Change site name

**GitHub Pages** (cero servicios extra, pero URL más larga)
1. Repo → Settings → Pages
2. Source: Deploy from a branch → `main` / `/ (root)` → Save
3. Te da `https://TU_USUARIO.github.io/kotto-site/`

En las tres, cada `git push` a `main` republica en segundos.

### 3. Flujo de trabajo diario

```bash
git add .
git commit -m "Ajusta copy del CTA"
git push
```

Para probar cambios sin tocar el link que ya entregaste:

```bash
git checkout -b nueva-seccion
# ...cambios...
git push -u origin nueva-seccion
```

Cloudflare y Netlify generan una **URL de preview** para esa rama. El link oficial (`main`) queda intacto hasta que hagas merge.

---

## Antes de entregar el link al hackathon

- [ ] **Congela la versión juzgada** con una etiqueta, para poder volver a ella:
      `git tag -a v1.0-hackathon -m "Versión entregada" && git push --tags`
- [ ] **Actualiza `og:url`** en `index.html` con tu dominio real (está en el `<head>`, con un comentario). Sin eso, la vista previa al compartir apunta a otro sitio.
- [ ] **Ponle un nombre decente al sitio** en el panel del host. `kotto.pages.dev` se lee mejor que `kotto-site-a8f3.netlify.app`.
- [ ] **Prueba en teléfono**, no solo en escritorio. Los jueces van a abrir el link en el celular.
- [ ] **Verifica la demo**: toca el teléfono del hero, cambia mensual/anual, confirma el pago.
- [ ] **Ten un plan B sin internet**: es un archivo estático, así que puedes abrir `index.html` desde el disco si la conexión falla en la presentación. Ojo: las tipografías vienen de Google Fonts, así que sin conexión se ven con las del sistema (la página funciona igual).

## Dominio propio (opcional)

Si quieres `kotto.app` o similar en vez del subdominio del host: compra el dominio, y en Cloudflare Pages / Netlify ve a *Custom domains* → agrégalo → apunta los DNS según te indiquen. **Verifica disponibilidad del nombre antes de imprimirlo en cualquier material** — y ten en cuenta que "kotto" es una palabra común, así que puede estar tomado en las extensiones más obvias.

---

## Nota

El sitio declara explícitamente que es un concepto en etapa de diseño y que nada mueve dinero real. Conviene mantener ese aviso mientras no exista la alianza bancaria y el piloto en producción.
