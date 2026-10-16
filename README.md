# H2O Lerma con Encanto — Sitio web

### Proyecto en desarrollo (WIP)
> [!WARNING]
> El sitio **todavía no está desplegado ni es funcional**. Este repositorio se encuentra
> en fase de cimientos: el modelo de datos y la arquitectura están definidos y
> documentados; la implementación va comenzando.
>
> Si llegaste aquí buscando información de la asociación, los canales oficiales están en
> [Contacto](#contacto).

---

## ¿Qué es esto?

**H2O Lerma con Encanto** es una asociación civil mexicana dedicada a la biorremediación
del río Lerma utilizando cascarón de huevo como biofiltro. Este repositorio contiene
el código de su página web institucional, comunitaria y operativa.

La página no es sólo un catálogo de centros de acopio. Sus objetivos son:

- **Generar comunidad** alrededor del proyecto.
- **Comunicar el sustento científico** que lo respalda.
- **Acercar** voluntarios, donadores e industrias.
- **Servir como panel de administración** operable por personas sin conocimientos técnicos.

---

## Estado del proyecto

> [!CAUTION]
> La planeación del proyecto está en proceso de reescribirse. Todos los sprints de abajo
> están deprecados. Cuando quede listo se actualizará el documento.

| Fase | Contenido | Estado |
|---|---|---|
| **Planeación** | Requerimientos, modelo de datos, roles y permisos, mapeo de sitio | 🔄 En replanteamiento |
| **Catálogo (fase paralela)** | Sincronización Excel Online ↔ MongoDB Atlas | 📋 Planeada y documentada aparte |
| **Sprint 0** | Cimientos: repo, MERN base, JWT, despliegue de preview, GA4 | 🚧 En curso |
| **Sprint 1** | Identidad institucional (Home, Sobre Nosotros, Historia, Sustento Científico) | ⬜ Pendiente |
| **Sprint 2** | Contenido educativo y formularios | ⬜ Pendiente |
| **Sprint 3** | Directorio interactivo de centros con mapa y fotos | ⬜ Pendiente |
| **Sprint 4** | Comunidad: cuentas, eventos, comentarios, valoraciones, entregas | ⬜ Pendiente |
| **Sprint 5** | Panel administrativo, estadísticas y gamificación | ⬜ Pendiente |
| **Sprint 6** | Cierre, testing, documentación de entrega y transferencia a H2O | ⬜ Pendiente |

El proyecto nació como un **servicio social** (septiembre 2025 – mayo 2026) y hoy lo
continúan personas voluntarias. Está pensado para entregarse a la asociación y para que
otras personas puedan retomarlo, por lo que la claridad del código y de la documentación
son un requisito. Esto aplica para cualquier modificación nueva.

---

## Stack

| Capa | Tecnología |
|---|---|
| Base de datos | MongoDB Atlas (NoSQL) + Mongoose |
| Backend | Node.js + Express 5 (API REST) |
| Frontend | React 19 + Vite, React Router, Axios |
| Autenticación | JWT + bcrypt |
| Subida de archivos | Multer → Cloudinary |

### Servicios externos

| Servicio | Para qué | Estado |
|---|---|---|
| **Cloudinary** | Imágenes de centros, eventos, perfiles y evidencias de entregas. En MongoDB solo se guardan URLs | Confirmado |
| **Brevo** | Correos transaccionales y notificaciones masivas | Sugerido, sin confirmar |
| **Google Analytics 4** | Métricas de uso | Confirmado |
| **Hosting** | Despliegue | ⚠️ Por definir |
| **Dominio** | — | ⚠️ Por definir |

> [!IMPORTANT]
> **Política de cuentas:** todo servicio de infraestructura (MongoDB, Cloudinary, Brevo,
> Azure, dominio) se registra con el **correo institucional de H2O**, nunca con cuentas
> personales, para que la transferencia a la asociación sea limpia. Las herramientas de
> colaboración con transferencia nativa (GitHub, Figma) pueden vivir en cuenta personal
> temporalmente.

---

## Estructura del repositorio

```
/src                    Backend — Node.js + Express
  /config               Conexión a MongoDB, Cloudinary
  /models               Schemas de Mongoose (12, uno por entidad)
  /routes               Definición de endpoints REST
  /controllers          Lógica de cada endpoint
  /middleware           Auth (JWT), tokens de cuenta, auditoría
  /templates            Plantillas de correo (pendiente)
  /test                 Pruebas (pendiente)
  server.js
  .env.example          Plantilla de variables de entorno

/client                 Frontend — React + Vite
  /public
  /src
  index.html
  vite.config.js
```

### Convención de nombres en el backend

Hay dos capas con convenciones distintas y es correcto que no coincidan:

| Capa | Convención | Ejemplo |
|---|---|---|
| Archivos y modelos de Mongoose | `PascalCase`, **singular** | `models/Centro.js` → modelo `Centro` |
| Colecciones en MongoDB | `snake_case`, **plural** | colección `centros` |

Mongoose pluraliza automáticamente el nombre del modelo para derivar la colección
(`Centro` → `centros`, `Formulario_respuesta` → `formulario_respuestas`). No hay que
declararlo a mano ni "corregir" el singular de los archivos.

Las convenciones completas de código están en
[`CONTRIBUTING.md`](./CONTRIBUTING.md#3-convenciones-de-nombres).

---

## Instalación y puesta en marcha

> [!WARNING]
> 🚧 El proyecto está en Sprint 0. Los comandos de abajo funcionan, pero todavía no hay
> una aplicación que ver: faltan el seed de datos de prueba y buena parte de las rutas.

```bash
git clone https://github.com/h2o-lerma-con-encanto/H2O-pagina.git
cd H2O-pagina

# Backend
cd src
npm install
cp .env.example .env     # llenar con credenciales propias, ver tabla de abajo
npm run dev              # nodemon

# Frontend (en otra terminal)
cd client
npm install
npm run dev              # vite
```

### Variables de entorno (`src/.env`)

| Variable | Qué es | Dónde se consigue |
|---|---|---|
| `PORT` | Puerto del servidor | Tú lo eliges (ej. `5000`) |
| `MONGO_URI` | Cadena de conexión a Atlas | Panel de MongoDB Atlas → Connect |
| `JWT_SECRET` | Clave para firmar tokens | Genérala: `openssl rand -base64 32` |
| `TOKEN_EXPIRES_IN` | Duración de sesión | `3h` (definido en RNF-04) |
| `CLOUDINARY_URL` | Credencial completa de Cloudinary | Dashboard de Cloudinary |
| `CLOUDINARY_CLOUD_NAME` | Nombre del cloud | Dashboard de Cloudinary |
| `CLOUDINARY_API_KEY` | API key | Dashboard de Cloudinary |
| `CLOUDINARY_API_SECRET` | API secret | Dashboard de Cloudinary |
| `NODE_ENV` | Entorno | `development`, `test` o `production` |

**No necesitas las credenciales de producción de H2O para desarrollar.** Crea tus propias
cuentas gratuitas de MongoDB Atlas y Cloudinary y trabaja contra tu propio cluster. Las credenciales
institucionales solo se usan en despliegue y no se comparten.

> [!CAUTION]
> ⚠️ El archivo `.env` está correctamente ignorado por Git. **NUNCA** lo subas, y nunca
> quites esa regla del `.gitignore`.

### Pruebas

```bash
cd src && npm test       # jest
```

> [!WARNING]
> La carpeta `src/test/` está vacía por ahora.

---

## Documentación

Este repositorio no es la única fuente de verdad. **Esta tabla es el índice único de la
documentación del proyecto**: si algo cambia de lugar, se actualiza aquí y los demás
archivos enlazan a esta sección.

> [!CAUTION]
> **Pendiente de decidir:** hoy la documentación de planeación vive **fuera del
> repositorio**, en la carpeta de trabajo que lo contiene. Quien clone el repo NO la
> recibe. Se decidirá si se mueve a `/docs` dentro del repo. Ver
> [Decisiones pendientes](#decisiones-pendientes-que-bloquean-avance).

**Dentro del repositorio:**

| Documento | Qué contiene | Estado |
|---|---|---|
| [`CONTRIBUTING.md`](./CONTRIBUTING.md) | Cómo contribuir: entorno local, convenciones, ramas y PRs | ✅ Vigente |
| [`MAINTAINERS.md`](./MAINTAINERS.md) | Quién mantiene el proyecto y cómo contactarle | ✅ Vigente |
| [`LICENSE`](./LICENSE) | Licencia MIT | ✅ Vigente |

**Fuera del repositorio** (carpeta de trabajo del proyecto):

| Documento | Qué contiene | Estado |
|---|---|---|
| `H2O_Documentacion.docx` | Requerimientos, entidades y relaciones, atributos de cada colección, permisos CRUD por rol, mapeo completo del sitio (70 rutas) | ⚠️ Deprecado — referencia histórica |
| `Sprints_H2O.docx` | Planeación de sprints original | ⚠️ Deprecado — en reescritura |
| `Entidades_H2O.md` | Atributos detallados de cada entidad | 🔄 En revisión |
| `arquitectura-db.svg` | Diagrama entidad–relación de la base de datos | 🔄 En revisión |
| `mapeo-sitio.svg` | Diagrama de las 70 rutas del sitio | 🔄 En revisión |
| Manual del panel administrativo | Paso a paso para personas no técnicas | ⬜ Pendiente de entrega |

### Conceptos clave para entender el código

- **6 roles con permisos acumulativos**: visitante → usuario → voluntario → centro de
  acopio → prestador de servicio social → administrador. Con dos excepciones
  documentadas a la acumulación.
- **12 colecciones en MongoDB**, con dirección y horarios *embebidos* dentro de `centros`.
- **Campos en español, `snake_case` sin acentos** en base de datos y API;
  `camelCase` en variables JavaScript. Es intencional.
- **Soft delete**: los centros, usuarios y comentarios no se borran, se desactivan.

---

## Cómo contribuir

Lee [`CONTRIBUTING.md`](./CONTRIBUTING.md) antes de tu primer commit. El archivo incluye un orden de
lectura recomendado de la documentación.

> [!CAUTION]
> **NUNCA subas al repositorio** archivos `.env`, cadenas de conexión, claves de API,
> `JWT_SECRET`, ni datos personales reales de contactos de Centros de Acopio.

---

## Decisiones pendientes que bloquean avance

Estas piezas dependen de insumos de H2O y externos, no del código:

- [ ] **Gamificación**: nombres de niveles e insignias, puntos por kilo de cascarón,
      qué acciones otorgan experiencia
- [ ] **Formularios**: preguntas concretas de los cuatro formularios (centro de acopio,
      voluntariado, servicio social, contacto general)
- [ ] **Hosting y dominio**: plataforma sin elegir, dominio sin comprar
- [ ] **Textos institucionales**: misión, visión, historia, FAQ, sustento científico
- [ ] **Manual de identidad**: logos, colores, tipografías, mascota
- [ ] **Documentos legales**: aviso de privacidad (derechos ARCO), términos y condiciones,
      política de cookies — idealmente con asesoría legal
- [ ] **Servicio de correos**: confirmar Brevo o elegir alternativa

### Pendientes de documentación

- [ ] **`foto_perfil_url` falta en la entidad `usuarios`.** El campo se usa en la política
      de medios de Cloudinary y en la definición del perfil público, pero no aparece en la
      lista de atributos de `usuarios` ni en el documento de requerimientos. Agregarlo a
      ambos.
- [ ] **Ubicación de la documentación**: decidir si se mueve dentro del repositorio. Hoy
      queda fuera y no llega a quien clone.
- [ ] **Estandarizar nombres de archivo del backend**: hay dos inconsistencias menores en
      `src/` (un controlador con sufijo duplicado y un par controlador/ruta que no
      coinciden en singular/plural).
- [ ] **Programar tests**: por el momento está pendiente.

---

## Contacto

Para temas de **código**: abre un issue en este repositorio, o consulta
[`MAINTAINERS.md`](./MAINTAINERS.md).

Para temas de la **asociación** — colaboraciones, donaciones, ser Centro de Acopio,
voluntariado:

- **Correo:** [colectivoh2olerma@gmail.com](mailto:colectivoh2olerma@gmail.com)
- **Sitio web:** _en construcción_
- **Redes sociales:** [Página de Facebook](https://www.facebook.com/H2OLerma), [Instagram](https://www.instagram.com/h2o.lerma/)

---

## Licencia

Licencia **MIT** — ver [`LICENSE`](./LICENSE). El copyright corresponde a la asociación
civil H2O Lerma con Encanto.

---

<sub>Desarrollado inicialmente como proyecto de servicio social, septiembre 2025 – mayo 2026, y continuado de forma voluntaria desde entonces. La documentación es parte del entregable: si cambias el código, actualiza también el documento correspondiente.</sub>
