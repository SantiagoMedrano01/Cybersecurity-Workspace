### **1. Tener Docker iniciado**

chmod +x auto_deploy.sh
sudo ./auto_deploy.sh duque.tar                     
### **2. Descomprimir el ZIP**
unzip laboratorio.zip
cd laboratorio

### 3. Si tiene un archivo `docker-compose.yml`
docker compose up -d
docker compose -f /ruta/exacta/de/la/carpeta/docker-compose.yml up -d

### 4.**Si solo tiene un archivo `Dockerfile`:
Primero debes construir la imagen y luego crear el contenedor:

Bash

````
docker build -t laboratorio-img .
docker run -d --name mi-laboratorio -p 8080:80 laboratorio-img
``` *(Cambia los puertos `8080:80` según los requerimientos de tu guía).*
````
