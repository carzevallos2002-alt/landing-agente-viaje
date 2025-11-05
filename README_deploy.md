# Despliegue de la landing en GitHub Pages

Esta guía explica cómo publicar la carpeta `landing/` de este proyecto en GitHub Pages (gratuito) de dos maneras:

- Método A — usando GitHub (manual): crear un repo y subir la carpeta `landing/` como `gh-pages` branch o como sitio de Pages desde `main`.
- Método B — usar la CLI `gh` para crear el repo y subir automáticamente.
- Método C — usar GitHub Actions (incluido en este repo) para publicar automáticamente la carpeta `landing/` en Pages cuando empujes a `main`.

Requisitos
- Tener una cuenta en GitHub.
- Git instalado y configurado (nombre/email).
- Opcional: GitHub CLI (`gh`) para automatizar la creación del repositorio.

Método A — manual (más sencillo)
1. Crea un nuevo repositorio en GitHub (p. ej. `agente-viaje-landing`).
2. En tu máquina local, dentro de la carpeta del proyecto, inicializa un repo si aún no lo tienes:

```powershell
git init
git add .
git commit -m "initial"
```

3. Añade el remoto y sube la rama principal:

```powershell
git remote add origin https://github.com/<tu_usuario>/<tu_repo>.git
git push -u origin main
```

4. En GitHub > Settings del repo > Pages, selecciona la rama `main` (o `gh-pages`) y la carpeta `/(root)` o `/(docs)` según dónde hayas puesto los archivos. Si subes la carpeta `landing/` directamente en la raíz, la opción raíz funcionará.

5. Espera unos minutos y abre: `https://<tu_usuario>.github.io/<tu_repo>/` (o la URL que GitHub Pages te muestre).

Método B — con GitHub CLI (`gh`) (recomendado si lo tienes)
1. Instala y autentica `gh`: https://cli.github.com/
2. Desde la raíz del proyecto ejecuta el script PowerShell `deploy_landing.ps1` incluido en este repo (abre PowerShell como usuario normal):

```powershell
.\deploy_landing.ps1
```

El script te pedirá un nombre de repo y, si `gh` está disponible, creará el repo, subirá la carpeta `landing/` y empujará los cambios. Revisa el script antes de ejecutarlo.

Método C — GitHub Actions (Automático)
1. Este repositorio contiene un workflow en `.github/workflows/deploy_landing.yml` que usa `peaceiris/actions-gh-pages` para desplegar la carpeta `landing/` a GitHub Pages.
2. Para usarlo necesitarás:
   - Crear un token de GitHub con permisos `repo` (Settings > Developer settings > Personal access tokens) y añadirlo en tu repo como secreto `ACTIONS_DEPLOY_KEY` o `GH_PAT` según las instrucciones del workflow.
   - Ajustar el `path` del workflow si es necesario (por defecto apunta a `landing/`).
3. Empuja a `main` y el workflow publicará automáticamente la carpeta `landing/` en Pages.

Notas de seguridad y privacidad
- La landing incluye un formulario de captura que guarda localmente; si quieres recopilar leads en servidor real, prepara un endpoint y cumple GDPR (aviso, política de privacidad, consentimiento).

Si quieres, puedo:
- preparar el repo (crear el repo en GitHub si me das el nombre y confirmas),
- o ejecutarlo localmente (te guío paso a paso),
- o configurar el workflow y mostrar cómo establecer el token y el secreto.
