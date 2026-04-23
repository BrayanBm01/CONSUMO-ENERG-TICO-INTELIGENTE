# Sistema IoT de Consumo Energético Inteligente

## Descripción

Este proyecto implementa un sistema IoT para el monitoreo en tiempo real del consumo energético en un auditorio con cargas variables. El sistema analiza tres circuitos principales:

- Iluminación escénica  
- Equipos audiovisuales  
- Climatización  

Cada circuito es monitoreado mediante sensores de corriente conectados a microcontroladores, permitiendo:

- Medición continua del consumo eléctrico  
- Detección de anomalías o sobrecargas  
- Generación automática de alertas  
- Visualización en tiempo real mediante dashboard  

---

## Arquitectura del Sistema

El sistema sigue una arquitectura basada en eventos y desacoplada mediante el patrón Publish–Subscribe (MQTT).

---

## Diagramas C4

### Nivel 1 — Contexto

Describe el sistema como una caja negra y sus interacciones externas.

Actores:
- Administrador del sistema
- Técnico de mantenimiento

Sistemas externos:
- Broker MQTT (Mosquitto)
- Servicio de notificaciones (Email/SMS)
- Dispositivos ESP32 con sensores SCT013

Flujo general:
ESP32 → MQTT Broker → Node-RED → Dashboard / DB / Alertas

---

### Nivel 2 — Contenedores

| Contenedor | Tecnología | Responsabilidad |
|----------|----------|----------------|
| Sensores | ESP32 + SCT013 | Captura y envío de datos |
| Broker MQTT | Mosquitto | Comunicación Pub/Sub |
| Procesamiento | Node-RED | Lógica, reglas y alertas |
| Base de datos | PostgreSQL | Almacenamiento histórico |
| Dashboard | HTML, CSS, JS | Visualización |

---

## Decisiones Arquitectónicas (ADR)

### ADR 1 — Uso de MQTT (Pub/Sub)

Decisión: Implementar comunicación con MQTT  
Motivo: Desacoplar emisores y receptores  

Consecuencias:
- Alta escalabilidad  
- Fácil integración de nuevos dispositivos  
- Mayor complejidad en gestión de tópicos  

---

### ADR 2 — Procesamiento en Node-RED

Decisión: Centralizar lógica en Node-RED  
Motivo: Reducir carga en microcontroladores  

Consecuencias:
- Alta mantenibilidad  
- Fácil modificación de reglas  
- Posible cuello de botella  

---

### ADR 3 — Uso de Wi-Fi 2.4 GHz

Decisión: Conectividad inalámbrica  
Motivo: Red interna sin cableado  

Consecuencias:
- Fácil instalación  
- Vulnerable a interferencias  

---

### ADR 4 — Arquitectura Event-Driven

Decisión: Sistema reactivo basado en eventos  
Motivo: Procesamiento en tiempo real  

Consecuencias:
- Respuesta inmediata  
- Eficiencia energética  
- Mayor complejidad de diseño  

---

## Instalación

### Requisitos

- Node.js  
- Node-RED  
- Mosquitto MQTT  
- PostgreSQL  
- ESP32 (opcional)

---

### Paso 1 — Instalar Mosquitto

sudo apt update  
sudo apt install mosquitto mosquitto-clients  

Iniciar servicio:

sudo systemctl start mosquitto  

---

### Paso 2 — Instalar Node-RED

npm install -g --unsafe-perm node-red  
node-red  

Abrir en navegador:
http://localhost:1880  

---

### Paso 3 — Configurar Base de Datos

CREATE DATABASE consumo_energetico;

---

### Paso 4 — Dashboard

Puede usarse Node-RED Dashboard o un frontend personalizado.

---

## Demo (Prueba MQTT)

Publicar datos:

mosquitto_pub -h localhost -t "energia/circuito1" -m "7.5"

Suscribirse:

mosquitto_sub -h localhost -t "energia/#"

Flujo esperado:

1. Sensor publica datos  
2. Broker los distribuye  
3. Node-RED procesa  
4. Se actualiza dashboard  
5. Se generan alertas  

---

## Reglas de Consumo

| Rango | Consumo |
|------|--------|
| 2A – 6A | Normal |
| 6A – 10A | Moderado |
| > 10A | Alto |

---

## Atributos de Calidad

- Latencia menor a 500 ms  
- Disponibilidad mayor o igual a 99%  
- Mantenibilidad alta  
- Escalabilidad horizontal  
- Seguridad en accesos  

---

## Tecnologías Utilizadas

- MQTT (Mosquitto)  
- Node-RED  
- PostgreSQL  
- ESP32  
- HTML / CSS / JS  

---

## Autores

- Jaider Luis Ávila Correa  
- Brayan Borja Madrid  

---

## Referencias

- Software Architecture in Practice — Bass, Clements, Kazman  
- MQTT v3.1.1 — OASIS  
- Carnegie Mellon — Software Architecture  

---

## Conclusión

El sistema permite monitorear el consumo energético en tiempo real de forma eficiente, escalable y desacoplada, facilitando la detección de anomalías y la optimización del uso de energía.
