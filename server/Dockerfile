# Usa uma imagem Node.js baseada em Debian (necessário para instalar o Chrome)
FROM node:18-slim

# Instala bibliotecas necessárias para o Chrome rodar no Linux
# + google-chrome-stable
RUN apt-get update \
    && apt-get install -y wget gnupg \
    && wget -q -O - https://dl-ssl.google.com/linux/linux_signing_key.pub | apt-key add - \
    && sh -c 'echo "deb [arch=amd64] http://dl.google.com/linux/chrome/deb/ stable main" >> /etc/apt/sources.list.d/google.list' \
    && apt-get update \
    && apt-get install -y google-chrome-stable fonts-ipafont-gothic fonts-wqy-zenhei fonts-thai-tlwg fonts-kacst fonts-freefont-ttf libxss1 \
      --no-install-recommends \
    && rm -rf /var/lib/apt/lists/*

# Configura diretório de trabalho
WORKDIR /usr/src/app

# Define variáveis de ambiente pro Puppeteer usar o Chrome instalado e não baixar outro
ENV PUPPETEER_SKIP_CHROMIUM_DOWNLOAD=true
ENV PUPPETEER_EXECUTABLE_PATH=/usr/bin/google-chrome-stable

# Copia dependências primeiro (Cashing do Docker)
COPY package*.json ./

# Instala dependências
RUN npm install

# Copia o código fonte
COPY . .

# Compila o TypeScript
RUN npm run build

# Expõe a porta 3001
EXPOSE 3001

# Comando de inicialização
CMD [ "npm", "start" ]
