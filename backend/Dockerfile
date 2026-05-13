# ---- Etapa 1: Dependencias (Builder stage) ----
FROM node:18-alpine AS builder
WORKDIR /app

# Copiamos solo los archivos de dependencias primero para optimizar la caché de Docker
COPY package*.json ./

# Instalamos solo las dependencias de producción
RUN npm install --production

# ---- Etapa 2: Producción (Ejecución) ----
FROM node:18-alpine AS production
WORKDIR /app

# Copiamos las dependencias instaladas desde la Etapa 1
COPY --from=builder /app/node_modules ./node_modules

# Copiamos el código fuente de tu aplicación
COPY server.js package.json ./

# --- Configuración de Seguridad: Usuario No Root ---
# Otorgamos la propiedad de la carpeta al usuario 'node' (creado por defecto en la imagen)
RUN chown -R node:node /app

# Cambiamos del usuario 'root' al usuario seguro 'node'
USER node

# Exponemos el puerto que usa tu API
EXPOSE 3001

# Comando de inicio
CMD ["npm", "start"]