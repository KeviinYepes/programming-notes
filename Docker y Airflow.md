## Instaladores
   
### ⬇️Instalar Ubuntu en Windows 

1. Instalar Docker Desktop. (Todos los comandos con el docker desktop abierto)
2. Instalar Apache AirFlow.

````bash
wls --install
````
🔄despues de esto reiniciar.

###  📁Crear las carpetas

````bash
ApacheAirflow
│
├── config
├── dags
├── logs
└── plugins
````

### 💻Comando para crear el .yaml
````bash
curl -LfO 'https://airflow.apache.org/docs/apache-airflow/2.8.1/docker-compose.yaml'
````
 Apagar los servicios de docker
 ````bash
docker composer down
````

Encender los servicios de docker

 ````bash
docker composer up -d
````

