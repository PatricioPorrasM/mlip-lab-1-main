# Docker Compose Notes

## Paso 1 — Verificar que Docker esté corriendo
Abre una terminal (PowerShell o CMD) y ejecuta:

´´´bash
docker --version
docker ps
´´´

Si Docker Desktop no está iniciado, ábrelo primero.

## Paso 2 — Ir al directorio del proyecto

´´´bash
cd c:\GitHub\mlip-lab-1-main
´´´

## Paso 3 — Descargar las imágenes

´´´bash
docker compose pull
´´´

Esto descarga apache/kafka:latest y confluentinc/cp-kafkacat:latest. Solo necesitas hacerlo la primera vez.

## Paso 4 — Levantar los servicios

´´´bash
docker compose up -d
´´´

El flag -d lo corre en background (modo detached). Verás algo como:

✔ Container kafka   Started
✔ Container kcat    Started

## Paso 5 — Verificar que Kafka esté sano

´´´bash
docker compose ps
´´´

Espera a que el STATUS de kafka diga healthy (puede tomar ~30 segundos). Mientras tanto puede decir starting.

## Paso 6 — Probar la conexión con kcat

´´´bash
docker exec kcat kcat -b localhost:9092 -L
o
docker exec kcat kcat -b kafka:9092 -L
´´´

Deberías ver la metadata del broker. Esto confirma que Kafka está operativo.

## Paso 7 — Usar en el notebook Python
En KafkaDemo.ipynb, donde diga bootstrap_servers, usa:

bootstrap_servers = "localhost:9092"

# Comandos útiles del día a día
Acción	Comando
Ver logs de Kafka	docker compose logs -f kafka
Detener todo	docker compose down
Detener y borrar datos	docker compose down -v
Reiniciar Kafka	docker compose restart kafka
Ver topics desde kcat	docker exec kcat kcat -b localhost:9092 -L
Consumir desde el inicio	docker exec kcat kcat -b localhost:9092 -t <topic> -o beginning

--- 

Cuando quieras parar el lab, corre docker compose down. Los datos persisten en el contenedor hasta que uses -v.