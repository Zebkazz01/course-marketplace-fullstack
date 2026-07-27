# Courses Online — Marketplace de Cursos en Línea

Plataforma web de marketplace para la compra y venta de cursos en línea, construida con **Angular 15**. Incluye catálogo de cursos, carrito de compras, checkout con PayPal, panel de estudiante y autenticación JWT.

---

## Índice

1. [Descripción del Proyecto](#descripción-del-proyecto)
2. [Características Principales](#características-principales)
3. [Tecnologías Utilizadas](#tecnologías-utilizadas)
4. [Arquitectura del Proyecto](#arquitectura-del-proyecto)
5. [Estructura de Directorios](#estructura-de-directorios)
6. [Módulos y Rutas](#módulos-y-rutas)
7. [Conexión con el API REST](#conexión-con-el-api-rest)
8. [Autenticación y Autorización](#autenticación-y-autorización)
9. [Instalación y Configuración](#instalación-y-configuración)
10. [Ejecución del Proyecto](#ejecución-del-proyecto)
11. [Construcción para Producción](#construcción-para-producción)
12. [Pruebas](#pruebas)
13. [Variables de Entorno](#variables-de-entorno)
14. [Contribuciones](#contribuciones)
15. [Licencia](#licencia)

---

## Descripción del Proyecto

**Courses Online** es una aplicación web diseñada para facilitar la compra y venta de cursos en línea. Los usuarios pueden explorar un catálogo de cursos, buscar por categoría, instructor, nivel e idioma, agregar cursos a un carrito de compras y proceder al pago mediante PayPal. Los estudiantes registrados pueden acceder a un panel personalizado donde visualizan sus cursos adquiridos, hacen seguimiento de su progreso y dejan reseñas.

La aplicación está construida con una arquitectura **modular y basada en componentes**, separando claramente las funcionalidades para usuarios invitados, usuarios autenticados y la administración de autenticación. La comunicación con el backend se realiza mediante una **API REST** sobre HTTP con autenticación basada en tokens JWT.

---

## Características Principales

| Funcionalidad | Descripción |
|---|---|
| **Catálogo de Cursos** | Navegación por cursos organizados por categorías, banners promocionales y secciones destacadas. |
| **Búsqueda y Filtrado** | Búsqueda avanzada por nombre, categoría, instructor, nivel, idioma, rango de precios y calificación. |
| **Landing de Curso** | Página detallada de cada curso con descripción, instructor, reseñas y botón de agregar al carrito. |
| **Carrito de Compras** | Gestión del carrito: agregar, eliminar, aplicar cupones y calcular totales con descuentos. |
| **Checkout con PayPal** | Integración con PayPal SDK para procesar pagos de forma segura. |
| **Panel de Estudiante** | Dashboard con cursos matriculados, ventas, perfil editable y reseñas de cursos. |
| **Autenticación JWT** | Registro e inicio de sesión con tokens JWT almacenados en `localStorage`, con verificación de expiración. |
| **Autorización de Rutas** | Guardia de autenticación (`AuthGuard`) que protege las rutas exclusivas de usuarios autenticados. |
| **Notificaciones** | Mensajes toast en tiempo real mediante `ngx-toast-notifications`. |

---

## Tecnologías Utilizadas

### Frontend

| Tecnología | Versión | Propósito |
|---|---|---|
| **Angular** | 15.1.x | Framework principal de desarrollo web |
| **TypeScript** | ~4.9.4 | Lenguaje de programación tipado |
| **RxJS** | ~7.8.0 | Programación reactiva y manejo de observables |
| **Angular Router** | 15.1.x | Enrutamiento y navegación SPA |
| **Angular Forms** | 15.1.x | Formularios reactivos y basados en plantillas |
| **Angular HttpClient** | 15.1.x | Comunicación HTTP con el backend |
| **ngx-toast-notifications** | ^1.3.0 | Sistema de notificaciones toast |
| **Jasmine** | ~4.5.0 | Framework de pruebas unitarias |
| **Karma** | ~6.4.0 | Test runner para pruebas unitarias |

### Estilos y UI

| Tecnología | Propósito |
|---|---|
| **Bootstrap** | Framework CSS (cargado como asset externo) |
| **jQuery** | Manipulación DOM y plugins legacy |
| **SweetAlert2** | Diálogos y alertas personalizadas |
| **Swiper.js** | Sliders y carruseles |
| **Magnify Popup** | Modales y ventanas emergentes |
| **Font Awesome** | Iconografía |
| **Feather Icons** | Iconografía ligera |
| **Sal.js** | Animaciones de desplazamiento |
| **Odometer.js** | Contadores animados |

### Integraciones Externas

| Servicio | Propósito |
|---|---|
| **PayPal SDK** | Procesamiento de pagos en el carrito de compras |

### Herramientas de Desarrollo

| Herramienta | Propósito |
|---|---|
| **Angular CLI** | ~15.1.6 — Generación de código, construcción y despliegue |
| **Angular DevKit** | ^15.1.6 — Herramientas de compilación y desarrollo |

---

## Arquitectura del Proyecto

La aplicación sigue una arquitectura **modular basada en componentes** con los siguientes principios:

- **Módulos lazy-loaded**: Cada módulo de funcionalidad se carga bajo demanda mediante `loadChildren`.
- **Servicios singleton**: Los servicios (`@Injectable({ providedIn: 'root' })`) manejan la lógica de negocio y la comunicación con el API REST.
- **Comunicación reactiva**: Se utiliza `BehaviorSubject` y observables de RxJS para el estado del carrito de compras.
- **Separación de responsabilidades**: Componentes presentacionales, servicios de datos y guardias de ruta separados claramente.
- **Shared Module**: Componentes reutilizables (Header, Footer) exportados para uso en todos los módulos.

---

## Estructura de Directorios

```
course-marketplace-fullstack/
├── src/
│   ├── app/
│   │   ├── app.component.ts          # Componente raíz
│   │   ├── app.module.ts             # Módulo principal
│   │   ├── app-routing.module.ts     # Rutas principales (lazy-loading)
│   │   ├── config/
│   │   │   └── config.ts             # Configuración de URLs del backend
│   │   ├── shared/
│   │   │   ├── shared.module.ts      # Módulo compartido
│   │   │   ├── header/               # Componente Header
│   │   │   └── footer/               # Componente Footer
│   │   └── modules/
│   │       ├── auth/                 # Módulo de autenticación
│   │       │   ├── auth.module.ts
│   │       │   ├── auth-routing.module.ts
│   │       │   ├── auth.component.*
│   │       │   ├── login-and-register/
│   │       │   │   └── login-and-register.component.*
│   │       │   └── service/
│   │       │       ├── auth.service.ts       # API: login, register
│   │       │       └── auth.guard.ts         # Guardia de rutas
│   │       ├── home/                 # Módulo de página de inicio
│   │       │   ├── home.module.ts
│   │       │   ├── home-routing.module.ts
│   │       │   ├── home.component.*
│   │       │   └── service/
│   │       │       ├── home.service.ts       # API: listado home
│   │       │       └── cart.service.ts       # API: carrito CRUD
│   │       ├── tienda-guest/         # Módulo para usuarios invitados
│   │       │   ├── tienda-guest.module.ts
│   │       │   ├── tienda-guest-routing.module.ts
│   │       │   ├── tienda-guest.component.*
│   │       │   ├── landing-course/
│   │       │   │   └── landing-course.component.*
│   │       │   ├── filters-courses/
│   │       │   │   └── filters-courses.component.*
│   │       │   └── service/
│   │       │       └── tienda-guest.service.ts # API: cursos, búsqueda
│   │       └── tienda-auth/          # Módulo para usuarios autenticados
│   │           ├── tienda-auth.module.ts
│   │           ├── tienda-auth-routing.module.ts
│   │           ├── tienda-auth.component.*
│   │           ├── carts/
│   │           │   └── carts.component.*       # Carrito + PayPal
│   │           ├── student-dashboard/
│   │           │   └── student-dashboard.component.*
│   │           ├── course-leason/
│   │           │   └── course-leason.component.*
│   │           └── service/
│   │               └── tienda-auth.service.ts  # API: checkout, perfil, reseñas
│   ├── environments/
│   │   ├── environment.ts             # Configuración producción
│   │   └── environment.development.ts  # Configuración desarrollo
│   ├── assets/                        # Recursos estáticos (CSS, JS, imágenes)
│   ├── index.html                     # Punto de entrada HTML
│   ├── main.ts                        # Bootstrap de Angular
│   └── styles.css                     # Estilos globales
├── angular.json                       # Configuración del CLI de Angular
├── package.json                       # Dependencias y scripts
├── tsconfig.json                      # Configuración de TypeScript
├── tsconfig.app.json                  # Configuración TS para la app
├── tsconfig.spec.json                 # Configuración TS para pruebas
├── .gitignore
└── README.md
```

---

## Módulos y Rutas

La aplicación utiliza **enrutamiento perezoso (lazy-loading)** con `loadChildren`. Las rutas principales están definidas en `app-routing.module.ts`:

### Rutas de la Aplicación

| Ruta | Módulo | Componente | Autenticación | Descripción |
|---|---|---|---|---|
| `/` | HomeModule | `HomeComponent` | No | Página de inicio con catálogo de cursos |
| `/landing-curso/:slug` | TiendaGuestModule | `LandingCourseComponent` | No | Página de detalle de un curso |
| `/filtros-de-cursos` | TiendaGuestModule | `FiltersCoursesComponent` | No | Búsqueda y filtrado de cursos |
| `/carrito-de-compra` | TiendaAuthModule | `CartsComponent` | Sí (AuthGuard) | Carrito de compras con checkout PayPal |
| `/perfil-del-estudiante` | TiendaAuthModule | `StudentDashboardComponent` | Sí (AuthGuard) | Panel de estudiante (perfil, cursos, reseñas) |
| `/ver-curso/:slug` | TiendaAuthModule | `CourseLeasonComponent` | Sí (AuthGuard) | Visualización de clases del curso |
| `/auth/login` | AuthModule | `LoginAndRegisterComponent` | No | Registro e inicio de sesión |
| `**` | — | — | — | Redirección a error 404 |

### Flujo de Navegación

```
Usuario Invitado
    ├── / (Home) → Navega cursos, categorías, banners
    ├── /landing-curso/:slug → Detalle del curso
    ├── /filtros-de-cursos → Búsqueda avanzada
    └── /auth/login → Registro/Login

Usuario Autenticado
    ├── Todas las rutas anteriores
    ├── /carrito-de-compra → Carrito + PayPal Checkout
    ├── /perfil-del-estudiante → Dashboard, perfil, reseñas
    └── /ver-curso/:slug → Clases del curso, progreso
```

---

## Conexión con el API REST

La aplicación se comunica con un backend Node.js/Express mediante una API REST. La URL base se configura en los archivos de entorno y se importa a través de `src/app/config/config.ts`.

### Configuración de URLs

```typescript
// src/app/config/config.ts
import { environment } from '../../environments/environment.development';

export const URL_BACKEND = environment.URL_BACKEND;     // URL base del backend
export const URL_SERVICIOS = environment.URL_SERVICIOS; // URL base de la API (/api)
export const URL_FROTEND = environment.URL_FROTEND;     // URL del frontend
```

### Servicios y Endpoints

#### AuthService — Autenticación

| Método | Endpoint | Autenticación | Descripción |
|---|---|---|---|
| `POST` | `/users/login_tienda` | No | Iniciar sesión con email y password. Devuelve token JWT y datos del usuario. |
| `POST` | `/users/register` | No | Registrar un nuevo usuario (rol: cliente). |

**Cabeceras**: Ninguna (login/register) / `token` (operaciones autenticadas)

#### HomeService — Datos de Inicio

| Método | Endpoint | Autenticación | Descripción |
|---|---|---|---|
| `GET` | `/home/list?TIME_NOW={timestamp}` | No | Obtiene categorías, cursos destacados, banners, secciones y ofertas flash. |

#### CartService — Carrito de Compras

| Método | Endpoint | Autenticación | Descripción |
|---|---|---|---|
| `GET` | `/cart/list` | Sí (token) | Lista los ítems del carrito del usuario. |
| `POST` | `/cart/register` | Sí (token) | Agrega un curso al carrito. |
| `DELETE` | `/cart/remove/{cart_id}` | Sí (token) | Elimina un ítem del carrito. |
| `POST` | `/cart/update` | Sí (token) | Aplica un cupón de descuento al carrito. |

#### TiendaGuestService — Catálogo para Invitados

| Método | Endpoint | Autenticación | Descripción |
|---|---|---|---|
| `GET` | `/home/landing-curso/{slug}?TIME_NOW={timestamp}&CAMPAING_SPECIAL={campaing}` | Opcional (token) | Obtiene detalles completos de un curso (instructor, categorías, reseñas). |
| `GET` | `/home/config-all` | No | Obtiene configuración: categorías, instructores, niveles e idiomas. |
| `POST` | `/home/search-course?TIME_NOW={timestamp}` | No | Busca y filtra cursos por múltiples criterios. |

#### TiendaAuthService — Funcionalidades Autenticadas

| Método | Endpoint | Autenticación | Descripción |
|---|---|---|---|
| `POST` | `/checkout/register` | Sí (token) | Registra una orden de compra (integración con PayPal). |
| `GET` | `/profile/client` | Sí (token) | Obtiene el perfil del estudiante y sus cursos. |
| `POST` | `/profile/update` | Sí (token) | Actualiza los datos del perfil (FormData con avatar). |
| `POST` | `/profile/review-register` | Sí (token) | Registra una reseña para un curso. |
| `POST` | `/profile/review-update` | Sí (token) | Actualiza una reseña existente. |
| `GET` | `/profile/course/{slug}` | Sí (token) | Obtiene el contenido del curso y progreso del estudiante. |
| `POST` | `/profile/course-student` | Sí (token) | Actualiza el progreso de clases vistas. |

### Envío de Headers de Autenticación

Todos los endpoints que requieren autenticación envían el token JWT en el encabezado HTTP `token`:

```typescript
let headers = new HttpHeaders({ 'token': this.authService.token });
return this.http.get(URL, { headers: headers });
```

### Manejo de Respuestas de Error

La API devuelve respuestas con código `403` y un campo `message` para indicar errores de validación:

```typescript
if (resp.message == 403) {
  this.toaster.open({ text: resp.message_text, caption: 'VALIDACIÓN', type: 'danger' });
}
```

---

## Autenticación y Autorización

### Flujo de Autenticación

1. El usuario ingresa sus credenciales en `LoginAndRegisterComponent`.
2. `AuthService.login()` envía una petición `POST /users/login_tienda`.
3. La API responde con un objeto `{ USER: { token, user } }`.
4. El token JWT y los datos del usuario se almacenan en `localStorage`.
5. El usuario es redirigido a la página principal.

### Almacenamiento

```typescript
// AuthService.savelocalStorage()
localStorage.setItem("token", auth.USER.token);
localStorage.setItem("user", JSON.stringify(auth.USER.user));
```

### Verificación de Token (AuthGuard)

La guardia `AuthGuard` (`auth.guard.ts`) protege las rutas de `TiendaAuthModule`:

1. Verifica que exista un usuario en `localStorage`.
2. Verifica que exista un token.
3. Decodifica el token JWT con `atob()` y extrae el campo `exp` (expiración).
4. Si el token ha expirado, redirige al login.

```typescript
let expiration = (JSON.parse(atob(token.split(".")[1]))).exp;
if (Math.floor(new Date().getTime() / 1000) >= expiration) {
  this.authService.logout();
  return false;
}
```

### Cierre de Sesión

```typescript
// AuthService.logout()
localStorage.removeItem("token");
localStorage.removeItem("user");
setTimeout(() => {
  location.href = URL_FROTEND + "/auth/login";
}, 50);
```

---

## Instalación y Configuración

### Requisitos Previos

- **Node.js**: versión 16.x o superior
- **npm**: versión 8.x o superior
- **Angular CLI**: versión 15.1.6

### Pasos de Instalación

1. **Clonar el repositorio**:

   ```bash
   git clone https://github.com/tu-usuario/course-marketplace-fullstack.git
   cd course-marketplace-fullstack
   ```

2. **Instalar dependencias**:

   ```bash
   npm install
   ```

3. **Configurar variables de entorno** (ver [Variables de Entorno](#variables-de-entorno)):

   El proyecto incluye archivos de entorno preconfigurados. Para desarrollo local, asegúrate de que `src/environments/environment.development.ts` apunte a tu backend local.

4. **Iniciar el servidor de desarrollo**:

   ```bash
   npm start
   ```

   La aplicación se abrirá en `http://localhost:4200/`.

---

## Ejecución del Proyecto

### Desarrollo

```bash
npm start
```

- **URL**: `http://localhost:4200`
- **API Backend**: `http://127.0.0.1:3000/api` (configurable en `environment.development.ts`)
- **Hot Module Replacement**: Activado (recarga automática al guardar cambios)
- **Source Maps**: Activados para depuración

### Modo Producción

```bash
npm run build
```

- **Salida**: `dist/courses_online/`
- **Optimización**: Activada (minificación, tree-shaking, hashing de archivos)
- **Source Maps**: Desactivados

---

## Construcción para Producción

```bash
npm run build -- --configuration production
```

O simplemente:

```bash
npm run build
```

Esto generará los archivos optimizados en `dist/courses_online/`, listos para ser servidos por cualquier servidor web estático (Nginx, Apache, Firebase Hosting, Vercel, etc.).

### Optimizaciones Aplicadas

- **Minificación** de CSS y JavaScript
- **Tree-shaking** para eliminar código no utilizado
- **Hashing** de nombres de archivos para cacheo eficiente
- **Lazy-loading** de módulos para reducir el bundle inicial

---

## Pruebas

El proyecto utiliza **Jasmine** como framework de pruebas y **Karma** como test runner.

### Ejecutar Pruebas Unitarias

```bash
npm test
```

Esto abrirá el navegador y ejecutará las pruebas en modo observador (watch mode), recargando automáticamente cuando se modifiquen los archivos.

### Estructura de Pruebas

- **Archivo de configuración**: `karma.conf.js` (generado por Angular CLI)
- **Archivo de specs**: `src/app/app.component.spec.ts`
- **Configuración TypeScript**: `tsconfig.spec.json`

---

## Variables de Entorno

La configuración del backend se gestiona mediante archivos de entorno de Angular:

### Desarrollo (`src/environments/environment.development.ts`)

```typescript
export const environment = {
  URL_BACKEND: 'http://127.0.0.1:3000/',
  URL_SERVICIOS: 'http://127.0.0.1:3000/api',
  URL_FROTEND: 'http://localhost:4200',
};
```

### Producción (`src/environments/environment.ts`)

```typescript
export const environment = {
  URL_BACKEND: 'http://api.cursos-udemy.site/',
  URL_SERVICIOS: 'http://api.cursos-udemy.site/api',
  URL_FROTEND: 'http://tienda.cursos-udemy.site',
};
```

### Descripción de Variables

| Variable | Descripción |
|---|---|
| `URL_BACKEND` | URL base del servidor backend (sin `/api`) |
| `URL_SERVICIOS` | URL base de la API REST (con `/api`) |
| `URL_FROTEND` | URL del frontend, usada para redirecciones después del logout |

> **Nota**: El archivo `environment.development.ts` se usa durante `ng serve` y el archivo `environment.ts` se usa durante `ng build --prod`. La sustitución se configura en `angular.json` mediante `fileReplacements`.

---

## Contribuciones

Las contribuciones son bienvenidas. Por favor, sigue estos pasos:

1. **Haz un fork** del repositorio.
2. **Crea una rama** para tu funcionalidad:
   ```bash
   git checkout -b feature/nueva-funcionalidad
   ```
3. **Realiza tus cambios** y haz commit:
   ```bash
   git commit -m "Descripción concisa de los cambios"
   ```
4. **Sube los cambios**:
   ```bash
   git push origin feature/nueva-funcionalidad
   ```
5. **Abre un Pull Request** describiendo los cambios y el propósito.

### Directrices

- Sigue las convenciones de código existentes (Angular Style Guide).
- Asegúrate de que las pruebas pasen: `npm test`.
- Documenta cualquier endpoint de API nuevo en este README.

---

## Licencia

Este proyecto está licenciado bajo la Licencia MIT. Consulta el archivo [LICENSE](LICENSE) para más detalles.

---

## Créditos

- **Framework**: [Angular](https://angular.io/) 15
- **CLI**: [Angular CLI](https://cli.angular.com/)
- **UI Framework**: [Bootstrap](https://getbootstrap.com/)
- **Iconos**: [Font Awesome](https://fontawesome.com/), [Feather Icons](https://feathericons.com/)
- **Notificaciones**: [ngx-toast-notifications](https://github.com/assunzo/ngx-toast-notifications)
- **Pagos**: [PayPal Developer](https://developer.paypal.com/)
- **Plantilla CSS base**: Basada en el tema RBT (React Bootstrap Template) adaptado para Angular

---

¿Tienes preguntas o necesitas ayuda? Abre un issue en el repositorio y con gusto te atenderemos.