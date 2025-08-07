# GitHub Actions Setup

Este directorio contiene la configuración de GitHub Actions para el despliegue automático de la aplicación.

## Configuración de Secrets

Para que el workflow funcione correctamente, necesitas configurar los siguientes secrets en tu repositorio de GitHub:

### 1. Ir a Settings > Secrets and variables > Actions

### 2. Agregar los siguientes secrets:

#### Para Desarrollo (branch `develop`):
- `DEV_APP_NAME`: Nombre de tu Azure App Service para desarrollo
- `AZURE_WEBAPP_PUBLISH_PROFILE_DEV`: Perfil de publicación de Azure para desarrollo

#### Para Producción (branch `main`):
- `PROD_APP_NAME`: Nombre de tu Azure App Service para producción  
- `AZURE_WEBAPP_PUBLISH_PROFILE_PROD`: Perfil de publicación de Azure para producción

### 3. Cómo obtener el Publish Profile de Azure:

1. Ve al Azure Portal
2. Navega a tu App Service
3. En el menú lateral, ve a "Overview"
4. Haz clic en "Get publish profile"
5. Descarga el archivo .PublishSettings
6. Abre el archivo y copia el contenido del elemento `<publishProfile>`
7. Pega este contenido como el valor del secret correspondiente

## Flujo de Trabajo

El workflow se ejecuta automáticamente cuando:

- Se hace push a las ramas `develop` o `main`
- Se crea un Pull Request hacia `develop` o `main`

### Pasos del Workflow:

1. **Checkout**: Descarga el código del repositorio
2. **Setup Node.js**: Configura Node.js LTS con cache de npm
3. **Install Dependencies**: Instala las dependencias
4. **Build**: Construye la aplicación con `npm run build`
5. **Package**: Crea el paquete de despliegue (ZIP)
6. **Deploy**: Despliega a Azure App Service según la rama

## Diferencias con GitLab CI

- Usa `actions/setup-node@v4` en lugar de `node:lts` image
- Usa `azure/webapps-deploy@v3` en lugar de Azure CLI
- Los secrets se configuran en GitHub en lugar de variables de GitLab
- El workflow es más simple y directo
