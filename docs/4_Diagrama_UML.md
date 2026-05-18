# 🗺️ Diagrama UML de Casos de Uso (Mermaid.js)

![UML](https://img.shields.io/badge/Modelado-UML_2.5-blue)
![Render](https://img.shields.io/badge/Render-Nativo_en_GitHub-success)
![Enfoque](https://img.shields.io/badge/Enfoque-AI--First_Architecture-purple)

Este artefacto representa el modelo visual de comportamiento de la plataforma **SeniorVital**. Cumpliendo con los principios de desarrollo ágil moderno y *AI-Augmented Development*, este diagrama ha sido construido utilizando **Diagramas como Código (Diagram-as-Code)**, permitiendo su renderizado nativo y control de versiones transparente.

---

## 📊 1. Diagrama de Casos de Uso del Sistema

```mermaid
flowchart LR
    %% ==========================================================
    %% DEFINICIÓN DE PALETA CROMÁTICA PROFESIONAL (ISO 25010)
    %% ==========================================================
    classDef human fill:#FFE0B2,stroke:#FB8C00,stroke-width:2px,color:#212121
    classDef ai fill:#E1BEE7,stroke:#8E24AA,stroke-width:2px,stroke-dasharray: 4 4,color:#212121
    classDef infrastructure fill:#ECEFF1,stroke:#546E7A,stroke-width:1.5px,color:#37474F
    classDef uc fill:#F1F8E9,stroke:#558B2F,stroke-width:2px,color:#1B5E20
    classDef ucAI fill:#E3F2FD,stroke:#1565C0,stroke-width:2px,color:#0D47A1
    classDef ucAdmin fill:#FFF9C4,stroke:#F57F17,stroke-width:2px,color:#5D4037

    %% ==========================================================
    %% ACTORES IZQUIERDA (HUMANOS Y AGENTES AUTÓNOMOS)
    %% ==========================================================
    subgraph Actores [Ecosistema de Actores]
        direction TB
        User((Usuario Final<br>Adulto Mayor +60)):::human
        Familiar((Familiar /<br>Cuidador)):::human
        Admin((Fisioterapeuta /<br>Administrador)):::human
        Coach{{🤖 Agente<br>Wellness Coach}}:::ai
        Preventivo{{🤖 Agente<br>Preventivo / Analytics}}:::ai
    end

    %% ==========================================================
    %% LÍMITE DEL SISTEMA (SEGMENTADO EN BLOQUES SEMÁNTICOS)
    %% ==========================================================
    subgraph SeniorVital [Límite del Sistema: Plataforma SeniorVital]
        direction TB

        subgraph Modulo_Paciente [Eje 1: Experiencia del Adulto Mayor]
            direction LR
            CU01(["CU01: Registro y Onboarding<br>«Usabilidad, Seguridad»"]):::uc
            CU03(["CU03: Registrar Ejecución (RPE)<br>«Fiabilidad: Offline-First»"]):::uc
            CU05(["CU05: Dashboard de Progreso<br>«Usabilidad: WCAG 2.1 AA»"]):::uc
            CU11(["CU11: Registro de Hábitos Manuales<br>«Mantenibilidad: Fase 2 Ready»"]):::uc
        end

        subgraph Modulo_IA [Eje 2: Core Autónomo e Inteligencia Artificial]
            direction LR
            CU02(["CU02: Generar Rutina Diaria<br>«Eficiencia: Latencia <2s»"]):::ucAI
            CU04(["CU04: Proyecciones Temporales<br>«Adecuación Funcional»"]):::ucAI
            CU09(["CU09: Detección de Estancamiento<br>«Adecuación Funcional»"]):::ucAI
            CU10(["CU10: Recordatorios Proactivos<br>«Usabilidad: No punitivos»"]):::ucAI
        end

        subgraph Modulo_Gestion [Eje 3: Control Clínico y Supervisión]
            direction LR
            CU06(["CU06: Modo Cuidador/Familiar<br>«Seguridad: Read-Only»"]):::ucAdmin
            CU07(["CU07: Gestión de Usuarios/Rutinas<br>«Interoperabilidad»"]):::ucAdmin
            CU08(["CU08: Biblioteca de Ejercicios<br>«Mantenibilidad: NoSQL tags»"]):::ucAdmin
        end
    end

    %% ==========================================================
    %% INFRAESTRUCTURA DERECHA (GOOGLE CLOUD & FIREBASE)
    %% ==========================================================
    subgraph Ecosistema_Cloud [Infraestructura GCP / Firebase]
        direction TB
        Auth([Firebase Auth]):::infrastructure
        Firestore[(Cloud Firestore NoSQL)]:::infrastructure
        Storage[(GCP Storage Buckets)]:::infrastructure
        FCM([Firebase Messaging FCM]):::infrastructure
    end

    %% ==========================================================
    %% ENLACES E INTERACCIONES DE ACTORES
    %% ==========================================================
    User ---> CU01
    User ---> CU03
    User ---> CU05
    User ---> CU11

    Familiar ---> CU01
    Familiar ---> CU06

    Admin ---> CU07
    Admin ---> CU08

    Coach ---> CU02
    Coach ---> CU09

    Preventivo ---> CU04
    Preventivo ---> CU05
    Preventivo ---> CU06
    Preventivo ---> CU09
    Preventivo ---> CU10

    %% ==========================================================
    %% RELACIONES INTERNAS (INCLUDES / EXTENDS)
    %% ==========================================================
    CU02 -.->|<< include >>| CU01
    CU03 -.->|<< extend >>| CU02
    CU04 -.->|<< include >>| CU03

    %% ==========================================================
    %% CONEXIONES DE PERSISTENCIA Y SERVICIOS NATIVOS CLOUD
    %% ==========================================================
    CU01 -.-> Auth
    CU06 -.-> Auth
    CU01 -.-> Firestore
    CU02 -.-> Firestore
    CU03 -.-> Firestore
    CU11 -.-> Firestore
    CU02 -.-> Storage
    CU08 -.-> Storage
    CU10 -.-> FCM
