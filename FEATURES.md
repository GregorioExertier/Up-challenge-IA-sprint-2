# Documentación de Nuevas Funcionalidades - Broknet

## 📋 Resumen General

Se han implementado tres nuevas funcionalidades principales para mejorar la experiencia administrativa y de usuario:

1. **Administración de Usuarios**: Gestión de permisos administrativos
2. **Administración de Características**: CRUD de características de productos
3. **Visualización de Características**: Muestra de características en detalle de productos

---

## 👥 1. Administración de Usuarios

### Descripción
Permite a los administradores ver todos los usuarios del sistema y asignar/remover permisos administrativos.

### Características principales:
- **Solo acceso para Admin Principal**: Solo "Roccoadmin67" puede acceder a esta página
- **Listar todos los usuarios**: Visualiza nombre, usuario, email, saldo y estado
- **Asignar/Remover permisos**: Cambiar entre estado "Admin" y "Usuario"
- **Admin Principal protegido**: "Roccoadmin67" siempre permanece como admin
- **Interfaz responsiva**: Se adapta a dispositivos móviles

### Cómo acceder:
1. Inicia sesión como administrador
2. Ve a Panel administrativo → Pestaña "Usuarios" → "Administrar usuarios"
3. Desde la URL: `users-admin.html`

### Datos persistentes:
Los cambios se guardan en `data/users.json` en el servidor:
```json
{
  "id": "user_xyz",
  "username": "nombre_usuario",
  "profile": "admin", // o null para usuario normal
  "email": "usuario@email.com",
  "balance": 50000,
  "createdAt": "2026-04-27T..."
}
```

### Endpoints utilizados:
- `GET /api/admin/users` - Obtener lista de todos los usuarios
- `PUT /api/admin/users/:id/profile` - Cambiar perfil de usuario

---

## ⚙️ 2. Administración de Características

### Descripción
Permite crear, editar y eliminar características que pueden ser asociadas a productos.

### Características principales:
- **CRUD completo**: Crear, leer, actualizar y eliminar características
- **Categorización**: Las características se organizan por categoría (Riesgo, País, Tipo)
- **Icono personalizado**: Cada característica tiene un emoji/icono asociado
- **Descripción**: Campo opcional para documentar la característica
- **Validación**: Campos requeridos (nombre, icono, categoría)

### Tipos de características predefinidas:

#### Riesgo:
- 🛡️ Riesgo Bajo
- ⚖️ Riesgo Medio
- ⚠️ Riesgo Alto

#### País/Región:
- 🇦🇷 Argentina
- 🇺🇸 Estados Unidos
- 🌍 Global

#### Tipo de instrumento:
- 📈 Renta Variable
- 📊 Renta Fija
- 🏦 CEDEAR
- 📑 Acciones
- 🎯 Opciones
- ⏳ Futuros
- 💼 Fondos Comunes

### Cómo acceder:
1. Inicia sesión como administrador
2. Ve a Panel administrativo → Pestaña "Características" → "Administrar características"
3. Desde la URL: `characteristics-admin.html`

### Estructura de datos:
Almacenadas en `data/characteristics.json`:
```json
{
  "id": "char_risk_bajo",
  "name": "Riesgo Bajo",
  "icon": "🛡️",
  "category": "risk",
  "description": "Inversión con bajo nivel de riesgo"
}
```

### Endpoints utilizados:
- `GET /api/characteristics` - Obtener características (público)
- `GET /api/admin/characteristics` - Obtener características (admin)
- `POST /api/admin/characteristics` - Crear característica
- `PUT /api/admin/characteristics/:id` - Actualizar característica
- `DELETE /api/admin/characteristics/:id` - Eliminar característica

### Funcionalidades de UI:
- **Formulario de crear/editar**: En la parte superior, cambiar título y botones según acción
- **Botón Cancelar**: Aparece al editar para descartar cambios
- **Alertas**: Mensajes de éxito y error
- **Agrupación por categoría**: Las características se muestran organizadas por categoría

---

## 📦 3. Visualización de Características en Productos

### Descripción
Muestra un bloque visual con todas las características asociadas a un producto en su página de detalle.

### Características principales:
- **Bloque visual separado**: Sección "Características" con icono y nombre
- **Grid responsivo**: Se adapta automáticamente al tamaño de pantalla
- **Información clara**: Muestra icono, nombre y categoría de cada característica
- **Efecto hover**: Las características tienen efecto visual al pasar el mouse
- **Sin dependencias Node.js**: La lógica es 100% JavaScript del lado del cliente

### Cómo funciona:
1. Cuando se carga `product.html?id=PRODUCT_ID`:
   - Se carga la lista de características disponibles
   - Se obtiene el producto con sus características asociadas
   - Se renderiza el bloque de características si existen

### Ejemplo visual:
```
┌─────────────────────────────────────────┐
│ ⚙️ Características                      │
├─────────────────────────────────────────┤
│ 🛡️ Riesgo Bajo  │  🇦🇷 Argentina      │
│ (risk)          │  (country)           │
├─────────────────────────────────────────┤
│ 📈 Renta Variable │  🏦 Acciones       │
│ (type)            │  (type)             │
└─────────────────────────────────────────┘
```

### Estructura de datos en producto:
```json
{
  "id": "AAPL",
  "name": "AAPL",
  "symbol": "AAPL",
  "characteristics": ["char_risk_bajo", "char_country_usa", "char_type_cedear"],
  ...
}
```

### Estilos CSS:
- `.characteristics-block`: Contenedor principal
- `.characteristics-grid`: Grid responsivo
- `.characteristic-item`: Cada característica individual
- `max-width: 150px` mínimo, se adapta automáticamente
- Responsive breakpoint en 768px

### Endpoints utilizados:
- `GET /api/characteristics` - Cargar características disponibles
- `GET /api/products/:id` - Obtener detalles del producto

---

## 🔌 Asociación de Características a Productos

### En el Panel Administrativo:
1. Ve a **Panel administrativo** → Pestaña **"Productos"**
2. Al crear o editar un producto, verás una sección **"Características del producto"**
3. Las características están organizadas por categoría
4. **Selecciona las que apliquen** con los checkboxes
5. Guarda el producto

### En el formulario:
```html
<fieldset>
  <legend>Riesgo</legend>
  <input type="checkbox" value="char_risk_bajo" /> 🛡️ Riesgo Bajo
  <input type="checkbox" value="char_risk_medio" /> ⚖️ Riesgo Medio
  ...
</fieldset>
```

### Datos guardados:
Al guardar un producto, se incluye el array `characteristics`:
```json
{
  "characteristics": ["char_risk_bajo", "char_country_usa", "char_type_cedear"]
}
```

---

## 🔐 Seguridad y Permisos

### Validaciones implementadas:

1. **Administración de Usuarios**:
   - ✅ Solo "Roccoadmin67" puede cambiar permisos
   - ✅ Las contraseñas no se exponen en listados
   - ✅ El admin principal no puede ser removido

2. **Administración de Características**:
   - ✅ Requiere autenticación y perfil admin
   - ✅ Al eliminar una característica, se remueve de todos los productos
   - ✅ Validación de campos requeridos

3. **Productos**:
   - ✅ Solo admins pueden crear/editar/eliminar
   - ✅ Las características son opcionales
   - ✅ Se validan en frontend y backend

---

## 📁 Estructura de Archivos Modificados y Creados

### Backend (Node.js):
- `server.js`: Actualizado con endpoints de características y usuarios

### Frontend - HTML:
- `public/admin.html`: Rediseñado con tabs
- `public/users-admin.html`: Nueva página de administración de usuarios
- `public/characteristics-admin.html`: Nueva página de características
- `public/product.html`: Actualizado con estilos para características

### Frontend - JavaScript:
- `public/assets/js/app.js`: Sin cambios sustanciales (compatible)
- `public/assets/js/admin.js`: Nuevo - Manejo de tabs y características en productos
- `public/assets/js/users-admin.js`: Nuevo - Gestión de usuarios
- `public/assets/js/characteristics-admin.js`: Nuevo - CRUD de características
- `public/assets/js/product.js`: Nuevo - Visualización con características

### Datos:
- `data/characteristics.json`: Nuevo - Almacena características

---

## 🧪 Pruebas Recomendadas

### 1. Administración de Usuarios:
- [ ] Iniciar sesión como "Roccoadmin67"
- [ ] Acceder a usuarios-admin.html
- [ ] Verificar que aparezcan todos los usuarios
- [ ] Cambiar un usuario a admin y verificar en el servidor
- [ ] Intentar remover admin de Roccoadmin67 (no debe permitir)

### 2. Administración de Características:
- [ ] Crear una característica nueva
- [ ] Editar una característica existente
- [ ] Eliminar una característica
- [ ] Verificar que se actualice en productos
- [ ] Crear un producto con características

### 3. Visualización en Productos:
- [ ] Crear un producto con 3-4 características
- [ ] Ir a invertir y ver el producto
- [ ] Verificar que aparezcan todas las características
- [ ] Probar en móvil (responsive)
- [ ] Eliminar una característica y verificar que desaparezca del producto

---

## 🚀 Uso de API

### Ejemplo: Crear una característica
```javascript
fetch('/api/admin/characteristics', {
  method: 'POST',
  headers: {
    'Authorization': 'Bearer TOKEN',
    'Content-Type': 'application/json'
  },
  body: JSON.stringify({
    name: 'Riesgo Bajo',
    icon: '🛡️',
    category: 'risk',
    description: 'Inversión de bajo riesgo'
  })
})
```

### Ejemplo: Asociar características a producto
```javascript
fetch('/api/admin/products', {
  method: 'POST',
  headers: {
    'Authorization': 'Bearer TOKEN',
    'Content-Type': 'application/json'
  },
  body: JSON.stringify({
    name: 'AAPL',
    characteristics: ['char_risk_bajo', 'char_country_usa']
    // ... otros campos
  })
})
```

---

## ⚠️ Consideraciones Importantes

1. **No depende de Node.js**: La visualización de características es 100% JavaScript en el cliente
2. **Compatible con offline**: Si el servidor no responde, usa fallbacks
3. **Datos persistentes**: Se guardan en archivos JSON en el servidor
4. **Responsive**: Todos los interfaces se adaptan a móvil
5. **Seguro**: Validaciones en frontend y backend
6. **Documentado**: Este archivo + comentarios en código

---

## 📞 Soporte

Para problemas:
1. Verifica que estés autenticado
2. Verifica que tengas permisos de admin
3. Revisa la consola del navegador (F12)
4. Verifica que el servidor esté corriendo (`npm start`)

---

**Última actualización**: 27 de abril de 2026
**Versión**: 1.0
