# laravel-docker
🐳 Generic docker image for Laravel Applications

[![Docker Badge](https://img.shields.io/docker/image-size/pixelplant/laravel-docker/latest)](https://hub.docker.com/r/pixelplant/laravel-docker)


## Current stable

**`8.4-cypress`** is the current stable image. It is published under the following tag:

- `bryanabateacheteur/laravel:latest`

Highlights: PHP 8.4 (ondrej/php) + extensions, Node.js 20 + pnpm, Composer, PostgreSQL 16, Python 3 + numpy/scipy/pandas, Google Chrome + chrome-headless-shell (Cypress-ready), Ghostscript, wkhtmltopdf, OpenVPN, OpenSSH client.

Built for `linux/amd64`:

```sh
docker build --platform=linux/amd64 -t pixelplant/laravel-docker:cleanup ./8.4-cypress
```

## Tags

| Tags | PHP version | Features |
| - | - | - |
| 7.4 | 7.4 | ✅ Everything |
| 8.0 | 8.0 | ✅ Everything |
| 8.4-cypress | **8.4** | ✅ Current stable — Cypress + Chrome + full toolchain |
| stable | **8.0** | 🔗 Aliases the latest stable version of PHP that supports all features of this docker image.  |
| latest | **8.0** | 🔗 Aliases the latest stable version of PHP available (even if that version does not support all features yet). |
