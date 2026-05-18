Especificaciones funcionales y no funcionales
Proyecto de desarrollo de un sistema inteligente de gestión wellness basado en IA (SeniorVital)
Realizado por: Abdénago Nahmens y Daniel Sánchez
Maestría Tecnología Información y Comunicación
Cátedra  Ingeniería de Software y Base de Datos


Se ha llevado a cabo un análisis de requisitos sistemático, en el que, actuando como Analista de Requisitos Senior, se han extraído especificaciones accionables orientadas al equipo de desarrollo. Dicho análisis considera de manera explícita el perfil del usuario objetivo: adultos mayores de la tercera edad (+60), caracterizados por una posible brecha digital, recursos económicos limitados, y una necesidad prioritaria de seguridad en la práctica de ejercicios, así como de mecanismos que sostienen la motivación a largo plazo.


Especificaciones Funcionales (¿Qué debe hacer el sistema?)


Módulo de Usuario Final (Adulto +60)
Funcionalidad: Registro y Onboarding con Perfil de Salud
Acción: Registrar nuevo usuario y configurar perfil de salud inicial.
Actor(es): Usuario final (+60), opcionalmente familiar/cuidador.
Condición de disparo: Primera apertura de la aplicación.
Respuesta esperada del sistema:
Presentar un proceso guiado de máximo 5 preguntas iniciales (onboarding progresivo).
Capturar datos antropométricos: edad, peso, altura.
Capturar nivel de condición física autopercibido.
Capturar objetivos principales (fuerza, movilidad, flexibilidad, pérdida de peso, etc.).
Capturar restricciones médicas (artritis, osteoporosis, hipertensión, dolor articular, uso de prótesis) para que el Agente Wellness Coach filtre automáticamente los ejercicios contraindicados. 
Preguntar por disponibilidad de equipamiento (ninguno, bandas elásticas, etc.).
Generar el perfil de usuario y almacenarlo en formato de documento (JSON) dentro de Cloud Firestore (Base de datos NoSQL en GCP). 
Datos involucrados: Edad, peso, altura, restricciones médicas (array/lista de strings), objetivos (array/lista de strings), equipamiento disponible (lista), rol del usuario, fecha de registro. 


Funcionalidad: Generación de Rutina Diaria Personalizada
Acción: Sugerir rutina de ejercicios del día.
Actor(es): Agente Wellness Coach. 
Condición de disparo: Diario (ej. al abrir la app) o tras completar evaluación de fatiga.
Respuesta esperada del sistema:
Generar una rutina de bajo impacto (ej. "rutina de movilidad en silla", "entrenamiento de fuerza asistida"). 
Priorizar ejercicios sin equipamiento costoso ni riesgos de caídas (sentadillas asistidas con silla, flexiones en pared, movilidad articular). 
Adaptar la dificultad según el nivel actual del usuario utilizando un sistema simplificado de máximo 3 a 4 niveles de progresión segura (ej. Nivel 1: Sentado, Nivel 2: Con apoyo de silla, Nivel 3: De pie dinámico). 
Incluir calentamiento articular obligatorio y ejercicios de equilibrio (prevención de caídas). 
Mostrar cada ejercicio con video demostrativo accesible (alto contraste, velocidad ajustable) y voz guía opcional.
Ajustar dinámicamente la rutina si el usuario reporta fatiga/dolor en la escala RPE. 
Datos involucrados: Historial de rutinas, nivel de fuerza (progresión actual), restricciones médicas, fatiga reportada, equipamiento disponible. 


Funcionalidad: Seguimiento de Ejercicio y Progresión
Acción: Registrar series, repeticiones y esfuerzo percibido durante la rutina.
Actor(es): Usuario final (+60) o cuidador. 
Condición de disparo: Durante la ejecución de una rutina (pantalla de ejercicio activo).
Respuesta esperada del sistema:
Permitir marcar serie completada (tiempo de respuesta <500 ms) y registrar repeticiones.
Ofrecer temporizador de descanso configurable y con alertas sonoras/visuales claras entre series. 
Registrar la escala RPE (esfuerzo percibido 1-10 adaptada visualmente con emojis/colores) al final de cada ejercicio. 
Almacenar los datos transaccionales de la sesión de manera estructurada en Cloud Firestore (Base de Datos NoSQL en GCP). 
Enviar los datos al Agente Preventivo / Analytics para actualizar el sistema de progresión de forma segura al finalizar la semana. 
Datos involucrados: Ejercicio ID, número de series, repeticiones, peso utilizado (si aplica), RPE, fecha-hora, duración de descanso. 


Funcionalidad: Proyecciones Temporales Personalizadas (Diferenciador Clave)
Acción: Mostrar proyección de logros futuros basada en ritmo actual.
Actor(es): Agente Preventivo / Analytics. 
Condición de disparo: Al finalizar la semana o al alcanzar una racha constante de cumplimiento. 
Respuesta esperada del sistema:
Calcular una proyección lineal automatizada adaptada al alcance del MVP de 4 semanas para estimar la ganancia de movilidad o resistencia. 
Mostrar un mensaje motivador y personalizado con lectura de voz opcional: "María, llevas 2 semanas constante. Si mantienes este ritmo, el Agente calcula que en 4 semanas tu flexibilidad de hombro mejorará lo suficiente para peinarte con mayor facilidad." 
Proyectar la fecha estimada dentro de los límites seguros para avanzar al siguiente nivel de progresión física (ej. "alcanzarás el Nivel 3 de fuerza en 3 semanas"). 
No se encuentra implementado en competidores (oportunidad validada).
Datos involucrados: Historial de cumplimiento (racha), evolución de repeticiones y escala RPE por ejercicio, objetivo declarado. 


Funcionalidad: Dashboard de Progreso Visual
Acción: Visualizar evolución de métricas clave de salud y bienestar. 
Actor(es): Usuario final (+60), familiar/cuidador. 
Condición de disparo: Navegación a sección "Mi progreso".
Respuesta esperada del sistema:
Mostrar un calendario de entrenamiento simplificado con indicadores visuales de alto contraste para los días completados. 
Gráfico de barras o líneas limpio que ilustre la evolución de la movilidad o disminución del esfuerzo percibido (RPE). 
Mapa anatómico bidimensional simplificado que resalte los grupos musculares estimulados. 
Resumen de la racha actual (premiando la constancia sin penalizaciones psicológicas por quiebres). 
Opción de cargar un registro fotográfico mensual almacenado de forma segura en Google Cloud Storage (Buckets de GCP). 
Datos involucrados: Historial de sesiones, registros de RPE, archivos de imagen (fotos de progreso). 


Funcionalidad: Modo Cuidador/Familiar
Acción: Supervisar de forma remota el progreso físico de un adulto mayor. 
Actor(es): Familiar o cuidador (rol secundario).
Condición de disparo: Vinculación de cuentas autorizada por el usuario principal.
Respuesta esperada del sistema:
Presentar un panel espejo de lectura simplificada (cumplimiento semanal, alertas proactivas emitidas por la IA).
Disparar notificaciones push o alertas automáticas gestionadas por el Agente Preventivo / Analytics si el usuario principal acumula inactividad inusual dentro de la semana.
Restringir estrictamente los privilegios del cuidador a solo lectura (Read-Only), impidiendo la modificación de las rutinas asignadas por el Agente Wellness Coach. 
Datos involucrados: ID del usuario supervisado, token de vinculación, bandera de alertas activas en Firestore. 


Módulo de Administrador / Entrenador (Staff)
Funcionalidad: Gestión de Usuarios y Supervisión de Rutinas (Módulo Admin) 
Acción: Supervisar, ajustar o asignar rutinas terapéuticas de forma individual o grupal. 
Actor(es): Administrador (Fisioterapeuta/Entrenador), Agente Preventivo / Analytics. 
Condición de disparo: Acceso autenticado al panel web de administración. 
Respuesta esperada del sistema:
Listar usuarios con una vista de semáforo gestionada por la IA: verde (en ritmo), ámbar (riesgo de abandono), rojo (inactivo >7 días o con fatiga severa reportada). 
Permitir al administrador revisar y, si es necesario, sobrescribir o ajustar la rutina sugerida por el Agente Wellness Coach (ej. asignar manualmente una "Rutina específica de movilidad para artrosis de rodilla"). 
Visualizar el historial detallado almacenado en Firestore: series/repeticiones por ejercicio, RPE, cumplimiento semanal. 
Exportar datos filtrados a formato CSV/PDF para que el usuario pueda entregarlo como informe a su médico tratante. 
Datos involucrados: Colección de usuarios, rutinas asignadas, historial de RPE, métricas agregadas por el agente. 


Funcionalidad: Biblioteca de Ejercicios y Progresiones Seguras 
Acción: Crear y parametrizar el catálogo de ejercicios de bajo impacto para que la IA los asigne. 
Actor(es): Administrador (fisioterapeuta o entrenador).
Condición de disparo: Acceso al módulo de "Gestión de ejercicios".
Respuesta esperada del sistema:
Definir ejercicios funcionales (ej. "Sentadilla asistida con silla") con un límite estricto de 3 a 4 niveles de progresión segura (ej. Nivel 1: Rango corto sentado, Nivel 2: Apoyo isométrico, Nivel 3: Levantamiento con silla). 
Asociar a cada nivel: descripción adaptada, URL del video demostrativo (alojado en Google Cloud Storage) y criterios de paso conservadores (ej. "Completar 3 días sin dolor articular"). 
Marcar etiquetas de contraindicaciones médicas obligatorias (tags como "prohibido para osteoporosis severa" o "evitar en prótesis de cadera"). 
Actualizar el diccionario de datos en Firestore para que el Agente Wellness Coach filtre instantáneamente los ejercicios peligrosos al generar rutinas. 
Datos involucrados: Nombre del ejercicio, niveles (1..4), video URL (GCP Storage), tags de contraindicaciones, grupos musculares primarios. 
.
Módulo de Sistema Automático (Backend)
Funcionalidad: Detección de Estancamiento y Ajuste de Rutina (Módulo Backend) 
Acción: Detectar mesetas de rendimiento o dolor prolongado y reestructurar la rutina de forma autónoma. 
Actor(es): Agente Preventivo / Analytics, Agente Wellness Coach. 
Condición de disparo: Evaluación periódica (semanal) del historial del usuario en Firestore. 
Respuesta esperada del sistema:
El Agente Preventivo analiza los datos y detecta una "meseta" (mismas repeticiones durante 2 semanas) o un estancamiento por fatiga (escala RPE consistentemente alta). 
Notificar al usuario con empatía y sin frustración: "Noté que este ejercicio te está costando un poco más. ¿Qué te parece si probamos una variante más cómoda hoy?" 
El Agente Preventivo instruye al Agente Wellness Coach para que sugiera un ejercicio del mismo nivel de progresión (ej. Nivel 2) pero con distinto patrón de movimiento o menor impacto articular. 
Datos involucrados: Historial temporal de repeticiones en Firestore (últimas 2-4 semanas), valores de RPE, fecha del último ajuste de rutina. 


Funcionalidad: Recordatorios y Notificaciones Proactivas
Acción: Enviar recordatorios amigables de rutina, hidratación y bienestar general. 
Actor(es): Agente Preventivo / Analytics (vía Firebase Cloud Messaging - FCM). 
Condición de disparo: Horario configurado por el usuario o detección de inactividad prolongada (>2 días). 
Respuesta esperada del sistema:
Generar notificaciones push dinámicas y contextuales: "Hora de tu rutina de movilidad. ¡Unos minutos al día hacen la diferencia!" o "¿Recuerdas tomar agua? Llevas 3 vasos hoy." 
Mantener un refuerzo positivo estricto, eliminando cualquier sistema de "rachas punitivas" que pueda generar ansiedad o culpa en el adulto mayor si falta un día por motivos de salud. 
Permitir la configuración granular en el perfil (horarios preferidos, activar/desactivar alertas de hidratación). 
Datos involucrados: Preferencias de horario de usuario, token del dispositivo (FCM), timestamp de última notificación, historial de aperturas de la app. 


Funcionalidad: Registro de Hábitos y Wearables (Acotación MVP 4 Semanas) 
Acción: Registrar métricas complementarias de salud (pasos, sueño, frecuencia cardíaca). 
Actor(es): Usuario final (+60), Agente Preventivo / Analytics. 
Condición de disparo: Carga manual del usuario o sincronización en segundo plano (Fase 2). 
Respuesta esperada del sistema:
Para el MVP de 4 semanas: Permitir la entrada manual simplificada de hábitos diarios (ej. "¿Cuántas horas dormiste?" o registro manual de vasos de agua) mediante botones grandes. 
Para el Trabajo Futuro (Fase 2 / Tesis): Importar automáticamente pasos diarios, ritmo cardíaco y horas de sueño mediante integración con APIs de Wearables (Google Fit / Apple HealthKit / ROOK). 
Correlacionar los hábitos con el rendimiento: El Agente Preventivo envía un insight como: "He notado que cuando duermes menos de 6 horas, tu fatiga en la rutina aumenta. ¡Intenta descansar más hoy!" 
Datos involucrados: Pasos diarios, frecuencia cardíaca, horas de sueño (profundo/ligero), timestamp. 


Especificaciones No Funcionales (Atributos de Calidad)
Usabilidad
Requisito
Fuente / Justificación Clínica
Estándar Sugerido
Tamaño de botones mínimo 44x44 pt
Compensación de pérdida de motricidad fina (temblores / artritis).
Material Design (48dp) / WCAG 2.1
Tipografía mínimo 16pt, configurable hasta 24pt
Adaptación a presbicia y agudeza visual reducida en usuarios mayores (+60).
WCAG 2.1 nivel AA (escalable 200%)
Contraste de color ratio mínimo 4.5:1
Mejorar legibilidad ante cataratas o daltonismo por edad.
WCAG 2.1 AA
Navegación máximo 3 niveles de profundidad
Mitigar fatiga cognitiva y evitar que el usuario se "pierda" en la app.
Estándar UX de usabilidad gerontológica
Onboarding progresivo máximo 5 preguntas iniciales
Reducir el tiempo de escritura manual para prevenir abandono temprano.
Validado para retención en usuarios senior
Feedback táctil y sonoro al completar ejercicio
Confirmación multisensorial de una acción para compensar déficits de atención.
Recomendable: Vibración corta (50ms) + tono suave
Tiempo de aprendizaje para usuario novel
Diseñado para usuarios con alta brecha digital (inmigrantes digitales).
< 3 minutos para completar y registrar la primera rutina



Rendimiento
Métrica
Límite Aceptable
Fuente / Justificación
Tiempo de respuesta del Agente Wellness Coach (Generación de rutina)
< 2 segundos
Crítico para evitar que el usuario (+60) piense que la app se congeló.
Respuesta de la interfaz al marcar serie completada o RPE
< 500 ms
Prevención de toques dobles por problemas de motricidad fina (Parkinson leve/temblores).
Sincronización de progreso a la nube
< 5 segundos al recuperar conexión
Aprovechamiento de la caché offline nativa de Cloud Firestore.
Consumo de batería (sesión de 30 min)
< 10% de batería
Crítico para adultos mayores que olvidan cargar sus dispositivos.
Tamaño de la App (Descarga inicial)
< 50 MB
Optimización para teléfonos de gama de entrada. Los videos se transmitirán por streaming desde GCP Cloud Storage, no se descargarán.



Mantenibilidad
Modularidad (Cloud-Native): Arquitectura basada en microservicios (Gestión de Usuarios, Rutinas, Analíticas y Notificaciones) desplegados de forma independiente mediante contenedores en Google Cloud Run. Esto permite escalar o actualizar un módulo sin afectar la disponibilidad del resto del sistema. 
Documentación Viva (Living Documentation): La documentación técnica será generada y mantenida como código en el repositorio de GitHub (ej. README.md). Los diagramas de arquitectura y casos de uso se construirán utilizando Mermaid.js, garantizando que la documentación evolucione a la par del código fuente. 
Facilidad para añadir ejercicios (NoSQL): El administrador podrá crear nuevas rutinas o añadir etiquetas de restricciones médicas mediante un formulario web sin necesidad de reprogramar. Al utilizar Cloud Firestore (NoSQL), la colección de ejercicios permite insertar nuevos atributos dinámicamente sin el bloqueo de un esquema rígido. 
Desacoplamiento de Agentes de IA: La lógica de los Agentes (Wellness Coach y Preventivo) estará separada del backend central interactuando a través de APIs. Esto permite que en el futuro se pueda actualizar el modelo de IA subyacente (ej. cambiar a una versión más nueva de Gemini en Vertex AI) sin necesidad de una refactorización total del sistema. 


Tiempos de Respuesta (Servidor)
Operación
Tiempo Máximo (p95)
Nota Técnica / Justificación
Login / Autenticación
< 1 segundo
Validación mediante token seguro (Firebase Auth / JWT) para evitar fatiga de espera en el adulto mayor.
Generar / Consultar rutina del día
< 2 segundos
Incluye el tiempo de procesamiento y personalización en tiempo real por parte del Agente Wellness Coach.
Guardar sesión completa
< 1 segundo
Escritura tipo batch (lote) optimizada directamente en Cloud Firestore (NoSQL).
Generar proyección temporal
< 2 segundos
Proyección lineal calculada por el Agente Preventivo / Analytics. Puede ser asíncrono o almacenarse en caché semanalmente.
Dashboard de progreso (carga inicial)
< 2 segundos
Datos servidos rápidamente mediante consultas indexadas en Firestore y paginación en el Frontend.



Seguridad (ISO 25010)
Cifrado de Datos: Toda la información de salud sensible (restricciones médicas, RPE, peso, fotos) cuenta con cifrado automático en reposo mediante AES-256 (gestionado por GCP) y en tránsito mediante el protocolo TLS 1.3. 
Autenticación Unificada: El inicio de sesión se gestionará a través de Firebase Authentication (tokens JWT de renovación automática), garantizando un acceso seguro (login en < 1 segundo) con opciones simplificadas como Google u OAuth2. 
Control de Accesos Basado en Roles (RBAC): * El Usuario Final (+60) tiene control total sobre sus datos de salud. 
El Familiar/Cuidador opera bajo un principio de privilegio mínimo, con acceso estricto de solo lectura (Read-Only) a los tableros de progreso y alertas. 


Cumplimiento Legal y Clínico: * Consentimiento y Derecho al Olvido: Aceptación explícita para el uso de datos biológicos e imágenes, garantizando el borrado lógico y físico total de los registros en un plazo de 30 días tras la solicitud de baja. 
Portabilidad: Los historiales de RPE y cumplimiento podrán ser exportados en formato JSON o informe PDF para el médico tratante. 
Buenas Prácticas: El diseño sigue los principios de resguardo clínico de la ley HIPAA para proteger la confidencialidad absoluta del adulto mayor. 


Portabilidad
Plataforma / Entorno
Compatibilidad Requerida
Nota Técnica / Justificación
Móvil (Android / iOS)
Android 10+ / iOS 15+
Desarrollo multiplataforma (ej. Flutter o React Native) para abarcar el 90% de los dispositivos del mercado con un único código base durante el MVP.
Tablet (iPadOS / Android)
Pantallas > 8 pulgadas
Crítico para Tercera Edad (+60): La interfaz debe ser fluida y responsiva en tablets, ya que los adultos mayores suelen preferir pantallas grandes para ver los videos de los ejercicios.
Web (Admin y Modo Cuidador)
Chrome, Safari, Edge (últimas 2 versiones)
Acceso universal a los paneles de administración y supervisión de alertas sin necesidad de instalar la aplicación (diseño web responsivo).
Smartwatch / Wearables
N/A en MVP
Excluido del MVP de 4 semanas. La integración de APIs de salud se reserva para la Fase 2 (Tesis). En el MVP solo se recibirán notificaciones push estándar.
Backend / Agentes IA
Contenedores estandarizados (Docker)
El código de los microservicios y la conexión a los agentes será 100% portable, empaquetado para ejecutarse nativamente en Google Cloud Run.



Compatibilidad / Integraciones
APIs Externas Obligatorias (MVP): Integración nativa con Firebase Cloud Messaging (FCM) para el envío de notificaciones y alertas generadas por el Agente Preventivo / Analytics. (Nota: La integración con Google Fit, Apple HealthKit y Terra/ROOK API queda diseñada a nivel de arquitectura, pero su implementación se pospone para la Fase 2 / Tesis). 
Formatos de importación/exportación:
Importar: CSV para la carga inicial de la biblioteca de ejercicios por el administrador. 
Exportar: JSON (progreso del usuario), PDF (informe para médico), CSV (para entrenador).
Calendarios: Integración opcional con Google Calendar / iCal para agendar las rutinas sugeridas por el Agente Wellness Coach (Fase 2). 
Disponibilidad esperada: 99.5% (aprox. 3.7 horas de downtime al mes), respaldado por los SLAs (Acuerdos de Nivel de Servicio) nativos de la infraestructura serverless de Google Cloud Run. 
Recuperación ante fallos (Disaster Recovery): En lugar de bases de datos relacionales tradicionales, el sistema confía en la replicación multi-zona automática de Cloud Firestore, eliminando la necesidad de mantener servidores en hot standby. Tiempo Objetivo de Recuperación (RTO) < 4 horas, Punto Objetivo de Recuperación (RPO) < 15 minutos. 
Persistencia de datos sin pérdida (Offline-First): Sustitución de bases de datos locales (SQLite) por la caché offline nativa de Cloud Firestore. Los datos de las sesiones y la escala RPE introducidos por el adulto mayor sin conexión a internet se persisten automáticamente y se sincronizan al recuperar la red, asegurando integridad transaccional sin código extra. 
Mecanismos de Resiliencia: Implementación de reintentos con backoff exponencial (esperas progresivas) específicamente para la comunicación entre el backend y los Agentes de IA (ej. Vertex AI / Gemini), previniendo fallos en la generación de rutinas si hay micro-cortes de red. 


Accesibilidad (WCAG 2.1 nivel AA)
Criterio WCAG 2.1
Requisito de Calidad
Implementación Arquitectónica e Impacto Senior (+60)
1.4.3 Contraste (Mínimo)
Ratio mínimo de 4.5:1
Obligatorio para textos informativos y etiquetas. Mitiga el impacto de las cataratas, glaucoma o la pérdida natural de sensibilidad al contraste cromático.
1.4.4 Escalado de Texto
Zoom fluido hasta el 200%
La interfaz debe ser responsiva y redimensionar fuentes sin romper cajas de texto ni superponer elementos. Esencial para usuarios con presbicia severa.
1.4.12 Espaciado de Texto
Interlineado ≥ 1.5
Configuración de estilos globales con espaciado amplio entre caracteres y párrafos separados. Reduce la fatiga de lectura y la desorientación visual en pantallas móviles.
2.1.1 Teclado / Navegación
Accesibilidad total por periféricos
Toda funcionalidad interactiva debe ser operable sin gestos táctiles complejos (ej. drag and drop o deslizamientos rápidos), permitiendo el uso de pulsadores externos o teclados físicos adaptados.
2.5.5 Tamaño del Objetivo
Área de toque ≥ 44x44 pt
Candado de diseño UI. Maximiza la tasa de éxito al presionar botones, previniendo la frustración causada por temblores esenciales o artritis en las manos.
3.3.2 Etiquetas e Instrucciones
Marcado explícito de campos
Prohibido usar textos de marcador (placeholders) como única guía. Cada campo del onboarding debe incluir etiquetas estáticas y unidades claras (ej. "Peso (ej. 70 kg)") para evitar sobrecarga de memoria de trabajo.
4.1.2 Nombre, Rol, Valor
Compatibilidad con Lectores
Inclusión estricta de propiedades de accesibilidad (aria-label, etiquetas semánticas) para garantizar una interpretación correcta por parte de TalkBack (Android) y VoiceOver (iOS).
Criterio Avanzado (IA)
Interfaz de Voz Táctico-Asistida
Alineación con el Año Agéntico: Implementación de comandos de voz básicos locales ("siguiente ejercicio", "pausa", "reportar dolor") para permitir la operación manos libres, reduciendo el riesgo de caídas al manipular el teléfono durante la rutina.



Priorización y Dependencias
Mínimo Producto Viable (MVP) - 4 Semanas
Requisito Funcional
Prioridad
Dependencia Técnica / Operativa
Registro y onboarding con Perfil de Salud (Incluye restricciones médicas obligatorias)
Crítica
Autenticación en Firebase Auth. (Esencial para que la IA filtre riesgos).
Biblioteca base de ejercicios de bajo impacto (ej. movilidad en silla, sentadilla asistida)
Crítica
Contenido validado por fisioterapeuta / geriatra. (Reemplaza ejercicios de piso).
Generación de Rutina por Agente Wellness Coach (Máx 3-4 niveles de progresión segura)
Crítica
Biblioteca de ejercicios configurada y Perfil de Salud del usuario.
Seguimiento transaccional de series, repeticiones y esfuerzo (RPE)
Crítica
Backend NoSQL estructurado en Cloud Firestore (GCP).
Dashboard de progreso visual (Calendario y gráfico de reducción de fatiga/RPE)
Alta
Historial de sesiones guardado en la base de datos.
Modo Offline-First (Guardar sesión sin internet)
Alta
Habilitar la caché offline nativa de Cloud Firestore (Reemplaza SQLite).
Notificaciones proactivas (Agente Preventivo / Analytics)
Media
Integración y configuración de Firebase Cloud Messaging (FCM).



No incluido en MVP-Pospuesto para Fase 2 :
Modelos de IA Predictiva Profunda (Redes Neuronales / LSTM): Entrenar modelos complejos requiere meses de datos. El MVP se limitará a proyecciones lineales algorítmicas calculadas por el Agente Preventivo / Analytics. 
Integración automática con Wearables (APIs de Salud): La conexión con Google Fit, Apple HealthKit o Terra API requiere certificaciones extensas. Durante el MVP, el registro de hábitos (sueño, agua) se realizará de forma manual. 
Integración de Calendarios Externos: La sincronización bidireccional con Google Calendar / iCal queda pospuesta. 


Dependencias Clave
Dependencia Clínica y de Contenido (Ejercicios Sentados): Al enfocarse en un público +60, la inclusión de ejercicios sentados y asistidos es de carácter obligatorio desde el Día 1. Los videos demostrativos, limitados estrictamente a 3 o 4 niveles de progresión segura, requieren validación previa por un especialista (fisioterapeuta/geriatra) antes de ser alojados en Google Cloud Storage.
Dependencia de Datos (Agentes IA): El Agente Preventivo necesita volumen de información para detectar estancamientos. Es mandatorio estructurar las colecciones en Cloud Firestore desde el primer sprint para comenzar a capturar la escala RPE y el cumplimiento, garantizando que el agente tenga datos para operar al cierre de las 4 semanas.
Dependencia de Seguridad (RBAC): La implementación del Modo Cuidador (incluido en el MVP) depende críticamente de la correcta configuración de los roles de acceso en Firebase Authentication. Se debe garantizar a nivel de código que el cuidador tenga un token de Solo Lectura, impidiendo cualquier modificación a las rutinas asignadas por la IA.


Riesgos y Ambigüedades Detectadas
Ambigüedades en los Documentos
Riesgo de Frustración por "Rachas Punitivas":
Ambigüedad: Los sistemas fitness tradicionales penalizan al usuario si pierde un día de entrenamiento (romper la racha). Para un usuario +60, esto genera ansiedad y abandono si faltan por motivos de salud.
Resolución: Se elimina el concepto de "racha diaria consecutiva". El Agente Preventivo / Analytics medirá la constancia en base a un objetivo de "Días activos por semana". Se establece una ventana de tolerancia de 4 días de inactividad antes de que el Agente envíe una alerta amigable al Modo Cuidador, no como penalización, sino como monitoreo de salud.
Frecuencia de Proyecciones Temporales:
Ambigüedad: Recalcular el progreso futuro después de cada sesión puede generar fluctuaciones erráticas en los datos y confundir al adulto mayor.
Resolución: El Agente Preventivo calculará las proyecciones lineales (ej. mejora de flexibilidad) en lotes (batch) procesando los datos de Firestore estrictamente cada 7 días.
Riesgo Legal y Clínico (Osteoporosis / Lesiones):
Riesgo: Responsabilidad de la plataforma si un usuario con restricciones médicas no declaradas ejecuta un ejercicio contraindicado.
Resolución: Se implementará un Disclaimer (Renuncia de Responsabilidad) de aceptación obligatoria en el Onboarding. A nivel de base de datos, el Agente Wellness Coach tendrá un filtro estricto: si las etiquetas (tags) médicas del perfil chocan con los requerimientos del ejercicio, la IA bloqueará la asignación de esa rutina.
Alcance de Wearables (APIs de Salud):
Ambigüedad: Los documentos iniciales debatían si exigir o no el uso de relojes inteligentes (Google Fit, Terra API) para medir progreso.
Resolución: Dado el límite de 4 semanas, la integración con APIs externas queda oficialmente pospuesta para la Fase 2. El MVP dependerá exclusivamente de la entrada manual simplificada de la escala de fatiga (RPE) por parte del usuario.
Contradicción Crítica en los Niveles de Progresión:
Riesgo: La documentación base proponía usar esquemas de 8 a 16 niveles de dificultad (basados en calistenia), lo cual representa un riesgo de lesión severo y una carga cognitiva excesiva para la tercera edad.
Resolución: Se descartan los modelos comerciales externos. La biblioteca de ejercicios se reestructuró bajo un límite innegociable de máximo 3 a 4 niveles de progresión funcional segura (ej. Nivel 1: Sentado, Nivel 2: Con apoyo, Nivel 3: De pie asistido).




Riesgos Técnicos
Riesgo de abandono por brecha digital (Probabilidad: Alta)
Contexto: Los usuarios mayores (+60) pueden sentirse abrumados por interfaces saturadas de datos o configuraciones complejas.
Mitigación: Diseño de un onboarding guiado de máximo 5 pasos. La aplicación iniciará por defecto en un "Modo Simple" que oculta gráficos analíticos profundos, priorizando botones grandes (44x44 pt) y la rutina del día. Se incluirá un tutorial interactivo corto apoyado por el Agente Wellness Coach.
Riesgo de lesiones por ejercicios mal adaptados (Impacto: Crítico)
Contexto: Asignar una rutina genérica a un adulto mayor con condiciones preexistentes (osteoporosis, prótesis) puede derivar en lesiones graves.
Mitigación Automatizada: En lugar de un cuestionario pasivo, el Agente Wellness Coach procesa dinámicamente un screening de 5 preguntas obligatorias (ej. "¿Ha tenido cirugía de cadera/rodilla?", "¿Siente dolor articular agudo al estar de pie?"). Si el usuario marca alguna restricción, el Agente bloqueará automáticamente las progresiones de nivel 3 y 4, limitará la rutina de ejercicios sentados (Nivel 1) y desplegará un Disclaimer exigiendo validación médica.
Dependencia de conectividad para ejecución de rutinas (Riesgo: Medio)
Contexto: El usuario podría entrenar en áreas sin cobertura (parques, sótanos).
Mitigación Arquitectónica: Para mantener el tamaño de la app por debajo de los 50 MB, no se descargan videos pesados al dispositivo. Los videos se transmitirán por streaming desde Google Cloud Storage cuando haya red. Si el usuario está offline, el sistema mostrará ilustraciones o animaciones vectoriales ligeras cacheadas en la app. Además, el motor offline-first de Cloud Firestore garantizará que el marcado de series y la escala RPE se guardan localmente y se sincronicen de forma transparente al recuperar la conexión.


Información Faltante (No especificada en documentos)
Aspecto a Definir
Resolución Técnica y de Producto
Estrategia de Motivación (Gamificación Suave)
Sin competiciones ni rachas punitivas. El Agente Wellness Coach gestionará un sistema de refuerzo positivo basado en la salud funcional. Los hitos a celebrar serán alcanzables y empáticos (ej. "3 días activos esta semana", "10 sesiones de movilidad en silla completadas"), evitando comparaciones sociales que generen frustración.
Frecuencia de Respaldo de Datos (Backups)
Totalmente administrado por GCP. Se elimina la gestión manual de copias de seguridad de PostgreSQL. El MVP utilizará la función nativa Point-in-Time Recovery (PITR) de Cloud Firestore, que mantiene copias de seguridad automáticas continuas. Los datos se retendrán mientras el usuario esté activo (o hasta su solicitud de Derecho al Olvido).
Idiomas Soportados y Tono de la IA
Español Neutro. El sistema y los prompts (instrucciones base) de los Agentes de IA estarán configurados en español neutro con un tono respetuoso, claro y motivador (tratamiento de "usted" o tuteo respetuoso según la configuración del perfil), garantizando su comprensión en toda Latinoamérica.
Modo Oscuro (Dark Mode)
Pospuesto para Fase 2. Aunque clínicamente beneficia a usuarios con cataratas o fotofobia, implementar un cambio de tema completo duplica los tiempos de diseño UI. Para las 4 semanas del MVP, la aplicación se mantendrá en Tema Claro garantizando estrictamente el contraste de 4.5:1 (WCAG AA).



Conclusión del Análisis
El sistema "SeniorVital" es técnicamente viable y presenta una clara oportunidad de diferenciación en el mercado de la salud digital (HealthTech) gracias a su enfoque exclusivo en la tercera edad (+60), el "Modo Cuidador" y la generación de rutinas de bajo impacto (ejercicios sentados/asistidos).
Para garantizar el éxito del MVP de 4 semanas y cumplir con la arquitectura Cloud-Native, los principales esfuerzos de ingeniería deben concentrarse en:
Diseño UX Ultra-Accesible (ISO 25010): Cumplimiento estricto de los estándares gerontológicos y WCAG 2.1 AA (botones de 44x44 pt, contraste de 4.5:1 y prevención de errores por motricidad fina).
Integración del Ecosistema Agéntico: Implementación del Agente Wellness Coach como núcleo seguro de asignación de rutinas (con un límite estricto de 3 a 4 niveles de progresión y filtro inquebrantable de restricciones médicas), apoyado por el Agente Preventivo / Analytics para el monitoreo y alertas de salud.
Arquitectura NoSQL y Nube (GCP): Consolidación de un modelo de datos dinámico en Cloud Firestore, capaz de persistir el progreso de forma Offline-First y alimentar las proyecciones de la IA de manera eficiente y gratuita durante la fase de validación.
Recomendación de Despliegue (MVP): Se recomienda iniciar el desarrollo del MVP de 4 semanas enfocado en una biblioteca base de 10 ejercicios funcionales, seguimiento de la escala de fatiga (RPE) y un dashboard intuitivo. Esto permitirá validar la interacción de los Agentes de IA con un grupo piloto de usuarios mayores (+60), dejando formalmente la integración con Wearables (Google Fit/HealthKit) y modelos de Deep Learning (LSTM) para una Fase 2 de evolución del producto.

