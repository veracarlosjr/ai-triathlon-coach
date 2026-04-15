# 🏊‍♂️ AI Triathlon Coach: Automatización de Entrenamiento de Alto Rendimiento

Este proyecto implementa un sistema de coaching automatizado diseñado para triatletas que buscan optimizar su preparación física sin perder tiempo en la gestión administrativa de sus planes. Utilizando **n8n**, **Google Gemini** (IA) y datos reales de **Strava**, el sistema analiza el rendimiento previo y planifica la semana siguiente de forma inteligente.

## 🎯 Objetivos del Proyecto 2026
El sistema está configurado para alcanzar hitos específicos de rendimiento en Argentina:
- **21K Buenos Aires** (23 de agosto): Objetivo Sub 2h.
- **42K Buenos Aires** (20 de septiembre): Objetivo Sub 4h.
- **Ironman 70.3 Buenos Aires** (4 de octubre): Objetivo Sub 6h.

---

## 🛠️ Stack Tecnológico
- **Orquestación:** n8n (Self-hosted en Docker).
- **Inteligencia Artificial:** Google Gemini (Generative AI).
- **Fuentes de Datos:** Strava API & Google Sheets.
- **Interfaces de Salida:** Gmail API & Google Calendar API.
- **Lenguaje:** JavaScript (para limpieza de datos y manejo de Timezones).

---

## 🚀 Guía de Instalación Paso a Paso

### 1. Infraestructura (Docker)
Para garantizar que el sistema esté disponible 24/7, n8n corre sobre un contenedor Docker.

1. Instalar Docker en tu servidor o PC.
2. Ejecutar el siguiente comando para levantar n8n con persistencia de datos:
   ```bash
   docker run -it --rm --name n8n -p 5678:5678 -v ~/.n8n:/home/node/.n8n n8nio/n8n
3. Acceder a http://localhost:5678.

4. Importante: Configurar la zona horaria en los settings de n8n como America/Argentina/Buenos_Aires.


### 2. Configuración de APIs y Credenciales
   
Cada nodo requiere permisos específicos para operar en n8n:

**Google Cloud (Gmail, Calendar & Sheets):**

Crear un proyecto en Google Cloud Console.

- Habilitar las APIs de Google Calendar, Gmail y Google Sheets.

- Configurar la pantalla de consentimiento OAuth y crear credenciales "OAuth 2.0 Client ID".

- Añadir el Redirect URL proporcionado por n8n.

**Google Gemini (IA):**

- Ingresar a Google AI Studio.

- Generar una API Key y cargarla en el nodo de Gemini en n8n para el análisis de carga de entrenamiento.

**Strava API:**

- Registrar una aplicación en Strava Developers.

- Obtener el Client ID y Client Secret.

- Autenticar el nodo para extraer métricas (distancia, ritmos y carga).

### 3. Lógica del Flujo (Workflow)

El flujo se activa automáticamente cada domingo a las 22:00 (ART) siguiendo esta secuencia:

1. Trigger: Disparador programado semanalmente.

2. Data Extraction: Consulta el historial en Strava y los parámetros base en Google Sheets.

3. AI Analysis: Gemini evalúa la fatiga y el progreso, devolviendo un plan balanceado.

4. Code Node (JS): Procesa el JSON de la IA y corrige el desfase horario (-03:00) para Argentina.

5. Execution: - Agenda las sesiones detalladas en Google Calendar.

       Envía un correo con el Feedback del Coach y la rutina semanal vía Gmail.

---

## 🧠 Lógica de Código Destacada.

Para asegurar que los entrenamientos aparezcan en la hora correcta en Buenos Aires, utilizamos esta lógica de normalización:

JavaScript
// Normalización de fecha y zona horaria (UTC-3)
item.json.start_datetime = `${item.json.dia}T${item.json.hora_inicio}:00-03:00`;
item.json.end_datetime = `${item.json.dia}T${finHoraStr}:${finMinutoStr}:00-03:00`;

---

## 📸 Galería del Proyecto
1. Planificación en Google Calendar
La IA agenda automáticamente los bloques de natación, ciclismo y carrera.

2. Orquestación en n8n
Vista del flujo completo conectando Strava con Gemini y los servicios de Google.

3. Reporte Semanal (Gmail)
Feedback personalizado y desglose técnico de la semana entrante.

---

## 📄 Licencia

Este proyecto es de uso personal para la comunidad de triatletas y entusiastas de la automatización.

Desarrollado por Carlos Vera Senior Data Analyst | Triatleta Amateur
