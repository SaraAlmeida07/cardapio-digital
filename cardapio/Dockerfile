# ==========================================
# ESTÁGIO 1: Construção (Build)
# ==========================================
# Usa uma versão leve do Node.js
FROM node:20-alpine as build-stage

# Define a pasta de trabalho lá dentro
WORKDIR /app

# Copia os arquivos de dependência e instala tudo
COPY package*.json ./
RUN npm install

# Copia o resto do seu projeto (o .dockerignore impede que a node_modules vá junto)
COPY . .

# Roda o comando do Vite para otimizar e empacotar o projeto para produção
RUN npm run build


# ==========================================
# ESTÁGIO 2: Servidor Web de Produção
# ==========================================
# Usa o Nginx, um servidor web ultrarrápido
FROM nginx:stable-alpine as production-stage

# Copia a pasta 'dist' (o projeto empacotado do estágio 1) para a pasta pública do Nginx
COPY --from=build-stage /app/dist /usr/share/nginx/html

# Expõe a porta 80 (padrão de internet)
EXPOSE 80

# Liga o servidor Nginx
CMD ["nginx", "-g", "daemon off;"]