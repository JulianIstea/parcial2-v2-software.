# Imagen base liviana con Nginx
FROM nginx:alpine

# Copia la aplicación estática al directorio público de Nginx
COPY index.html /usr/share/nginx/html/index.html

# Nginx escucha en el puerto 80
EXPOSE 80

# El CMD por defecto de nginx:alpine ya inicia el servidor en primer plano
