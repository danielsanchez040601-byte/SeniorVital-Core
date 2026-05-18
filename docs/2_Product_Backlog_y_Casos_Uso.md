# 📖 Product Backlog y Casos de Uso UML

![Metodología](https://img.shields.io/badge/Metodología-Agile%20%7C%20Scrum-blue)
![Modelado](https://img.shields.io/badge/Modelado-UML%20Estandarizado-success)
![Fase](https://img.shields.io/badge/Fase-Sprint_Técnico_1-orange)

Este documento consolida la planificación ágil y el modelado de comportamiento del sistema **SeniorVital**. Para garantizar la trazabilidad exigida por el **SWEBOK**, cada Especificación Funcional (F) se ha mapeado 1 a 1 con una Historia de Usuario (HU) en el *Product Backlog* y su respectivo Caso de Uso UML (CU).

---

## 🎯 1. Product Backlog: Historias de Usuario (MVP 4 Semanas)

### 🧑‍🦳 HU01: Registro y Onboarding del Perfil de Salud
* **Enunciado:** **Como** Adulto Mayor (+60), **quiero** completar un proceso de registro guiado y corto donde pueda declarar mis restricciones médicas, **para que** la aplicación conozca mi estado de salud desde el primer día y garantice mi seguridad física.
* **Criterios de Aceptación (DoD):**
  * **CA1.1:** El formulario debe limitarse a máximo 5 pasos, sin requerir mecanografía extensa.
  * **CA1.2:** Los botones de selección deben tener un área táctil mínima de 44x44 pt (WCAG 2.1 AA).
  * **CA1.3:** Si se seleccionan restricciones críticas (ej. osteoporosis), el sistema guarda los *tags* en Firestore y muestra un *Disclaimer* médico.

### 🤖 HU02: Generación Inteligente de Rutina Diaria
* **Enunciado:** **Como** Usuario Senior, **quiero** que el sistema me asigne automáticamente una rutina de ejercicios adaptada a mis dolores, **para** ejercitarme en casa sin riesgo de caídas o lesiones.
* **Criterios de Aceptación (DoD):**
  * **CA2.1:** El **Agente Wellness Coach** debe filtrar cualquier ejercicio que choque con los *tags* médicos del usuario en < 2 segundos.
  * **CA2.2:** La rutina se limita a progresiones seguras (Nivel 1: Sentado, Nivel 2: Asistido).
  * **CA2.3:** Los videos demostrativos se transmiten por *streaming* desde GCP Storage (sin saturar el almacenamiento del móvil).

### 📱 HU03: Registro de Ejecución y Fatiga (RPE)
* **Enunciado:** **Como** Adulto Mayor en entrenamiento, **quiero** registrar que terminé una serie y mi nivel de cansancio tocando botones grandes y claros, **para** llevar un control de mi esfuerzo sin complicarme con la tecnología.
* **Criterios de Aceptación (DoD):**
  * **CA3.1:** Feedback multisensorial (vibración 50ms + sonido) al marcar "Serie Completada".
  * **CA3.2:** Escala RPE (1 al 10) codificada con emojis/colores masivos sin introducción de texto.
  * **CA3.3:** Modo **Offline-First**: si no hay internet, guarda en caché local y sincroniza a Firestore en < 5s al reconectar.

### 📈 HU04: Proyecciones Temporales de Logros
* **Enunciado:** **Como** Usuario Constante, **quiero** ver estimaciones de cuándo mejoraré mi movilidad, **para** sentirme motivado con metas realistas de la vida diaria.
* **Criterios de Aceptación (DoD):**
  * **CA4.1:** El **Agente Preventivo / Analytics** procesa el historial cada 7 días para trazar proyecciones lineales.
  * **CA4.2:** El lenguaje debe ser empático y enfocado en la vida diaria (ej. "podrás peinarte más fácil").
  * **CA4.3:** Ocultar sección si hay menos de 7 días de datos para evitar gráficas erráticas.

### 📊 HU05: Dashboard Visual de Progreso
* **Enunciado:** **Como** Usuario Senior, **quiero** ver un panel visual con los días que he entrenado y cómo ha bajado mi fatiga, **para** sentirme orgulloso y mostrárselo a mi médico.
* **Criterios de Aceptación (DoD):**
  * **CA5.1:** Calendario simplificado con contraste 4.5:1.
  * **CA5.2:** Prohibido usar mensajes punitivos por "Rachas rotas" o gráficas obsesivas de peso.
  * **CA5.3:** Botón accesible para exportar historial a reporte médico en PDF.

### 🩺 HU06: Monitoreo Remoto por el Cuidador
* **Enunciado:** **Como** Familiar/Cuidador, **quiero** revisar un resumen de la actividad de mi adulto mayor desde mi móvil, **para** asegurar su bienestar sin invadir su privacidad.
* **Criterios de Aceptación (DoD):**
  * **CA6.1:** Cuenta blindada con regla de **Solo Lectura (Read-Only)** en Firebase.
  * **CA6.2:** Datos cifrados en tránsito (TLS 1.3).
  * **CA6.3:** Notificación Push automática generada por la IA tras 4 días de inactividad del abuelo.

### ⚙️ HU07: Gestión de Pacientes y Rutinas (Admin)
* **Enunciado:** **Como** Fisioterapeuta, **quiero** ver a mis pacientes con indicadores de riesgo y ajustar rutinas manualmente, **para** intervenir clínicamente si es necesario.
* **Criterios de Aceptación (DoD):**
  * **CA7.1:** Panel web con Semáforo IA de Riesgo (Verde/Ámbar/Rojo).
  * **CA7.2:** Capacidad de sobrescribir (override) la rutina del *Agente Wellness Coach*.
  * **CA7.3:** Diseño *responsive* compatible con navegadores de escritorio (Portabilidad).

### 📚 HU08: Gestión de Biblioteca de Ejercicios
* **Enunciado:** **Como** Fisioterapeuta, **quiero** cargar nuevos ejercicios indicando sus restricciones, **para** enriquecer el catálogo que usa la IA.
* **Criterios de Aceptación (DoD):**
  * **CA8.1:** Límite estricto de máximo 4 niveles de progresión de dificultad en el formulario.
  * **CA8.2:** Inserción dinámica de *Tags* médicos (NoSQL) de contraindicación.
  * **CA8.3:** Subida y transcodificación automática de video a GCP Storage.

### 🔄 HU09: Prevención de Estancamiento
* **Enunciado:** **Como** Usuario Senior, **quiero** que el sistema me ofrezca variaciones si nota que no avanzo o hay dolor, **para** no lastimar mis articulaciones.
* **Criterios de Aceptación (DoD):**
  * **CA9.1:** El Agente Preventivo detecta mesetas (2 semanas sin mejora) o RPE crítico (dolor).
  * **CA9.2:** El Agente Coach sustituye automáticamente el ejercicio por uno de impacto distinto.
  * **CA9.3:** Notificación suave sugiriendo la nueva alternativa.

### 🔔 HU10: Recordatorios Proactivos y Empáticos
* **Enunciado:** **Como** Usuario Senior, **quiero** recibir notificaciones amables recordándome hidratarme o entrenar, **para** mantener hábitos sin sentirme regañado.
* **Criterios de Aceptación (DoD):**
  * **CA10.1:** Integración asíncrona con Firebase Cloud Messaging (FCM).
  * **CA10.2:** Tono de refuerzo positivo obligatorio en el *prompt* de la IA.
  * **CA10.3:** Switch visible de "Modo No Molestar" funcional.

### 💧 HU11: Registro Simplificado de Hábitos Diarios
* **Enunciado:** **Como** Usuario Senior, **quiero** anotar vasos de agua u horas de sueño usando botones grandes, **para** llevar control sin conectar relojes inteligentes.
* **Criterios de Aceptación (DoD):**
  * **CA11.1:** Interfaz 100% de botones `[+]` y `[-]` (sin abrir teclado virtual).
  * **CA11.2:** Estructura de base de datos *Fase 2 Ready* (preparada para futura integración de APIs de wearables).

---

## ⚙️ 2. Casos de Uso UML (Especificación Técnica)

*Nota: La visualización gráfica de estos casos de uso se encuentra en el archivo `4_Diagrama_UML.md`.*

| ID | Nombre del Caso de Uso | Actor Principal | Actores Secundarios | Descripción Principal |
| :--- | :--- | :--- | :--- | :--- |
| **CU01** | Registro y Perfil de Salud | Usuario Final / Cuidador | Auth, Firestore | Creación de cuenta y onboarding con captura de tags médicos críticos y *disclaimers*. |
| **CU02** | Generar Rutina Diaria | 🤖 Agente Wellness Coach | Usuario, Firestore | Orquestación segura de ejercicios filtrando contraindicaciones en tiempo real (<2s). |
| **CU03** | Registrar Ejecución (RPE) | Usuario Final | Firestore (Caché) | Ingreso táctil de finalización de serie y fatiga. Funcional bajo modo *Offline-First*. |
| **CU04** | Generar Proyecciones | 🤖 Agente Preventivo | Usuario Final | Procesamiento *Batch* semanal para inferir mejoras de movilidad lineal. |
| **CU05** | Dashboard de Progreso | Usuario Final | Firestore | Despliegue de calendario y tendencias libre de penalizaciones y alto contraste visual. |
| **CU06** | Supervisar Progreso | Familiar / Cuidador | Firestore, Auth | Vista espejo con restricción estricta de "Solo Lectura" (Read-Only) para familiares. |
| **CU07** | Gestión Clínica de Rutinas | Fisioterapeuta / Admin | Firestore | Panel web con semáforo IA para realizar intervenciones (overrides) médicas manuales. |
| **CU08** | Biblioteca de Ejercicios | Fisioterapeuta / Admin | GCP Storage | Alimentación del catálogo NoSQL de la IA respetando el candado de 4 niveles máximos. |
| **CU09** | Detectar Estancamiento | 🤖 Agente Preventivo | 🤖 Agente Coach | Comunicación inter-agente para reestructurar la rutina si el abuelo presenta dolor o meseta. |
| **CU10** | Recordatorios Proactivos | 🤖 Agente Preventivo | FCM | Disparo de notificaciones push de hidratación generadas contextualmente. |
| **CU11** | Registro de Hábitos | Usuario Final | Firestore | Módulo manual para suplir temporalmente (MVP) la falta de wearables biométricos. |
