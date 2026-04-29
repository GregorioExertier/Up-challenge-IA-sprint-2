# Guía de Instalación y Uso - Nuevas Funcionalidades

## 🚀 Inicio Rápido

### 1. Instalación
```bash
# Instalar dependencias
npm install

# Iniciar servidor
npm start
```

El servidor estará disponible en `http://localhost:3000`

### 2. Acceso a las nuevas funcionalidades

#### Panel Administrativo (Principal)
- URL: `http://localhost:3000/admin.html`
- Usuario: `Roccoadmin67`
- Contraseña: `Roccopelado67`

#### Páginas administrativas:
- **Usuarios**: `http://localhost:3000/users-admin.html`
- **Características**: `http://localhost:3000/characteristics-admin.html`

---

## 📊 Guía paso a paso

### Crear un usuario y hacerlo administrador:

1. Crea un usuario normal desde la página de inicio
2. Inicia sesión como "Roccoadmin67"
3. Ve a `users-admin.html`
4. Busca el usuario en la lista
5. Haz clic en "✅ Hacer Admin"
6. Ahora ese usuario puede acceder a `admin.html`

### Crear características:

1. Inicia sesión como admin
2. Ve a Panel administrativo → Características
3. Rellena el formulario:
   - **Nombre**: Nombre descriptivo
   - **Icono**: Un emoji o símbolo
   - **Categoría**: risk, country, type, other
   - **Descripción**: Explicación breve (opcional)
4. Haz clic en "Guardar característica"
5. La característica aparecerá en la lista

### Asociar características a un producto:

1. Ve a Panel administrativo → Productos
2. Crea un nuevo producto o edita uno existente
3. En la sección "Características del producto":
   - Las características están organizadas por categoría
   - Selecciona las que apliquen con checkboxes
   - Puedes seleccionar múltiples
4. Guarda el producto
5. Verifica en la página de producto que aparezcan

### Ver características en un producto:

1. Ve a "Inversiones" (investments.html)
2. Haz clic en un producto que tenga características
3. En la página de detalle, verás el bloque "⚙️ Características"
4. Cada característica muestra: Icono | Nombre | Categoría

---

## 🔄 Flujo de datos

```
Usuario Admin
    ↓
users-admin.html (ver/cambiar permisos)
    ↓
PUT /api/admin/users/:id/profile
    ↓
data/users.json (actualizado)
    ↓
Usuario ahora es Admin
```

```
Admin
    ↓
characteristics-admin.html (CRUD de características)
    ↓
POST/PUT/DELETE /api/admin/characteristics
    ↓
data/characteristics.json (actualizado)
    ↓
Admin
    ↓
admin.html - Productos (asociar características)
    ↓
POST/PUT /api/admin/products (con características)
    ↓
data/products.json (actualizado)
    ↓
Usuario
    ↓
product.html (ve características asociadas)
    ↓
GET /api/characteristics + GET /api/products/:id
    ↓
Visualización en bloque "Características"
```

---

## 📝 Ejemplos de Características Sugeridas

### Por Riesgo:
```
Riesgo Bajo     → 🛡️
Riesgo Medio    → ⚖️
Riesgo Alto     → ⚠️
```

### Por País:
```
Argentina       → 🇦🇷
Estados Unidos  → 🇺🇸
Brasil          → 🇧🇷
Global          → 🌍
```

### Por Tipo de Instrumento:
```
Renta Variable  → 📈
Renta Fija      → 📊
CEDEAR          → 🏦
Acciones        → 📑
Opciones        → 🎯
Futuros         → ⏳
Fondos Comunes  → 💼
```

---

## 🔧 Solución de problemas

### "No puedo acceder a users-admin.html"
- ✅ Verifica que estés autenticado
- ✅ Verifica que seas admin (username = "Roccoadmin67")
- ✅ Intenta cerrar sesión y volver a iniciar

### "No aparecen características en el producto"
- ✅ Verifica que hayas creado características
- ✅ Verifica que las hayas seleccionado al crear/editar producto
- ✅ Recarga la página del producto (F5)

### "Cambié el perfil pero no funciona"
- ✅ Verifica que sea el admin "Roccoadmin67" haciendo el cambio
- ✅ Recarga la página del usuario (F5)
- ✅ Intenta con otro navegador

### Servidor no responde
```bash
# Verifica que esté corriendo
ps aux | grep node

# Mata proceso anterior si está zombi
kill -9 <PID>

# Reinicia
npm start
```

---

## 💾 Backups y Restauración

Los datos se guardan en:
- `data/users.json`
- `data/characteristics.json`
- `data/products.json`
- `data/transactions.json`

Para hacer backup:
```bash
cp -r data/ data-backup-$(date +%Y%m%d)/
```

Para restaurar:
```bash
rm -r data/
cp -r data-backup-<fecha>/ data/
```

---

## 🧪 Casos de uso típicos

### Caso 1: Crear una inversión nueva con características
1. Admin crea características (ej: "Riesgo Bajo", "Argentina", "Renta Variable")
2. Admin crea un producto nuevo (ej: "YPF")
3. Admin asocia las características al producto
4. Usuario ve el producto con todas sus características en la página de detalle
5. Usuario puede invertir sabiendo las características del activo

### Caso 2: Promover usuario a administrador
1. Usuario normal se registra
2. Admin principal entra a users-admin.html
3. Busca el usuario y le da permisos de admin
4. El usuario ahora puede crear/editar productos y características
5. El nuevo admin puede promover a otros usuarios

### Caso 3: Actualizar características de productos existentes
1. Admin va a admin.html → Productos
2. Edita un producto existente
3. Cambia las características seleccionadas
4. Guarda los cambios
5. Los cambios aparecen inmediatamente en la página de detalle

---

## 📱 Responsividad

Todas las nuevas interfaces son 100% responsivas:

### Desktop (1024px+)
- Grids de múltiples columnas
- Formularios lado a lado

### Tablet (768px - 1023px)
- Grids de 2 columnas
- Ajuste automático de tamaños

### Mobile (< 768px)
- Grids de 1 columna
- Botones full-width
- Fuentes ajustadas
- Espaciado optimizado

Prueba redimensionando el navegador o con F12.

---

## 🔒 Seguridad

### Validaciones implementadas:

1. **Token-based**: Todos los endpoints admin requieren autenticación
2. **Role-based**: Verificación de perfil = "admin"
3. **Restricted actions**: Solo admin principal puede cambiar permisos
4. **Input validation**: Campos requeridos validados en frontend y backend
5. **No password exposure**: Las contraseñas nunca se muestran en listados

### Mejores prácticas seguidas:

✅ Contraseñas almacenadas en texto plano (⚠️ SOLO para simulación educativa)
✅ Tokens generados para sesiones
✅ CORS habilitado
✅ Validación de roles en cada endpoint
✅ Eliminación en cascada de características en productos

---

## 📚 Referencias

- **Características**: Ver `FEATURES.md`
- **API**: Ver endpoints en `server.js`
- **Estilos**: Ver `public/assets/css/styles.css`
- **Configuración**: Ver `package.json`

---

**¿Preguntas?** Revisa los comentarios en el código fuente.

**Última actualización**: 27 de abril de 2026
