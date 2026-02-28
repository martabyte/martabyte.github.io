# h4ck3r_blog — Jekyll setup

## Estructura

```
.
├── _config.yml          ← configuración global + datos del autor
├── _layouts/
│   ├── default.html     ← base (nav + footer)
│   ├── home.html        ← index con matrix rain + lista de posts
│   └── post.html        ← página de entrada individual
├── _includes/
│   ├── nav.html         ← barra de navegación
│   ├── footer.html      ← statusbar inferior
│   └── matrix.html      ← hero con matrix rain + boot sequence
├── _sass/
│   └── main.scss        ← todos los estilos
├── _posts/
│   └── YYYY-MM-DD-slug.md   ← tus posts aquí
├── pages/
│   └── whoami.md        ← página whoami
└── index.md             ← homepage
```

## Instalación

```bash
gem install bundler
bundle install
bundle exec jekyll serve
```

## Publicar en GitHub Pages

1. Sube el repo a `tuusuario/tuusuario.github.io`
2. En Settings → Pages → Source: `main` branch
3. Cambia la gem a `github-pages` en el Gemfile (ver comentario)

## Crear un post nuevo

```bash
# Crea el archivo en _posts/
touch _posts/$(date +%Y-%m-%d)-mi-writeup.md
```

Frontmatter mínimo:

```yaml
---
layout: post
title: "Título del post"
date: 2025-03-01 12:00:00 +0000
tags: [web, sqli]
difficulty: medium
excerpt: "Descripción corta para el index."
---
```

## Personalización

Edita `_config.yml` para cambiar:
- Nombre, alias, certs, HTB stats → sección `author:`
- Redes sociales → `author.contact:`
- Certificaciones → `author.certs:`

Los skills del sidebar se calculan **automáticamente** por frecuencia de tags en los posts.

## Posts bilingües

Añade `lang: es` o `lang: en` al frontmatter de cada post:

```yaml
---
layout: post
title: "Mi writeup"
lang: es        # ← esto alimenta el toggle del index
tags: [web]
---
```

Si no especificas `lang`, se asume `es` por defecto.
