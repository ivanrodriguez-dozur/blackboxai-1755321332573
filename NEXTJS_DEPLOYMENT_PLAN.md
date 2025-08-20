# 🚀 Plan de Despliegue - TiendaOnline Next.js

## 📋 Información Recopilada

### Estado Actual del Proyecto
- **Framework**: Next.js 14 con App Router
- **Base de datos**: SQLite con Prisma ORM
- **API Routes**: Rutas RESTful completas (/api/products, /api/cart, etc.)
- **Estado global**: Context API con AppContext
- **Autenticación**: Sistema de usuarios simulado
- **Características**: Carrito, favoritos, órdenes, notificaciones, búsqueda

### Requisitos de Despliegue
- **Node.js**: Requerido para servidor Next.js
- **Base de datos**: SQLite (archivo local)
- **Variables de entorno**: Configuración necesaria
- **Build optimizado**: Next.js build requerido

## 🎯 Plan de Despliegue

### Opción 1: Vercel (Recomendado)
**Ventajas**: Despliegue automático, optimizado para Next.js, gratis para proyectos personales

### Opción 2: Railway/Render
**Ventajas**: Soporte para bases de datos persistentes, buena para aplicaciones full-stack

### Opción 3: Docker + VPS
**Ventajas**: Control completo, escalable, buena para producción

## 🔧 Configuración de Variables de Entorno

### Variables Necesarias
```bash
# Database
DATABASE_URL="file:./ecommerce.db"

# Next.js
NEXTAUTH_URL="https://tu-dominio.vercel.app"
NEXTAUTH_SECRET="tu-secreto-aleatorio"

# Analytics (opcional)
NEXT_PUBLIC_GA_ID="G-XXXXXXXXXX"
```

## 📦 Preparación para Despliegue

### 1. Optimización de Build
- [ ] Limpiar dependencias no utilizadas
- [ ] Optimizar imágenes y assets
- [ ] Configurar bundle analyzer
- [ ] Verificar tamaño del bundle

### 2. Configuración de Base de Datos
- [ ] Migrar de SQLite a PostgreSQL (recomendado para producción)
- [ ] Configurar conexión persistente
- [ ] Seed inicial de datos

### 3. Seguridad
- [ ] Configurar CORS apropiadamente
- [ ] Validar y sanitizar inputs
- [ ] Configurar rate limiting
- [ ] HTTPS automático

## 🚀 Pasos de Despliegue - Vercel

### Paso 1: Preparar Proyecto
```bash
# Instalar dependencias
npm install

# Build local para verificar
npm run build

# Iniciar en modo producción local
npm start
```

### Paso 2: Configurar Vercel
1. Instalar CLI de Vercel:
   ```bash
   npm i -g vercel
   ```

2. Login en Vercel:
   ```bash
   vercel login
   ```

3. Desplegar:
   ```bash
   vercel --prod
   ```

### Paso 3: Configurar Variables de Entorno en Vercel
1. Ir al dashboard de Vercel
2. Seleccionar el proyecto
3. Settings → Environment Variables
4. Agregar todas las variables necesarias

### Paso 4: Configurar Base de Datos PostgreSQL
1. Crear base de datos en Vercel Postgres
2. Actualizar DATABASE_URL
3. Ejecutar migraciones:
   ```bash
   npx prisma migrate deploy
   ```

## 🐳 Alternativa: Docker Deployment

### Dockerfile
```dockerfile
FROM node:18-alpine AS deps
RUN apk add --no-cache libc6-compat
WORKDIR /app
COPY package*.json ./
RUN npm ci --only=production

FROM node:18-alpine AS builder
WORKDIR /app
COPY --from=deps /app/node_modules ./node_modules
COPY . .
RUN npm run build

FROM node:18-alpine AS runner
WORKDIR /app
ENV NODE_ENV production

RUN addgroup --system --gid 1001 nodejs
RUN adduser --system --uid 1001 nextjs

COPY --from=builder /app/public ./public
COPY --from=builder /app/.next/standalone ./
COPY --from=builder /app/.next/static ./.next/static

USER nextjs
EXPOSE 3000
ENV PORT 3000
CMD ["node", "server.js"]
```

### docker-compose.yml
```yaml
version: '3.8'
services:
  app:
    build: .
    ports:
      - "3000:3000"
    environment:
      - DATABASE_URL=postgresql://user:password@db:5432/tiendaonline
    depends_on:
      - db
  db:
    image: postgres:15
    environment:
      POSTGRES_DB: tiendaonline
      POSTGRES_USER: user
      POSTGRES_PASSWORD: password
    volumes:
      - postgres_data:/var/lib/postgresql/data
volumes:
  postgres_data:
```

## 🔍 Verificación Post-Despliegue

### Checklist de Funcionalidades
- [ ] Página de inicio carga correctamente
- [ ] Navegación entre páginas funciona
- [ ] API endpoints responden
- [ ] Base de datos conecta
- [ ] Carrito de compras persiste
- [ ] Favoritos funcionan
- [ ] Búsqueda de productos
- [ ] Sistema de órdenes
- [ ] Notificaciones
- [ ] Analytics (si configurado)

### Performance Checks
- [ ] Lighthouse score > 90
- [ ] Tiempo de carga < 3s
- [ ] Core Web Vitals óptimos
- [ ] No hay errores en consola

## 📊 Monitoreo

### Herramientas Recomendadas
- **Vercel Analytics**: Métricas de performance
- **LogRocket**: Session replay y debugging
- **Sentry**: Error tracking y monitoreo

### Métricas a Monitorear
- Tiempo de respuesta de API
- Tasa de conversión
- Errores 4xx/5xx
- Performance de base de datos

## 🔄 CI/CD Pipeline

### GitHub Actions
```yaml
name: Deploy to Vercel
on:
  push:
    branches: [main]
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-node@v3
        with:
          node-version: '18'
      - run: npm ci
      - run: npm run build
      - uses: amondnet/vercel-action@v20
        with:
          vercel-token: ${{ secrets.VERCEL_TOKEN }}
          vercel-org-id: ${{ secrets.ORG_ID }}
          vercel-project-id: ${{ secrets.PROJECT_ID }}
```

## 🚨 Solución de Problemas Comunes

### Error: "Module not found"
- Verificar imports y exports
- Limpiar caché: `rm -rf .next node_modules && npm install`

### Error: Database connection
- Verificar DATABASE_URL
- Asegurar que la base de datos esté accesible
- Verificar permisos de archivo para SQLite

### Error: Build fails
- Verificar sintaxis de TypeScript
- Revisar dependencias faltantes
- Ejecutar `npm run lint`

## 📞 Soporte Post-Despliegue

### Recursos
- [Vercel Documentation](https://vercel.com/docs)
- [Next.js Deployment Docs](https://nextjs.org/docs/deployment)
- [Prisma Deployment Guide](https://www.prisma.io/docs/guides/deployment)

### Comunidad
- [Next.js Discord](https://nextjs.org/discord)
- [Vercel Community](https://vercel.community/)

---

## 🎉 ¡Listo para Desplegar!

Tu aplicación TiendaOnline Next.js está configurada y lista para despliegue. El método recomendado es Vercel por su simplicidad y optimización para Next.js.
