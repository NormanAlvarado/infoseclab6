# Auth0 Express Application

Una aplicación web simple construida con Express.js que implementa autenticación usando Auth0 OpenID Connect.

## Características

- 🔐 Autenticación segura con Auth0
- 🏠 Página de inicio que muestra el estado de autenticación
- 📊 Dashboard protegido para usuarios autenticados
- 🔧 Configuración mediante variables de entorno
- 🎨 Archivos estáticos CSS incluidos

## Tecnologías Utilizadas

- **Node.js** - Entorno de ejecución de JavaScript
- **Express.js** - Framework web para Node.js
- **Auth0** - Servicio de autenticación e identidad
- **express-openid-connect** - Middleware de Auth0 para Express
- **Swig** - Motor de plantillas
- **dotenv** - Manejo de variables de entorno

## Estructura del Proyecto

```
├── server.js              # Servidor principal de la aplicación
├── package.json           # Dependencias y scripts del proyecto
├── .env                   # Variables de entorno (no incluido en git)
├── .env.example          # Ejemplo de variables de entorno
├── .gitignore            # Archivos ignorados por git
├── static/               # Archivos estáticos (CSS, JS, imágenes)
│   └── css/
│       └── style.css     # Estilos de la aplicación
└── views/                # Plantillas HTML
    ├── index.html        # Página de inicio
    └── dashboard.html    # Dashboard del usuario
```

## Instalación

### Prerrequisitos

- Node.js (versión 14 o superior)
- npm o yarn
- Cuenta de Auth0

### Pasos de Instalación

1. **Clona o descarga el proyecto**
   ```bash
   git clone <tu-repositorio>
   cd <nombre-del-proyecto>
   ```

2. **Instala las dependencias**
   ```bash
   npm install
   ```

3. **Configura las variables de entorno**
   
   Copia el archivo de ejemplo y configura tus credenciales:
   ```bash
   cp .env.example .env
   ```
   
   Edita el archivo `.env` con tus credenciales de Auth0:
   ```env
   AUTH0_SECRET=tu_secreto_largo_y_aleatorio_aqui
   AUTH0_BASE_URL=http://localhost:3000
   AUTH0_CLIENT_ID=tu_client_id_de_auth0
   AUTH0_ISSUER_BASE_URL=https://tu-dominio.auth0.com
   PORT=3000
   ```

## Configuración de Auth0

### 1. Crear una Aplicación en Auth0

1. Ve a tu [Dashboard de Auth0](https://manage.auth0.com/)
2. Navega a **Applications** > **Applications**
3. Haz clic en **Create Application**
4. Selecciona **Regular Web Application**
5. Elige **Node.js (Express)** como tecnología

### 2. Configurar la Aplicación

En la configuración de tu aplicación de Auth0:

- **Allowed Callback URLs**: `http://localhost:3000/callback`
- **Allowed Logout URLs**: `http://localhost:3000`
- **Allowed Web Origins**: `http://localhost:3000`

### 3. Obtener Credenciales

Copia las siguientes credenciales de tu aplicación Auth0:
- **Domain** (para `AUTH0_ISSUER_BASE_URL`)
- **Client ID** (para `AUTH0_CLIENT_ID`)

## Uso

### Iniciar el Servidor

```bash
npm start
```

El servidor se ejecutará en `http://localhost:3000`

### Rutas Disponibles

- **`/`** - Página de inicio (muestra si el usuario está autenticado)
- **`/login`** - Inicia el proceso de autenticación (generada automáticamente por Auth0)
- **`/logout`** - Cierra la sesión del usuario (generada automáticamente por Auth0)
- **`/dashboard`** - Dashboard protegido (requiere autenticación)
- **`/callback`** - Callback de Auth0 (generada automáticamente)

### Flujo de Autenticación

1. El usuario visita la página principal (`/`)
2. Si no está autenticado, puede hacer clic en "Login" o visitar `/login`
3. Es redirigido a Auth0 para autenticarse
4. Después de la autenticación exitosa, regresa a la aplicación
5. Puede acceder al dashboard protegido en `/dashboard`

## Variables de Entorno

| Variable | Descripción | Ejemplo |
|----------|-------------|---------|
| `AUTH0_SECRET` | Secreto largo y aleatorio para firmar cookies | `una_cadena_larga_y_aleatoria` |
| `AUTH0_BASE_URL` | URL base de tu aplicación | `http://localhost:3000` |
| `AUTH0_CLIENT_ID` | Client ID de tu aplicación Auth0 | `E2hORy9j8koBxM8UPr5194AgFZ5UzSKA` |
| `AUTH0_ISSUER_BASE_URL` | URL de tu tenant de Auth0 | `https://tu-dominio.auth0.com` |
| `PORT` | Puerto donde se ejecutará el servidor | `3000` |

## Desarrollo

### Scripts Disponibles

- `npm start` - Inicia el servidor en modo producción
- `npm run dev` - Inicia el servidor en modo desarrollo (si tienes nodemon configurado)

### Agregar Nuevas Rutas Protegidas

Para crear una nueva ruta que requiera autenticación:

```javascript
app.get('/mi-ruta-protegida', requiresAuth(), (req, res) => {
  // El usuario está autenticado
  const usuario = req.oidc.user;
  res.render('mi-plantilla', { user: usuario });
});
```

### Verificar Autenticación en Plantillas

En tus plantillas HTML puedes verificar si el usuario está autenticado:

```javascript
// En el servidor
app.get('/mi-ruta', (req, res) => {
  res.render('mi-plantilla', { 
    isAuthenticated: req.oidc.isAuthenticated(),
    user: req.oidc.user 
  });
});
```

## Seguridad

- ✅ Las credenciales están almacenadas en variables de entorno
- ✅ El archivo `.env` está excluido del control de versiones
- ✅ Se usa HTTPS en producción (configurar en el servidor)
- ✅ Las rutas sensibles están protegidas con `requiresAuth()`

## Solución de Problemas

### Error: "Cannot find module 'dotenv'"
```bash
npm install dotenv
```

### Error: "AUTH0_SECRET is undefined"
Verifica que el archivo `.env` existe y contiene todas las variables necesarias.

### Error de CORS o Callback
Verifica que las URLs de callback estén configuradas correctamente en tu aplicación de Auth0.

### Advertencia sobre form_post en HTTP
Esta advertencia es normal en desarrollo local. En producción, usa HTTPS.

## Contribuir

1. Fork el proyecto
2. Crea una rama para tu feature (`git checkout -b feature/nueva-caracteristica`)
3. Commit tus cambios (`git commit -m 'Agrega nueva característica'`)
4. Push a la rama (`git push origin feature/nueva-caracteristica`)
5. Abre un Pull Request

## Licencia

Este proyecto está bajo la Licencia MIT. Ver el archivo `LICENSE` para más detalles.

## Enlaces Útiles

- [Documentación de Auth0](https://auth0.com/docs)
- [Express OpenID Connect](https://github.com/auth0/express-openid-connect)
- [Express.js](https://expressjs.com/)
- [Node.js](https://nodejs.org/)

## Soporte

Si encuentras algún problema o tienes preguntas, por favor:

1. Revisa la sección de [Solución de Problemas](#solución-de-problemas)
2. Consulta la [documentación de Auth0](https://auth0.com/docs)
3. Abre un issue en el repositorio del proyecto

---

**Desarrollado con ❤️ usando Node.js y Auth0**
