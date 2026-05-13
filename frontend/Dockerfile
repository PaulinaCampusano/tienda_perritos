# ---- Etapa 1: Preparación (Builder stage) ----
FROM alpine AS builder
WORKDIR /app

# Copiamos los archivos estáticos y de configuración a nuestro entorno de preparación
COPY index.html app.js default.conf ./

# ---- Etapa 2: Producción (Servidor Web) ----
FROM nginx:alpine AS production

# Limpiamos el contenido por defecto de Nginx (Capas limpias)
RUN rm -rf /usr/share/nginx/html/*

# Copiamos los archivos desde la etapa 1 (builder)
COPY --from=builder /app/index.html /app/app.js /usr/share/nginx/html/
COPY --from=builder /app/default.conf /etc/nginx/conf.d/default.conf

# --- Configuración de seguridad: Usuario No Root ---
# Un usuario no root necesita permisos sobre ciertas carpetas de Nginx para poder ejecutarse
RUN chown -R nginx:nginx /usr/share/nginx/html \
    && chown -R nginx:nginx /var/cache/nginx \
    && chown -R nginx:nginx /var/log/nginx \
    && chown -R nginx:nginx /etc/nginx/conf.d \
    && touch /var/run/nginx.pid \
    && chown -R nginx:nginx /var/run/nginx.pid

# Cambiamos al usuario 'nginx' (sin privilegios de administrador)
USER nginx

# Exponemos el puerto 8080. (¡Importante!: Los usuarios no root no pueden usar el puerto 80)
EXPOSE 8080