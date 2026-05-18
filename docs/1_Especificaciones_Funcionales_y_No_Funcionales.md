# 📑 Especificaciones Funcionales y No Funcionales (ISO/IEC 25010)

![Métrica de Calidad](https://img.shields.io/badge/Métricas-ISO_25010-blue)
![Accesibilidad](https://img.shields.io/badge/Accesibilidad-WCAG_2.1_AA-success)
![Estándar](https://img.shields.io/badge/Estándar-SWEBOK_Requisitos-orange)

Este documento constituye la especificación formal y estructurada de los requisitos de la plataforma **SeniorVital**, desarrollada bajo las áreas de conocimiento del estándar **SWEBOK** y evaluada bajo el marco de calidad de la norma internacional **ISO/IEC 25010**. El diseño considera de manera explícita el perfil psicofísico del usuario objetivo: **adultos mayores de la tercera edad (+60)** con brecha digital latente.

---

## 1. Especificaciones Funcionales (Módulo Core - MVP 4 Semanas)

### 👥 Módulo de Usuario Final (Adulto Mayor +60)

#### F1: Registro y Onboarding con Perfil de Salud
* **Acción:** Registrar nuevo usuario y configurar el perfil clínico base.
* **Actor(es):** Usuario final (+60) / Familiar o Cuidador.
* **Condición de Disparo:** Primera apertura de la aplicación tras la descarga.
* **Respuesta del Sistema:** * Presentar un onboarding progresivo guiado de máximo 5 preguntas interactivas.
  * Capturar datos antropométricos esenciales (edad, peso, altura).
  * Registrar de forma obligatoria las restricciones médicas críticas (tags de: osteoporosis, hipertensión, artrosis, prótesis, etc.).
  * Almacenar de manera segura la información en formato JSON atómico estructurado dentro de la colección `usuarios` en **Cloud Firestore**.

#### F2: Generación Automatizada de Rutina Diaria Personalizada
* **Acción:** Orquestar la carga de entrenamiento segura para el día actual.
* **Actor(es):** 🤖 Agente Wellness Coach (Sistema).
* **Condición de Disparo:** Acceso del usuario al módulo diario de entrenamiento desde el menú principal.
* **Respuesta del Sistema:**
  * El **Agente Wellness Coach** analiza dinámicamente el perfil médico base y el historial reciente.
  * Aplica un filtro inquebrantable para omitir y bloquear cualquier patrón de movimiento contraindicado.
  * Selecciona rutinas de bajo impacto adaptadas estrictamente a un esquema seguro de **máximo 3 o 4 niveles de progresión funcional** (ej. Nivel 1: Sentado en silla, Nivel 2: Con apoyo de pie).
  * Renderiza la sesión en pantalla vinculando videos demostrativos y audios guía mediante *streaming* asíncrono desde **Google Cloud Storage**.

#### F3: Seguimiento de Ejecución y Registro de Esfuerzo Percibido (RPE)
* **Acción:** Capturar el cumplimiento de series físicas y la percepción del cansancio.
* **Actor(es):** Usuario final (+60).
* **Condición de Disparo:** Ejecución activa de un ejercicio en la interfaz móvil.
* **Respuesta del Sistema:**
  * Ofrecer botones expandidos para marcar el fin de una serie con confirmación multisensorial instantánea (vibración y sonido suave).
  * Al finalizar la rutina, desplegar un control visual de la escala de fatiga **RPE (1 al 10)** codificado con emojis y colores masivos (evitando la introducción de texto manual).
  * Persistir la información mediante un enfoque **Offline-First**, permitiendo el almacenamiento local en caché ante cortes de red y sincronizando automáticamente con Firestore al recuperar la conexión.

#### F4: Proyecciones Temporales de Logros Funcionales
* **Acción:** Calcular estimaciones empíricas de progreso futuro basándose en los hábitos actuales.
* **Actor(es):** 🤖 Agente Preventivo / Analytics (Sistema).
* **Condición de Disparo:** Cumplimiento de un ciclo continuo de 7 días de entrenamiento.
* **Respuesta del Sistema:**
  * Procesamiento por lotes (*batch*) en el backend de Google Cloud.
  * Trazado de proyecciones lineales simples para estimar la ganancia de movilidad futura.
  * Despliegue de un *insight* en lenguaje empático orientado a la vida diaria (ej. *"Manteniendo este ritmo, en 4 semanas podrás abrocharte los zapatos con mayor facilidad"*), ocultándose automáticamente si los datos históricos son insuficientes.

#### F5: Dashboard Visual de Progreso Sostenible
* **Acción:** Presentar un resumen limpio de la constancia sin generar estrés cognitivo.
* **Actor(es):** Usuario final (+60) / Familiar o Cuidador.
* **Condición de Disparo:** Navegación hacia la pestaña de progreso del usuario.
* **Respuesta del Sistema:**
  * Renderizar un calendario intuitivo de asistencia cromática y una gráfica lineal de la reducción paulatina de fatiga RPE.
  * Excluir métricas de peso obsesivas o penalizaciones punitivas por quiebres de racha consecutivos.
  * Proveer un botón interactivo de un solo toque para exportar el historial de salud funcional a un informe médico portable en formato **PDF**.

#### F10: Recordatorios Proactivos y Empáticos
* **Acción:** Notificar alertas oportunas de hidratación, descanso y entrenamiento.
* **Actor(es):** 🤖 Agente Preventivo / Firebase Cloud Messaging (FCM).
* **Condición de Disparo:** Ventanas de tiempo parametrizadas según las preferencias del perfil del usuario.
* **Respuesta del Sistema:** Enviar notificaciones push amigables empleando técnicas de refuerzo positivo, integrando un switch accesible de "Modo No Molestar" para mitigar la sobrecarga atencional.

#### F11: Registro Simplificado de Hábitos Diarios (Mecánica MVP)
* **Acción:** Registrar manualmente variables complementarias de salud.
* **Actor(es):** Usuario final (+60).
* **Condición de Disparo:** Interacción con el panel complementario de hábitos.
* **Respuesta del Sistema:** Permitir la adición manual (vasos de agua, horas de sueño) operada exclusivamente por controles gigantes de `[+]` y `[-]`, omitiendo el despliegue del teclado virtual y dejando la estructura NoSQL *Fase 2 Ready* para futuras integraciones automatizadas con wearables (Google Fit/HealthKit).

---

### 🛡️ Módulo de Seguridad y Supervisión (Familiares y Administradores)

#### F6: Modo Cuidador / Familiar Activo
* **Acción:** Monitorear el estado de bienestar y recibir alertas de riesgo de forma remota.
* **Actor(es):** Familiar / Cuidador.
* **Condición de Disparo:** Apertura de la sesión autenticada con rol asignado de supervisor.
* **Respuesta del Sistema:** Desplegar una vista espejo simplificada de solo lectura (*Read-Only*). Si el **Agente Preventivo** detecta **4 días de inactividad consecutiva** en el adulto mayor, el sistema disparará de manera automática una notificación push preventiva al cuidador.

#### F7: Panel Web de Gestión y Supervisión Clínica (Admin)
* **Acción:** Controlar la población de usuarios y auditar alertas críticas.
* **Actor(es):** Administrador / Fisioterapeuta.
* **Condición de Disparo:** Autenticación exitosa en la consola web de administración.
* **Respuesta del Sistema:** Carga responsiva de un panel con un **Semáforo IA de Riesgo** (Verde/Ámbar/Rojo). Permite al especialista clínico sobrescribir de forma manual cualquier rutina asignada autónomamente por la IA y exportar las analíticas agregadas a formato **CSV**.

#### F8: Mantenimiento de Biblioteca de Ejercicios y Restricciones
* **Acción:** Administrar el catálogo de patrones motores evaluados por la IA.
* **Actor(es):** Administrador / Fisioterapeuta.
* **Condición de Disparo:** Ingreso al módulo de configuración de ejercicios.
* **Respuesta del Sistema:** Formulario estructurado NoSQL que obliga a fijar un **límite innegociable de máximo 4 niveles de progresión segura** por movimiento y permite adjuntar dinámicamente etiquetas semánticas de contraindicación médica, cargando la multimedia en buckets optimizados de Google Cloud Storage.

#### F9: Detección Automatizada de Estancamiento y Ajuste Pasivo
* **Acción:** Identificar mesetas de rendimiento y reestructurar cargas sin fricción.
* **Actor(es):** 🤖 Agente Preventivo & Agente Wellness Coach (Sincronizados).
* **Condición de Disparo:** Análisis periódico en segundo plano sobre los registros de Firestore.
* **Respuesta del Sistema:** Al detectar 2 semanas de inactividad de progreso o picos críticos en el RPE, el Agente Analytics envía una instrucción interna para que el Agente Wellness Coach sustituya el ejercicio estancado por una alternativa segura equivalente de manera transparente en la siguiente sesión del usuario.

---

## 2. Especificaciones No Funcionales (ISO/IEC 25010)

| Característica ISO 25010 | Subcategoría | Requisito de Ingeniería / Criterio de Aceptación Técnico |
| :--- | :--- | :--- |
| **Usabilidad** | Accesibilidad (WCAG 2.1 AA) | • **Tamaño del Objetivo:** Botones e interactivos $\ge$ 44x44 pt para mitigar temblores articulares.<br>• **Tipografía Senior:** Mínimo de 16pt, escalable nativamente hasta 24pt sin romper layouts.<br>• **Contraste Cromático:** Ratio mínimo de **4.5:1** en todos los textos e instrucciones obligatorias.<br>• **Navegación Simplificada:** Arquitectura de información con un tope máximo de **3 niveles de profundidad** en toda la aplicación. |
| **Fiabilidad** | Tolerancia a Fallos y Madurez | • **Enfoque Offline-First:** Integración de la caché local nativa de Cloud Firestore. Persistencia inmediata de datos en entornos sin internet (parques, sótanos) con sincronización automática en un lapso $<$ 5 segundos al recuperar red.<br>• **Disponibilidad del Sistema:** Garantía de un **99.5%** de *uptime* mensual respaldado en la infraestructura *serverless* de **Google Cloud Run**.<br>• **Resiliencia de IA:** Implementación de mecanismos de reintento automatizados con *backoff exponencial* para peticiones asíncronas hacia las APIs de los Agentes de IA (Vertex AI/Gemini). |
| **Seguridad** | Confidencialidad e Integridad | • **Control de Acceso (RBAC):** Autenticación centralizada en Firebase Auth, aplicando reglas de seguridad estrictas que impiden a las cuentas con rol de Cuidador mutar datos médicos sensibles (*Read-Only enforcement*).<br>• **Protección de Datos:** Cifrado de datos en tránsito utilizando el protocolo **TLS 1.3** y en reposo mediante las directivas nativas del backend de Google Cloud. |
| **Eficiencia de Desempeño** | Comportamiento Temporal | • **Latencia de Procesamiento:** La renderización completa de rutinas y el filtrado por patologías operado por el *Agente Wellness Coach* debe completarse en un tiempo de respuesta de servidor inferior a **2 segundos**.<br>• **Optimización de Recursos:** Peso de instalación de la aplicación optimizado por debajo de los **50 MB** delegando la multimedia pesada a streaming asíncrono desde Google Cloud Storage. |
| **Mantenibilidad** | Modificabilidad | • **Arquitectura Acoplada a IA:** Desacoplamiento lógico mediante la encapsulación del comportamiento de los Agentes autónomos en microservicios independientes. Uso de un motor NoSQL flexible que permita añadir nuevos ejercicios y *tags* médicos sin alterar el esquema base del backend. |
| **Portabilidad** | Adaptabilidad | • **Compatibilidad Multiplataforma:** Código base unificado (ej. Flutter o React Native) adaptado de forma responsive para abarcar dispositivos móviles **Android 10+ (API 29)**, **iOS 15+** y **Tablets** (pantallas mayores a 8 pulgadas críticas para la visualización senior). Panel web compatible con las 2 últimas versiones de Chrome, Safari y Edge. |
