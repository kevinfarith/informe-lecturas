# Automatización: informe de lecturas + panel ATILA

Repositorio: [https://github.com/kevinfarith/informe-lecturas](https://github.com/kevinfarith/informe-lecturas)

Producción: [https://inf-iota.vercel.app](https://inf-iota.vercel.app)

Aplicación **React (Vite) + Tailwind** en `web/`, y **API FastAPI** en `api/` (Python) para generar el Excel y resumir el progreso ATILA.

## Desarrollo local

**Frontend** (`web/`):

```bash
cd web
npm install
npm run dev
```

**API** (desde la raíz de este proyecto, carpeta `automation/`):

```bash
python3 -m venv .venv
source .venv/bin/activate   # Windows: .venv\Scripts\activate
pip install -r requirements.txt
python3 -m uvicorn api.main:app --reload --host 127.0.0.1 --port 8001
```

En desarrollo, el proxy de Vite envía `/api` al puerto `8001` (ver `web/vite.config.js`).

Si al subir un **.csv** u otra actualización de la API sigue saliendo el mensaje antiguo *«Se requiere un archivo .xlsx del administrador»*, el proceso de **uvicorn** sigue con código viejo: detén la API (Ctrl+C) y vuelve a arrancarla, o usa `./start-dev.sh` (incluye `--reload`).

## GitHub y Vercel

- Remoto: `origin` → `https://github.com/kevinfarith/informe-lecturas.git` · rama `main`
- El repo está conectado al proyecto Vercel: cada push a `main` despliega a producción y las demás ramas/PR generan una vista previa.
- `vercel.json` (raíz) usa **Vercel Services**: el servicio `frontend` (`web/`, Vite) y el servicio `backend` (FastAPI, `api/main.py`) se publican en el mismo dominio; `/api/*` va a la API. No hace falta `VITE_API_BASE_URL`.
- Límite de subida a la API en Vercel: ~4,5 MB por petición.
- Despliegue de la API en otro hosting (Railway, Render…): ver [DEPLOY.md](./DEPLOY.md)

## Estructura principal

- `web/` — interfaz
- `api/main.py` — FastAPI
- `informe_builder.py` — generación del informe
- `progress_dashboard.py` — cálculo del panel de progreso
