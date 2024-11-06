# Utiliza la imagen oficial de Node.js como base
FROM node:18 AS builder

# Establece el directorio de trabajo
WORKDIR /app

# Copia el archivo de dependencias
COPY package.json pnpm-lock.yaml nx.json ./

# Instala pnpm (si no está en tu imagen base)
RUN npm install -g pnpm

# Instala las dependencias
RUN pnpm install

# Copia el resto de los archivos
COPY . .

# Compila la aplicación
RUN pnpm nx build catalog --configuration=production

# Utiliza una imagen base ligera de NGINX para servir el contenido
FROM nginx:stable-alpine

# Copia los archivos compilados al directorio de NGINX
COPY --from=builder /app/dist/apps/catalog /usr/share/nginx/html

# Expone el puerto 80
EXPOSE 80

# Inicia NGINX
CMD ["nginx", "-g", "daemon off;"]
