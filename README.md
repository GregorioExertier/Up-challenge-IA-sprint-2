# Broknet - Plataforma de Simulación de Inversiones

Una aplicación web completa para simular inversiones financieras con arquitectura de tres capas: frontend, backend y base de datos plana.

## 📋 Tabla de Contenidos

- [Características](#características)
- [Arquitectura](#arquitectura)
- [Instalación](#instalación)
- [Uso](#uso)
- [API Endpoints](#api-endpoints)
- [Base de Datos](#base-de-datos)
- [Funcionalidades](#funcionalidades)
- [Perfiles de Inversor](#perfiles-de-inversor)
- [Panel Administrativo](#panel-administrativo)
- [Scraping de Datos](#scraping-de-datos)
- [Tecnologías](#tecnologías)
- [Contribución](#contribución)

## ✨ Características

- 🔐 **Autenticación completa**: Registro e inicio de sesión con usuario/contraseña
- 👤 **Perfiles de inversor**: Conservador, Moderado y Agresivo con recomendaciones personalizadas
- 💰 **Simulación financiera**: Saldo virtual, operaciones de compra/venta, historial
- 📊 **Dashboard interactivo**: Visualización de portafolio y balance
- 🛠️ **Panel administrativo**: Gestión completa de productos de inversión
- 📱 **Responsive design**: Funciona en desktop y móvil
- 🔄 **Sincronización**: Datos locales con respaldo en servidor
- 📈 **Scraping automático**: Datos reales de BYMA (Bolsa de Comercio de Buenos Aires)

## 🏗️ Arquitectura

```
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   Frontend      │    │   Backend       │    │   Database      │
│   (HTML/CSS/JS) │◄──►│   (Express.js)  │◄──►│   (JSON files)  │
│                 │    │                 │    │                 │
│ - SPA routing   │    │ - REST API      │    │ - Products      │
│ - Modal auth    │    │ - Auth system   │    │ - Users         │
│ - Real-time UI  │    │ - File storage  │    │ - Transactions  │
└─────────────────┘    └─────────────────┘    └─────────────────┘
```

### Estructura de Archivos

```
broknet/
├── public/                 # Frontend estático
│   ├── index.html         # Página principal
│   ├── dashboard.html     # Panel de usuario
│   ├── investments.html   # Catálogo de inversiones
│   ├── profile.html       # Selección de perfil
│   ├── history.html       # Historial de operaciones
│   ├── admin.html         # Panel administrativo
│   ├── assets/
│   │   ├── css/styles.css
│   │   └── js/app.js
│   └── product.html       # Detalle de producto
├── data/                  # Base de datos JSON
│   ├── products.json
│   ├── users.json
│   └── transactions.json
├── server.js              # Backend Express
├── scrape_byma.py         # Script de scraping
├── package.json
├── requirements.txt
└── README.md
```

## 🚀 Instalación

### Prerrequisitos

- **Node.js** 18+ ([descargar](https://nodejs.org/))
- **Python** 3.8+ ([descargar](https://python.org/))
- **Git** ([descargar](https://git-scm.com/))

### Pasos de Instalación

1. **Clonar el repositorio**
   ```bash
   git clone <repository-url>
   cd broknet
   ```

2. **Instalar dependencias del backend**
   ```bash
   npm install
   ```

3. **Instalar dependencias de Python**
   ```bash
   pip install -r requirements.txt
   ```

4. **Iniciar el servidor**
   ```bash
   npm start
   ```

5. **Abrir en navegador**
   ```
   http://localhost:3000
   ```

6. **Ejecutar scraping de datos (opcional)**
   ```bash
   npm run scrape
   ```

## 🎯 Uso

### Página Inicial
- Al acceder a `http://localhost:3000` siempre se carga **index.html** como página principal
- No requiere autenticación previa para ver las inversiones recomendadas

### Para Usuarios

1. **Registro**: Crear cuenta con nombre, usuario y contraseña
2. **Perfil**: Seleccionar perfil de inversor (Conservador/Moderado/Agresivo)
3. **Explorar**: Ver productos recomendados en la página principal
4. **Invertir**: Comprar/vender activos desde la sección de inversiones
5. **Dashboard**: Gestionar saldo y ver portafolio
6. **Historial**: Revisar todas las operaciones realizadas
7. **Persistencia**: Los usuarios se guardan en el servidor y pueden iniciar sesión desde cualquier computadora

### Para Administradores

1. **Acceso**: Usuario `Roccoadmin67`, contraseña `Roccopelado67`
2. **Crear productos**: Agregar nuevos activos desde el panel admin
3. **Editar productos**: Modificar productos existentes (precios, descripciones, categorías)
4. **Eliminar productos**: Quitar productos del catálogo
5. **Gestionar**: Ver lista completa de productos disponibles

## 🔌 API Endpoints

### Autenticación
- `POST /api/auth/register` - Registro de usuario
- `POST /api/auth/login` - Inicio de sesión
- `POST /api/auth/profile` - Actualizar perfil de usuario

### Productos
- `GET /api/products` - Listar productos (con paginación)
- `GET /api/products/:id` - Detalle de producto específico
- `POST /api/admin/products` - Crear producto (solo admin)
- `PUT /api/admin/products/:id` - Editar producto (solo admin)
- `DELETE /api/admin/products/:id` - Eliminar producto (solo admin)

### Perfiles de Inversor
- `GET /api/profiles` - Configuración de perfiles y filtros por riesgo

### Operaciones Financieras
- `GET /api/balance` - Consultar saldo
- `POST /api/trade` - Realizar compra/venta
- `GET /api/history` - Historial de transacciones

## 💾 Base de Datos

La aplicación utiliza archivos JSON planos para persistencia:

### products.json
```json
[
  {
    "id": "AAPL",
    "name": "Apple Inc.",
    "symbol": "AAPL",
    "category": "Renta Variable",
    "subCategory": "CEDEARs",
    "price": 195.50,
    "featured": "recommended",
    "description": "CEDEAR de Apple para exposición global",
    "details": "Apple es un líder tecnológico global...",
    "image": "assets/images/aapl.jpg"
  }
]
```

### users.json
```json
[
  {
    "id": "user_abc123",
    "name": "Juan Pérez",
    "username": "juanperez",
    "password": "hashed_password",
    "profile": "Moderado",
    "balance": 45000,
    "portfolio": {
      "AAPL": 10,
      "GGAL": 5
    },
    "token": "token_xyz789"
  }
]
```

### profiles.json
```json
{
  "Conservador": {
    "name": "Conservador",
    "description": "Perfil de bajo riesgo...",
    "riskLevel": "Bajo",
    "allowedSubCategories": ["Fondos Comunes", "Acciones", "CEDEARs"],
    "recommendedProducts": ["Fondos Comunes...", "Acciones blue-chip..."],
    "maxAllocationPerAsset": 0.2,
    "minExperience": "Principiante"
  }
}
```

**Uso**: Configuración dinámica de perfiles de inversor que determina qué productos están disponibles para cada tipo de usuario.

## 🎨 Funcionalidades

### Autenticación
- ✅ Registro con validación de contraseña (8+ caracteres, símbolo especial)
- ✅ Inicio de sesión con usuario/contraseña
- ✅ Sesión persistente con localStorage
- ✅ Protección de rutas privadas
- ✅ Logout automático al cerrar sesión

### Perfiles de Inversor
- ✅ Tres perfiles: Conservador, Moderado, Agresivo
- ✅ Recomendaciones personalizadas por perfil
- ✅ Filtrado automático de productos según riesgo

### Simulación Financiera
- ✅ Saldo inicial: $50,000 ARS
- ✅ Operaciones de compra/venta
- ✅ Validación de fondos suficientes
- ✅ Historial completo de transacciones
- ✅ Portafolio en tiempo real
- ✅ **Sincronización automática** de productos desde panel admin

### Gestión de Productos
- ✅ **Creación desde panel admin** con validación completa
- ✅ **Sincronización en tiempo real** con sección de inversiones
- ✅ **Filtrado dinámico** por perfil de inversor desde API
- ✅ **Categorización inteligente** por tipo y subcategoría

### Interfaz de Usuario
- ✅ Diseño responsive (móvil/desktop)
- ✅ Navegación intuitiva
- ✅ Modales para autenticación
- ✅ Mensajes de feedback
- ✅ Loading states

## 👥 Perfiles de Inversor

La aplicación incluye un sistema dinámico de perfiles de inversor que se configura desde la API. Cada perfil determina qué tipos de productos están disponibles para el usuario.

### Configuración de Perfiles

Los perfiles se configuran en `data/profiles.json`:

```json
{
  "Conservador": {
    "name": "Conservador",
    "description": "Perfil de bajo riesgo, enfocado en preservar capital",
    "riskLevel": "Bajo",
    "allowedSubCategories": ["Fondos Comunes", "Acciones", "CEDEARs"],
    "recommendedProducts": ["Fondos Comunes de inversión", "Acciones blue-chip", "CEDEARs estables"],
    "maxAllocationPerAsset": 0.2,
    "minExperience": "Principiante"
  }
}
```

**Campos importantes**:
- `allowedSubCategories`: Tipos de productos que puede ver el perfil
- `recommendedProducts`: Lista de recomendaciones específicas
- `maxAllocationPerAsset`: Límite de inversión por activo (0.0-1.0)
- `minExperience`: Nivel mínimo de experiencia requerido

### Modificación de Perfiles

Para modificar perfiles:
1. Editar `data/profiles.json`
2. Reiniciar el servidor
3. Los cambios se aplican automáticamente a todos los usuarios

### Funcionamiento

1. **Usuario selecciona perfil** durante el registro
2. **API filtra productos** según `allowedSubCategories` del perfil
3. **Interfaz muestra** productos personalizados por riesgo
4. **Información contextual** sobre el perfil y recomendaciones

### Conservador
**Riesgo**: Bajo
**Objetivo**: Preservar capital con rendimientos moderados
**Productos recomendados**:
- Fondos Comunes de inversión
- Acciones de empresas estables
- CEDEARs de compañías blue-chip

### Moderado
**Riesgo**: Medio
**Objetivo**: Balance entre crecimiento y estabilidad
**Productos recomendados**:
- CEDEARs de empresas tecnológicas
- Fondos Comunes mixtos
- Acciones de crecimiento

### Agresivo
**Riesgo**: Alto
**Objetivo**: Máximo crecimiento del capital
**Productos recomendados**:
- Acciones de alto crecimiento
- CEDEARs de empresas innovadoras
- Fondos Comunes de alto riesgo

## 🛠️ Panel Administrativo

### Acceso
- Usuario: `Roccoadmin67`
- Contraseña: `Roccopelado67`

### Funcionalidades
- ✅ Crear nuevos productos de inversión
- ✅ Validación de datos obligatorios
- ✅ Prevención de nombres duplicados
- ✅ Categorización por tipo y subcategoría
- ✅ Lista completa de productos existentes
- ✅ **Sincronización automática** con sección de inversiones
- ✅ **Filtrado dinámico** por perfil de usuario

### Sincronización con Inversiones

Los productos creados en el panel administrativo aparecen **automáticamente** en la sección de inversiones:

1. **Admin crea producto** → Se guarda en `data/products.json`
2. **Usuario visita inversiones** → API devuelve productos actualizados
3. **Filtrado por perfil** → Solo productos permitidos para el perfil del usuario
4. **Visualización inmediata** → Sin necesidad de recargar la página

**Ejemplo**: Un admin crea un nuevo "Fondo Tech 2030" categorizado como "Fondos Comunes". Los usuarios con perfil "Moderado" lo verán inmediatamente en su sección de inversiones.

### Campos de Producto
- **Nombre**: Nombre descriptivo del activo
- **Símbolo**: Código único (ej: AAPL, GGAL)
- **Precio**: Valor inicial en ARS
- **Categoría**: Renta Fija / Renta Variable
- **Subcategoría**: Acciones, CEDEARs, Fondos Comunes, etc.
- **Tipo**: Recomendado / Nuevo / General
- **Descripción**: Texto breve
- **Detalles**: Información completa
- **Imagen**: URL opcional

## 📊 Scraping de Datos

### BYMA (Bolsa de Comercio)
```bash
npm run scrape
```

**Funcionalidad**:
- ✅ Extracción automática de cotizaciones
- ✅ Guardado en `data/byma_scrape.json`
- ✅ Datos históricos para análisis
- ✅ Integración con productos existentes

**Datos extraídos**:
- Símbolos de activos
- Precios actuales
- Volúmenes de negociación
- Variaciones diarias

## 🛠️ Tecnologías

### Frontend
- **HTML5**: Estructura semántica
- **CSS3**: Diseño responsive con variables CSS
- **Vanilla JavaScript**: Lógica del cliente
- **Fetch API**: Comunicación con backend

### Backend
- **Node.js**: Runtime de JavaScript
- **Express.js**: Framework web
- **File System**: Persistencia en archivos JSON
- **CORS**: Comunicación cross-origin

### Base de Datos
- **JSON**: Formato de datos estructurado
- **File-based**: Sin dependencias externas
- **Atomic writes**: Consistencia de datos

### Scraping
- **Python**: Lenguaje de scripting
- **BeautifulSoup**: Parsing HTML
- **Requests**: HTTP client
- **JSON**: Export de datos

### DevOps
- **npm scripts**: Automatización de tareas
- **Git**: Control de versiones
- **ESLint**: Calidad de código

## 🤝 Contribución

1. Fork el proyecto
2. Crear rama feature (`git checkout -b feature/nueva-funcionalidad`)
3. Commit cambios (`git commit -am 'Agrega nueva funcionalidad'`)
4. Push a la rama (`git push origin feature/nueva-funcionalidad`)
5. Crear Pull Request

### Guías de Desarrollo

- **JavaScript**: Usar ES6+ features
- **CSS**: Variables CSS para temas
- **API**: RESTful design principles
- **Testing**: Validar en múltiples navegadores
- **Documentation**: Actualizar README con cambios

## 📄 Licencia

© 2026 Broknet Capital. Todos los derechos reservados.

---

**Desarrollado con ❤️ para la simulación responsable de inversiones**
- El backend ofrece endpoints REST para productos, autenticación, operaciones y cotizaciones.
- El frontend está construido con HTML, CSS y JavaScript puro para mayor compatibilidad.
