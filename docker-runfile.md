# Guía de Comandos Docker: Configuración de RabbitMQ con STOMP

Guía paso a paso para levantar e interconectar RabbitMQ en Docker con una aplicación de Spring Boot que utiliza WebSockets y `StompBrokerRelay`.

---

## 1. Descarga de la Imagen y Despliegue del Contenedor

Descarga la imagen oficial con el panel de administración web (`management`) y levanta el contenedor exponiendo los **tres puertos esenciales**:

* **`5672`**: Puerto AMQP (mensajería interna con Spring AMQP).
* **`15672`**: Puerto HTTP para el panel de administración web (Management Dashboard).
* **`61613`**: Puerto STOMP (requerido para el `StompBrokerRelay` de Spring WebSocket).

```bash
# Descargar la imagen oficial
docker pull rabbitmq:3-management

# Crear y ejecutar el contenedor con todos los puertos mapeados
docker run -d \
  --hostname my-rabbit-server \
  --name rabbit-event-driver-arch-demo \
  -p 5672:5672 \
  -p 15672:15672 \
  -p 61613:61613 \
  rabbitmq:3-management

```

---

## 2. Habilitación del Plugin STOMP

El puerto `61613` necesita tener activo el protocolo STOMP dentro de RabbitMQ. Ejecuta el comando directamente en el contenedor en ejecución:

```bash
# Activar el plugin de STOMP dentro del contenedor
docker exec -it rabbit-event-driver-arch-demo rabbitmq-plugins enable rabbitmq_stomp

```

---

## 3. Verificación del Estado

Verifica que el contenedor esté corriendo con los puertos expuestos correctamente (`5672`, `15672` y `61613`):

```bash
docker ps

```

---

## 4. Accesos Rápidos

| Servicio | URL / Host | Puerto | Credenciales por Defecto |
| --- | --- | --- | --- |
| **Panel Web (Management)** | `http://localhost:15672` | `15672` | `guest` / `guest` |
| **AMQP (Spring Boot)** | `localhost` | `5672` | `guest` / `guest` |
| **STOMP Broker Relay** | `localhost` | `61613` | `guest` / `guest` |

## 5. Fotos del rabbitMQ y docker en ejecución

<img width="1366" height="768" alt="WhatsApp Image 2026-10-08 at 9 42 50 PM" src="https://github.com/user-attachments/assets/9ea8542f-1b9f-4028-b218-404f63dabd0f" />
<img width="1366" height="768" alt="WhatsApp Image 2026-10-08 at 9 42 50 PM (1)" src="https://github.com/user-attachments/assets/6244d12d-c224-47fb-ad83-9c3fc00f45b3" />
