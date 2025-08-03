# Mathesar para EasyPanel

1. En EasyPanel crea un nuevo Proyecto → Servicio tipo **App** (elige fuente GitHub apuntando a esta rama).
2. En la pestaña **Build**, selecciona **Dockerfile (ruta baja)** si no está automáticamente.
3. En la pestaña **Environment**, declara estas variables obligatorias:

    SECRET_KEY=… (50 caracteres alfanuméricos)
    POSTGRES_HOST=…  # host del servicio PostgreSQL que hayas agregado vía el panel
    POSTGRES_USER=…
    POSTGRES_PASSWORD=…
    POSTGRES_DB=…
    POSTGRES_PORT=5432

4. Opcional: si tu DNS apunta a esta app como dominio primario, puedes dejar `ALLOWED_HOSTS=*` (fallback da acceso abierto).
5. Haz click en “Deploy”.

EasyPanel se encargará del resto: hará build, expondrá el puerto 8000 del contenedor, configurará el dominio público con https/TLS gratis, etc.

---

✅ Con este enfoque **no necesitas Docker Compose ni Caddy**, ya que Mathesar se ejecuta con su solo contenedor escuchando en el puerto 8000 y EasyPanel gestiona la redirección y SSL. Esto reduce las posibles interferencias de puertos o conflicto de nombres de contenedor.

---

### 🔐 Sobre las variables de entorno

Mathesar requiere especificar algunas variables —especialmente `SECRET_KEY`, `POSTGRES_*` y opcionalmente `ALLOWED_HOSTS`— como detalla su documentación oficial  [oai_citation:0‡docs.mathesar.org](https://docs.mathesar.org/latest/administration/install-via-docker-compose/?utm_source=chatgpt.com).  

Si ya tienes una base de datos PostgreSQL en EasyPanel puedes usar esos datos directamente; no cambies `DOMAIN_NAME`, ya que EasyPanel ya proporciona el dominio HTTPS automáticamente  [oai_citation:1‡easypanel.io](https://easypanel.io/docs/services/app?utm_source=chatgpt.com).

---

### ⚠️ Consideraciones adicionales

- Asegúrate de usar **solo un** servicio App: si un contenedor anterior estaba expuesto en puertos como 80 o 443, deténlo antes.
- Regenera `SECRET_KEY` con un comando como:
  ```bash
  LC_CTYPE=C tr -dc 'a-zA-Z0-9' </dev/urandom | head -c50
