# 🗺️ Diagrama UML de Casos de Uso (Mermaid.js)

![UML](https://img.shields.io/badge/Modelado-UML_2.5-blue)
![Render](https://img.shields.io/badge/Render-Nativo_en_GitHub-success)
![Enfoque](https://img.shields.io/badge/Enfoque-AI--First_Architecture-purple)

Este artefacto representa el modelo visual de comportamiento de la plataforma **SeniorVital**. Cumpliendo con los principios de desarrollo ágil moderno y *AI-Augmented Development*, este diagrama ha sido construido utilizando **Diagramas como Código (Diagram-as-Code)**, permitiendo su renderizado nativo y control de versiones transparente.

---

## 📊 1. Diagrama de Casos de Uso del Sistema

```mermaid
flowchart LR
    %% ==========================================
    %% DEFINICIÓN DE ESTILOS ARQUITECTÓNICOS
    %% ==========================================
    classDef human fill:#ffcc80,stroke:#e65100,stroke-width:2px,color:#000
    classDef ai fill:#ce93d8,stroke:#6a1b9a,stroke-width:2px,stroke-dasharray: 5 5,color:#000
    classDef external fill:#cfd8dc,stroke:#37474f,stroke-width:1px,color:#000
    classDef uc fill:#e3f2fd,stroke:#1565c0,stroke-width:2px,color:#000

    %% ==========================================
    %% COLUMNA IZQUIERDA: ACTORES INICIADORES
    %% ==========================================
    User((Usuario Final +60)):::human
    Familiar((Familiar/Cuidador)):::human
    Coach{{Agente Wellness}}:::ai
    Admin((Fisioterapeuta)):::human
    Preventivo{{Agente Preventivo}}:::ai

    %% ==========================================
    %% COLUMNA CENTRAL: LÍMITE DEL SISTEMA
    %% ==========================================
    subgraph SeniorVital [Límite del Sistema: Plataforma SeniorVital]
        direction TB
        CU01(["CU01: Registro y Onboarding<br>«Usabilidad, Seguridad»"]):::uc
        CU02(["CU02: Generar Rutina Diaria<br>«Eficiencia Desempeño: <2s»"]):::uc
        CU03(["CU03: Registrar Ejecución (RPE)<br>«Fiabilidad: Offline-First»"]):::uc
        CU04(["CU04: Proyecciones Temporales<br>«Adecuación Funcional»"]):::uc
        CU05(["CU05: Dashboard de Progreso<br>«Usabilidad: WCAG 2.1 AA»"]):::uc
        CU06(["CU06: Modo Cuidador/Familiar<br>«Seguridad: Read-Only»"]):::uc
        CU07(["CU07: Gestionar Usuarios y Rutinas<br>«Interoperabilidad»"]):::uc
        CU08(["CU08: Biblioteca de Ejercicios<br>«Mantenibilidad: NoSQL tags»"]):::uc
        CU09(["CU09: Detección de Estancamiento<br>«Adecuación Funcional»"]):::uc
        CU10(["CU10: Recordatorios Proactivos<br>«Usabilidad: No punitivos»"]):::uc
        CU11(["CU11: Registro de Hábitos<br>«Mantenibilidad: Fase 2 Ready»"]):::uc

        %% Enlaces invisibles para forzar apilamiento vertical perfectamente ordenado
        CU01 ~~~ CU02 ~~~ CU03 ~~~ CU04 ~~~ CU05 ~~~ CU06 ~~~ CU07 ~~~ CU08 ~~~ CU09 ~~~ CU10 ~~~ CU11
    end

    %% ==========================================
    %% COLUMNA DERECHA: INFRAESTRUCTURA (GCP)
    %% ==========================================
    Auth([Firebase Auth]):::external
    Firestore[(Cloud Firestore)]:::external
    Storage[(GCP Storage)]:::external
    FCM([Firebase Messaging]):::external

    %% ==========================================
    %% INTERACCIONES: ACTORES -> CASOS DE USO
    %% ==========================================
    User ---> CU01
    User ---> CU03
    User ---> CU05
    User ---> CU11

    Coach ---> CU02
    Coach ---> CU09

    Familiar ---> CU01
    Familiar ---> CU06

    Admin ---> CU07
    Admin ---> CU08

    Preventivo ---> CU04
    Preventivo ---> CU05
    Preventivo ---> CU06
    Preventivo ---> CU09
    Preventivo ---> CU10

    %% ==========================================
    %% DEPENDENCIAS (INCLUDES / EXTENDS)
    %% ==========================================
    CU02 -.->|<< include >>| CU01
    CU03 -.->|<< extend >>| CU02
    CU04 -.->|<< include >>| CU03

    %% ==========================================
    %% INTERACCIONES: CASOS DE USO -> INFRAESTRUCTURA
    %% ==========================================
    CU01 -.->|Valida Token| Auth
    CU06 -.->|Verifica RBAC| Auth

    CU01 -.->|Guarda Perfil| Firestore
    CU02 -.->|Lee Tags| Firestore
    CU03 -.->|Guarda RPE| Firestore
    CU11 -.->|Persiste Hábito| Firestore

    CU02 -.->|Streaming Video| Storage
    CU08 -.->|Sube Demostración| Storage

    CU10 -.->|Dispara Push| FCM
