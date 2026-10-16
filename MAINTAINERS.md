# Mantenimiento del proyecto

Este archivo existe para que cualquier persona que retome el proyecto sepa **a quién
preguntarle**. Si vas a heredar el mantenimiento, agrega tu fila a la tabla antes de
empezar y marca la fecha de fin de quien te precede.

> Este proyecto nació como un **servicio social** para H2O Lerma con Encanto y desde
> entonces lo sostienen personas que entran y salen: prestadores de servicio social y
> voluntarios. La tabla de más abajo es el historial: nunca borres filas, solo agrega.

---

## Responsable actual

| | |
|---|---|
| **Nombre** | DHH |
| **GitHub** | [@CikiUmi](https://github.com/CikiUmi) |
| **Contacto** | Abre un issue en este repositorio |
| **Periodo** | Septiembre 2025 – actualidad |
| **Vínculo** | Servicio social (sep 2025 – may 2026), actualmente **voluntaria** |
| **Alcance** | Arquitectura, base de datos, backend y frontend |

**Qué puedo responder:** decisiones de diseño de la base de datos, por qué los permisos
CRUD están repartidos así entre los seis roles, cómo se conecta el Excel Online con
MongoDB Atlas, y el razonamiento detrás del mapeo de sitio.

**Qué NO puedo responder:** credenciales de servicios externos, contenido institucional
(textos, fotos, datos del proyecto) ni decisiones operativas de la asociación. Eso le
corresponde a H2O.

---

## Historial de responsables

| Periodo | Persona | GitHub | Vínculo | Fase del proyecto |
|---|---|---|---|---|
| Sep 2025 – May 2026 | DHH | [@CikiUmi](https://github.com/CikiUmi) | Servicio social | Catálogo en Excel, modelado de datos y desarrollo inicial |
| Oct 2026 – actualidad | DHH | [@CikiUmi](https://github.com/CikiUmi) | Voluntaria | Replanteamiento del alcance y desarrollo por incrementos |

> **Nota sobre el historial de commits:** los commits de la fase inicial se hicieron desde
> la cuenta institucional de H2O, que es también la dueña del repositorio y de todos los
> servicios externos. No están mal atribuidos: se hicieron así a propósito para que el
> código perteneciera a la asociación desde el principio. La tabla de arriba es la
> referencia real de quién trabajó en qué.

---

## Contacto institucional

Para temas que no son de código — colaboraciones, donaciones, ser Centro de Acopio,
voluntariado — el canal correcto es H2O Lerma con Encanto, no el repositorio.

| | |
|---|---|
| **Correo** | [colectivoh2olerma@gmail.com](mailto:colectivoh2olerma@gmail.com) |
| **Sitio web** | _en construcción_ |
| **Facebook** | [H2OLerma](https://www.facebook.com/H2OLerma) |
| **Instagram** | [@h2o.lerma](https://www.instagram.com/h2o.lerma/) |

---

## Dónde vive el resto de la documentación

El repositorio **no** es la única fuente de verdad. El índice completo y actualizado de
la documentación está en el [README](./README.md#documentación) — se mantiene en un solo
lugar a propósito, para que no haya tres listas que se contradigan.

> [!CAUTION]
> La planeación del proyecto está en proceso de reescribirse. Los documentos de
> requerimientos y sprints anteriores están deprecados; el índice del README indica el
> estado de cada uno.

Si vas a cambiar el modelo de datos, los permisos de un rol o las rutas del sitio,
**actualiza también la documentación correspondiente**. Un cambio en el código que no se
refleja ahí es un cambio que la siguiente persona no va a entender.
