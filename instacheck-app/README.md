# InstaCheck 🔍

Diagnóstico de Instagram con IA. Bonus para miembros anuales de **The Content Room** (Pao Digital).

La persona sube una captura de su perfil, Claude (con visión) la analiza con el criterio de Pao, y devuelve una ficha con puntaje por área y recomendaciones concretas.

---

## Cómo funciona

- `public/index.html` — la página: sube la captura, la comprime y muestra la ficha.
- `api/analizar.js` — función serverless (Vercel): manda la imagen a la API de Anthropic y devuelve el diagnóstico en JSON.

Mismo stack que el generador de hooks: Vercel (Hobby) + API de Anthropic, servido desde un subdominio de `paodigital.net`.

---

## Desplegar (paso a paso)

### 1. Subir a GitHub
Crea un repo nuevo (ej. `instacheck`) y sube esta carpeta:

```bash
cd instacheck-app
git init
git add .
git commit -m "InstaCheck inicial"
git branch -M main
git remote add origin https://github.com/TU-USUARIO/instacheck.git
git push -u origin main
```

### 2. Conectar en Vercel
1. En [vercel.com](https://vercel.com) → **Add New → Project** → importa el repo `instacheck`.
2. No cambies nada de build (es estático + función). Click **Deploy**.

### 3. Poner la API key
En el proyecto de Vercel → **Settings → Environment Variables**, agrega:

| Name | Value |
|------|-------|
| `ANTHROPIC_API_KEY` | tu API key de Anthropic |

Luego **Deployments → Redeploy** para que tome la variable. (Es la misma key que usas en el generador de hooks.)

### 4. Subdominio (opcional)
Para servirlo en `instacheck.paodigital.net`:
1. En Vercel → **Settings → Domains** → agrega `instacheck.paodigital.net`.
2. En SiteGround → **DNS Zone Editor** → crea un **CNAME**:
   - Host: `instacheck`
   - Apunta a: `cname.vercel-dns.com` (Vercel te muestra el valor exacto)
3. Espera a que propague. Listo.

### 5. Ponerlo en Skool
Copia el enlace final y agrégalo como recurso del bono anual en tu comunidad.

---

## Costo
Cada análisis consume API de Anthropic (unos centavos por uso, según el tamaño de la imagen). Para un grupo de miembros es volumen controlado. Puedes ver el gasto en console.anthropic.com.

## Notas
- La captura no se guarda: se usa solo para generar la ficha y se descarta.
- El modelo analiza únicamente lo que se ve en la imagen; si algo no aparece (como el contenido de los reels), lo dice en vez de inventarlo.
- Para evitar que gente de fuera lo use, mantén el enlace solo dentro de Skool.
