# Cómo contribuir

¡Hola! Este documento es para cualquier persona que vaya a tocar el código de la
página de **H2O Lerma con Encanto**: prestadores de servicio social, voluntarios con perfil
técnico, o quien retome el proyecto más adelante.

Está escrito asumiendo que **no conoces el proyecto todavía**. Por favor lee la sección 1
antes de escribir código.

> ¿Tienes dudas que este documento no resuelve? En [`MAINTAINERS.md`](./MAINTAINERS.md)
> están las personas a las que preguntarle.

---

## 1. Antes de escribir una sola línea

Este proyecto tiene mucha más documentación que código, y es a propósito.

El **índice de toda la documentación** está en el
[README](./README.md#documentación). Empieza ahí: ese índice es el único lugar donde se
mantiene la lista, para que no haya varias versiones contradiciéndose.

> [!CAUTION]
> La planeación del proyecto está en proceso de reescribirse. El documento de
> requerimientos y la planeación de sprints anteriores están **deprecados**: sirven como
> referencia histórica, no como instrucción.

Orden de lectura recomendado del documento de requerimientos:

1. **Secciones 1 y 2** — qué es H2O y qué entra (y qué NO entra) en el alcance. Evita que
   implementes algo que ya se decidió dejar fuera.
2. **Sección 6: Datos, Entidades y Relaciones** — la más importante. Toda la lógica de la
   página sale de ahí. Si vas a tocar el backend, esto no es opcional.
3. **Sección 6.4: Permisos CRUD por rol** — son seis roles con permisos acumulativos. Casi
   cualquier bug de seguridad en este proyecto va a nacer de no haber leído esta tabla.
4. **Sección 7: Mapeo de sitio** — las 70 rutas y qué rol mínimo accede a cada una.

**Regla general:** si lo que vas a hacer contradice la documentación, no lo hagas todavía.
Primero platícalo ([ver sección 7](#7-reportar-problemas)), decidan, y **actualicen el
documento**. El código y la documentación se mueven juntos o el proyecto se muere en la
siguiente entrega.

---

## 2. Requisitos y entorno local

| Herramienta | Versión | Para qué |
|---|---|---|
| Node.js | 20 LTS o superior | Entorno de ejecución |
| npm | El que venga con Node | Gestor de paquetes |
| Git | Cualquiera reciente | Control de versiones |
| Cuenta de MongoDB Atlas | — | Base de datos (hay free tier) |
| Cuenta de Cloudinary | — | Fotos de Centros de Acopio (hay free tier) |

> Los comandos de instalación y la lista completa de variables de entorno están en el [README](./README.md#instalación-y-puesta-en-marcha).

---

## 3. Convenciones de nombres

Hay dos convenciones distintas conviviendo y **es intencional**. No las unifiques.

### Base de datos (MongoDB)

Definido en la sección 6.3 del documento de requerimientos:

- `snake_case` en minúsculas: `centro_id`, `fecha_creacion`, `costales_actuales_kg`
- **Sin acentos, sin ñ, sin caracteres especiales**
- Las referencias a otras entidades terminan en `_id`
- Los booleanos son estados (`activo`, `oculto`, `atendido`) o flags (`notificar_usuarios`)
- Las fechas usan el tipo `Date` de MongoDB

```js
// Bien :D
{ centro_id: ObjectId, costales_actuales_kg: 12.5, activo: true }

// Mal X
{ centroId: ObjectId, costalesActualesKg: 12.5, está_activo: true }
```

> Los **archivos y modelos de Mongoose** sí van en `PascalCase` singular (`models/Centro.js`),
> mientras que las **colecciones** quedan en `snake_case` plural (`centros`). No es un error:
> Mongoose pluraliza solo. Está explicado en el
> [README](./README.md#convención-de-nombres-en-el-backend).

### Código JavaScript

- Variables y funciones: `camelCase`
- Componentes de React: `PascalCase` (`CentroCard.jsx`)
- Constantes: `UPPER_SNAKE_CASE`
- Archivos de componentes: `PascalCase.jsx`; el resto, `camelCase.js`

La traducción entre ambos mundos se hace en la capa de modelos/controladores, no
esparcida por todo el código.

### Idioma

**Todo en español**: nombres de variables, comentarios, mensajes de commit, rutas de la
API. La gente que va a heredar esto son estudiantes de servicio social y colaboradores en
México. Las excepciones son las palabras reservadas del lenguaje y los términos técnicos
que no tienen traducción razonable (`token`, `endpoint`, `hash`).

---

## 4. Ramas y commits

### Ramas

- `main` — siempre desplegable. **No se hace push directo, pasa por una pipeline (pendiente de desarrollar).**
- `desarrollo` — integración. De aquí salen las ramas de trabajo.
- `tipo/descripcion-corta` — tu rama de trabajo.

```
feat/mapa-centros-acopio
fix/validacion-horarios
docs/manual-panel-admin
refactor/permisos-middleware
```

### Mensajes de commit

Usamos [Conventional Commits](https://www.conventionalcommits.org/es/). El formato:

```
tipo(ámbito): descripción en imperativo y minúscula

Cuerpo opcional explicando el POR QUÉ, no el qué.
El qué ya se ve en el diff.
```

| Tipo | Cuándo |
|---|---|
| `feat` | Funcionalidad nueva |
| `fix` | Corrección de un error |
| `docs` | Solo documentación |
| `style` | Formato, sin cambio de lógica |
| `refactor` | Reestructurar sin cambiar comportamiento |
| `test` | Pruebas |
| `chore` | Dependencias, configuración, tareas de mantenimiento |

```
feat(centros): agregar filtro por municipio en el directorio

Los usuarios de zonas con muchos centros no podían acotar la búsqueda.
Implementa RF-05 del documento de requerimientos.
```

**Referencia los IDs de requerimiento** (`RF-05`, `RNF-04`) cuando apliquen. Es la forma
más rápida de que alguien entienda qué decisión estabas implementando.

### Configura tu identidad al clonar

Para que tus commits queden atribuidos a ti y no a la cuenta institucional:

```bash
git config user.name "Tu Nombre"
git config user.email "tu-correo@ejemplo.com"
```

Si quieres que GitHub te enlace sin exponer tu correo personal, usa tu dirección `noreply`
(Settings → Emails → *Keep my email addresses private*):

```bash
git config user.email "TU_ID+tu-usuario@users.noreply.github.com"
```

---

## 5. Pull Requests

1. Crea tu rama desde `desarrollo`.
2. Haz commits pequeños y con sentido. Un commit gigante de "avances" no le sirve a nadie.
3. Abre el Pull Request **hacia `desarrollo`**, no hacia `main`.
4. En la descripción, responde: **qué cambia**, **por qué**, y **qué requerimiento
   implementa** si se trabaja sobre uno.
5. Si tocaste el modelo de datos, los permisos o las rutas, di explícitamente si ya
   actualizaste la documentación.

### Antes de pedir revisión

- [ ] El proyecto levanta sin errores en local
- [ ] No hay `console.log` de depuración olvidados
- [ ] No hay credenciales, tokens ni `.env` en el diff
- [ ] Los nombres de campos nuevos siguen la convención de la [sección 3](#3-convenciones-de-nombres)
- [ ] Si el endpoint es nuevo, valida permisos según la tabla de la **sección 6.4 del documento de requerimientos**
- [ ] Actualizaste la documentación si hacía falta

---

## 6. Lo que NUNCA va al repositorio

> [!WARNING]
> Esto no es una recomendación, es la regla que puede quemar las cuentas de la asociación.

- Archivos `.env` de cualquier tipo
- Cadenas de conexión de MongoDB Atlas
- Claves de API de Cloudinary, Brevo, Google Analytics
- `JWT_SECRET`
- Datos personales reales de contactos de Centros de Acopio (nombres, teléfonos, correos)
  en datos de prueba o capturas de pantalla
- Exportaciones completas de la base de datos

> [!CAUTION]
> Usa `.env.example` con los nombres de las variables y valores vacíos o falsos. Si por
> accidente subiste un secreto, **no basta con borrarlo en el siguiente commit**: sigue en el
> historial. Hay que rotar la credencial de inmediato y avisarle a quien mantiene el proyecto.

---

## 7. Reportar problemas

Usa los **Issues** del repositorio. Incluye:

- Qué esperabas que pasara y qué pasó
- Pasos para reproducirlo
- Rol de usuario con el que estabas probando
- Capturas o mensajes de error, con los datos sensibles tapados

Para proponer cambios de alcance o funcionalidades nuevas, revisa primero las
[decisiones pendientes del README](./README.md#decisiones-pendientes-que-bloquean-avance):
varias cosas que parecen faltantes son decisiones deliberadas o están esperando insumos
de H2O.

---

Gracias por trabajar en esto. El proyecto está pensado para durar más que cualquiera de
las personas que pasan por él, cuidar la documentación es parte del trabajo colectivo :D.
