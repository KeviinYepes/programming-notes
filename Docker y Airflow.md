## Instaladores
   
### ⬇️Obligatorios 
Programas obligatorios para conectar Docker con AirFlow

1. Instalar Docker Desktop. (Todos los comandos con el docker desktop abierto)
2. Instalar Apache AirFlow.
   
---

### 🐧Linux en Windows
Para poder usar linux en windows debemos de ejecutar el siguiente comando
````bash
wls --install

🔄despues de esto reiniciar.
````
para poder ingresar al entorno de Linux en Windows: 
````bash
wls
````
---

###  📁Crear las carpetas
Estructura necesaria para el ApacheAirFlow

````bash
ApacheAirflow
│
├── config
├── dags
├── logs
└── plugins
````
---

### 💻Comando para crear el .yaml
Este comando debe ir dentro de la carpeta `ApacheAirflow`
````bash
curl -LfO 'https://airflow.apache.org/docs/apache-airflow/2.8.1/docker-compose.yaml'
````
---
### 🛠️Construir o Reconstruir una imagen en docker
```bash
   docker compose build --no cache [Nombre de Imagen en caso de reconstruir]
````
---
### 🐋Encender o apagar docker

🔴 Apagar los servicios de docker
 ````bash
docker composer down
````

🟢 Encender los servicios de docker
 ````bash
docker composer up -d
````

