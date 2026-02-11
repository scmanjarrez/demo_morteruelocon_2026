# 🚀 Guía de Despliegue - Demo MorterueloCon 2026

Esta guía te ayudará a desplegar la demo para la MorterueloCon 2026.

## 📋 Requisitos Previos

### Software Necesario:

- **Node.js** v18 o superior
- **pnpm** v8 o superior
- **Git**
- **Telegram Desktop** (para probar la Mini App)
- Un **bot de Telegram** (creado con @BotFather)

### Conocimientos Recomendados:

- Conceptos básicos de Telegram Bots
- Uso de línea de comandos
- Deploy de aplicaciones web

## 🛠️ Paso 1: Configuración Local

### 1.1 Clonar el Repositorio

```bash
git clone [URL_DEL_REPO]
cd demo_morteruelocon_2026
```

### 1.2 Instalar Dependencias

```bash
pnpm install
```

### 1.3 Ejecutar en Modo Desarrollo

```bash
pnpm run dev
```

La aplicación estará disponible en `http://localhost:5173`

### 1.4 Ejecutar con HTTPS (recomendado para testing)

```bash
pnpm run dev:https
```

Esto generará certificados SSL locales. La aplicación estará en `https://localhost:5173`

**Nota**: Necesitarás aceptar el certificado autofirmado en tu navegador.

## 🤖 Paso 2: Crear Bot de Telegram

### 2.1 Abrir BotFather

1. Abre Telegram
2. Busca `@BotFather`
3. Inicia una conversación con `/start`

### 2.2 Crear Nuevo Bot

```
/newbot
```

Sigue las instrucciones:
- Nombre del bot: `MorterueloCon Demo`
- Username: `morteruelo_demo_bot` (debe terminar en `_bot`)

**Guarda el token** que te proporciona. Lo necesitarás más adelante.

### 2.3 Crear la Mini App

```
/newapp
```

Selecciona el bot que acabas de crear y proporciona:

- **Título**: MorterueloCon 2026 Demo
- **Descripción**: Demo educativa sobre seguridad en Mini Apps
- **Foto**: (sube una imagen representativa)
- **GIF de demo**: (opcional)
- **URL de la Web App**: Aquí va la URL donde desplegues la app

## 🌐 Paso 3: Despliegue en Producción

Tienes varias opciones para desplegar:

### Opción A: GitHub Pages (Gratis, Recomendado)

#### 3.1 Configurar GitHub Pages

1. Sube el proyecto a GitHub
2. Ve a Settings → Pages
3. Selecciona la rama `gh-pages` (se creará automáticamente)

#### 3.2 Desplegar

```bash
pnpm run deploy
```

Esto construirá la app y la desplegará en GitHub Pages.

Tu URL será: `https://[tu-usuario].github.io/demo_morteruelocon_2026/`

#### 3.3 Configurar Base URL (si usas subdirectorio)

Edita `vite.config.ts`:

```typescript
export default defineConfig({
  base: '/demo_morteruelocon_2026/', // Tu subdirectorio
  // ... resto de la configuración
});
```

### Opción B: Vercel (Gratis, Muy Fácil)

#### 3.1 Instalar Vercel CLI

```bash
npm install -g vercel
```

#### 3.2 Desplegar

```bash
vercel
```

Sigue las instrucciones en pantalla.

#### 3.3 Usar la URL de Producción

```bash
vercel --prod
```

### Opción C: Netlify (Gratis, Popular)

#### 3.1 Instalar Netlify CLI

```bash
npm install -g netlify-cli
```

#### 3.2 Construir la App

```bash
pnpm run build
```

#### 3.3 Desplegar

```bash
netlify deploy --prod --dir=dist
```

### Opción D: Servidor Propio (VPS)

#### 3.1 Construir para Producción

```bash
pnpm run build
```

#### 3.2 Configurar Nginx

```nginx
server {
    listen 80;
    server_name tu-dominio.com;
    
    root /path/to/demo_morteruelocon_2026/dist;
    index index.html;
    
    location / {
        try_files $uri $uri/ /index.html;
    }
}
```

#### 3.3 Configurar SSL con Let's Encrypt

```bash
sudo certbot --nginx -d tu-dominio.com
```

## 🔗 Paso 4: Configurar la Mini App

### 4.1 Actualizar el Manifest

Edita `public/tonconnect-manifest.json`:

```json
{
  "url": "https://tu-dominio.com",
  "name": "MorterueloCon 2026 Demo",
  "iconUrl": "https://tu-dominio.com/icon-512x512.png"
}
```

### 4.2 Configurar la URL en BotFather

Vuelve a @BotFather:

```
/myapps
```

Selecciona tu app y actualiza:
- **URL de la Web App**: `https://tu-dominio.com`

### 4.3 Verificar el Despliegue

1. Abre Telegram
2. Busca tu bot
3. Inicia la Mini App
4. Verifica que todo funciona correctamente

## 🧪 Paso 5: Testing

### 5.1 Testing Local con Telegram Desktop

1. Abre Telegram Desktop
2. Usa tu URL de desarrollo (con HTTPS)
3. Puedes usar ngrok para exponer localhost:

```bash
# Instalar ngrok
npm install -g ngrok

# Exponer puerto 5173
ngrok http 5173
```

Usa la URL HTTPS que proporciona ngrok.

### 5.2 Testing de Funcionalidades

Verifica que funcionen:

- ✅ Botón de validación
- ✅ Inyección en portapapeles
- ✅ Popup de Telegram
- ✅ Botón de ver portapapeles
- ✅ Visualización del payload
- ✅ Navegación entre páginas

### 5.3 Testing en Diferentes Plataformas

- **Telegram Desktop Windows**
- **Telegram Desktop macOS**
- **Telegram Desktop Linux**
- **Telegram Web** (funcionalidad limitada)
- **Telegram Mobile** (iOS/Android) - comportamiento diferente

## 📱 Paso 6: Configuración para la Demo

### 6.1 Preparar el Entorno

#### En tu Laptop:

1. **Telegram Desktop**: instalado y con sesión iniciada
2. **Terminal preparada**: sin historial sensible
3. **Navegador**: con DevTools disponible
4. **Capturas de respaldo**: por si falla algo

#### Crear un Script de Demo:

```bash
#!/bin/bash
# demo-setup.sh

echo "🚀 Preparando demo MorterueloCon 2026..."

# Limpiar terminal
clear

# Limpiar portapapeles
echo "" | pbcopy  # macOS
# echo "" | xclip -selection clipboard  # Linux

# Verificar que Telegram Desktop está corriendo
pgrep -x "Telegram" > /dev/null && echo "✅ Telegram Desktop: Running" || echo "❌ Telegram Desktop: Not running"

# Abrir la Mini App
open "tg://resolve?domain=morteruelo_demo_bot&appname=mortcon2026"  # macOS
# xdg-open "tg://..."  # Linux

echo "✅ Demo lista"
```

### 6.2 Preparar Comandos Útiles

#### Ver Portapapeles:

```bash
# macOS
pbpaste

# Linux
xclip -o

# Windows (PowerShell)
Get-Clipboard
```

#### Limpiar Portapapeles:

```bash
# macOS
echo "" | pbcopy

# Linux
echo "" | xclip -selection clipboard

# Windows (PowerShell)
Set-Clipboard -Value ""
```

## 🎤 Paso 7: Durante la Presentación

### 7.1 Checklist Pre-Demo

```
[ ] Laptop cargada
[ ] Conexión a Internet estable
[ ] Telegram Desktop abierto y autenticado
[ ] Terminal preparada y limpia
[ ] Mini App accesible
[ ] Herramientas de portapapeles listas
[ ] Capturas de respaldo disponibles
[ ] Slides/notas accesibles
[ ] Timer configurado
```

### 7.2 Flujo de la Demo

1. **Mostrar la app inicial** (1 min)
2. **Explicar el contexto** (2 min)
3. **Ejecutar el ataque** (3 min)
4. **Mostrar el payload** (2 min)
5. **Explicar contramedidas** (2 min)
6. **Q&A** (5 min)

### 7.3 Plan B si Falla Algo

#### Si no funciona Internet:

- Usa capturas de pantalla pregrabadas
- Video de la demo
- Slides explicativos

#### Si falla el portapapeles:

- Muestra el código en el editor
- Explica teóricamente el ataque
- Usa el simulador en la misma página

#### Si falla Telegram:

- Usa el navegador web directamente
- Muestra la app en modo desarrollo
- Explica con diagramas

## 🐛 Solución de Problemas Comunes

### Problema: "La Mini App no carga"

**Solución**:
```bash
# Verificar que la URL es HTTPS
# Verificar que el manifest.json es accesible
curl https://tu-dominio.com/tonconnect-manifest.json

# Verificar configuración en BotFather
/myapps → seleccionar app → Edit Web App URL
```

### Problema: "El portapapeles no se modifica"

**Solución**:
- Verificar que estás en Telegram Desktop (no web)
- Comprobar permisos del navegador
- Usar la versión HTTPS
- Verificar consola de desarrollador

```javascript
// En la consola del navegador
navigator.clipboard.writeText("test").then(
  () => console.log("✅ Clipboard works"),
  (err) => console.error("❌ Error:", err)
);
```

### Problema: "No aparece el popup de Telegram"

**Solución**:
```javascript
// Verificar que el SDK está inicializado
import { showPopup } from '@tma.js/sdk-vue';

// Verificar en la consola
console.log(window.Telegram);
```

### Problema: "Build falla"

**Solución**:
```bash
# Limpiar cache
rm -rf node_modules dist .vite
pnpm install
pnpm run build

# Verificar versiones
node --version  # debe ser v18+
pnpm --version  # debe ser v8+
```

## 📊 Monitoreo Durante la Demo

### Métricas a Observar:

1. **Latencia de la app**: Debe cargar en < 2 segundos
2. **Funcionalidad del portapapeles**: Verificar en cada demo
3. **Estabilidad de Telegram**: No debe crashear
4. **Conectividad**: Internet estable

### Logging y Debugging:

```javascript
// Añade logging temporal para la demo
console.log('🔍 Demo mode: ON');

// En ClipboardDemoPage.vue
async function handleValidation() {
  console.log('🎯 Validation clicked');
  try {
    await navigator.clipboard.writeText(maliciousPayload);
    console.log('✅ Clipboard modified:', maliciousPayload);
  } catch (error) {
    console.error('❌ Clipboard error:', error);
  }
}
```

## 🎬 Post-Demo

### Después de la Presentación:

1. **Agradecer a la audiencia**
2. **Compartir el repositorio**
3. **Responder preguntas adicionales**
4. **Recopilar feedback**

### Compartir Recursos:

```
🔗 Repositorio: https://github.com/[tu-usuario]/demo_morteruelocon_2026
📚 Documentación: https://github.com/[tu-usuario]/demo_morteruelocon_2026/blob/main/README.md
🛡️ Guía Ética: https://github.com/[tu-usuario]/demo_morteruelocon_2026/blob/main/ETICA_Y_SEGURIDAD.md
🎤 Slides: [URL si las subes]
```

### Métricas Post-Demo:

- Número de asistentes
- Preguntas recibidas
- Stars en GitHub
- Menciones en redes sociales
- Contactos establecidos

## 📞 Soporte

Si tienes problemas durante el despliegue:

1. Revisa los logs: `pnpm run build --debug`
2. Consulta la documentación de Telegram: https://docs.telegram-mini-apps.com/
3. Abre un issue en el repositorio
4. Contacta al autor: [TU_EMAIL]

---

**¡Mucha suerte con tu demo en la MorterueloCon 2026!** 🎉🔒

Recuerda: La preparación es clave para una demo exitosa. Practica varias veces antes del evento.
