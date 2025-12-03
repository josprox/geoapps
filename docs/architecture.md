# Arquitectura del Sistema

## Diagrama de Flujo de Datos

### Diagrama de Componentes (Estructura)
```mermaid
graph TD
    subgraph Client [Cliente]
        User((Usuario))
        Browser[Navegador Web]
    end

    subgraph Server [Servidor Next.js]
        Frontend["Frontend (/satellite)"]
        API["API Route (/api/process-image)"]
    end

    subgraph Backend [Procesamiento]
        Python["Worker Python (OpenCV)"]
        FS[("Sistema de Archivos (temp/)")]
    end

    User -->|Interacción| Browser
    Browser -->|Upload| Frontend
    Frontend -->|HTTP POST| API
    API -->|Spawn Process| Python
    API -->|Read/Write| FS
    Python -->|Read/Write| FS
```

### Diagrama de Secuencia (Flujo de Datos)
```mermaid
sequenceDiagram
    participant U as Usuario
    participant FE as Frontend
    participant API as API Route
    participant FS as FileSystem
    participant PY as Python Script

    U->>FE: Sube Imagen
    FE->>API: POST /api/process-image
    activate API
    API->>FS: Guarda imagen temporal
    API->>PY: Ejecuta proceso
    activate PY
    PY->>FS: Lee imagen
    PY->>PY: K-Means Clustering
    PY->>FS: Guarda imagen análisis
    PY-->>API: Retorna JSON (stdout)
    deactivate PY
    API->>FS: Lee imagen análisis
    API->>FS: Limpieza (borra temporales)
    API-->>FE: Respuesta JSON + Base64
    deactivate API
    FE-->>U: Muestra Dashboard
```

## Componentes Principales

### 1. Cliente (Browser)
-   Responsable de la interacción con el usuario.
-   Maneja la selección de archivos y parámetros (número de clusters).
-   Visualiza los resultados (imagen original vs. procesada).

### 2. Servidor API (Next.js)
-   Actúa como orquestador.
-   Recibe la petición HTTP.
-   Gestiona el almacenamiento temporal de archivos.
-   Invoca el proceso hijo (Python).
-   Transforma la salida para el cliente.

### 3. Motor de Procesamiento (Python)
-   Ejecuta la lógica pesada de visión por computadora.
-   Utiliza `OpenCV` para el algoritmo K-Means.
-   Utiliza `Matplotlib` para generar visualizaciones estáticas (dashboards).

## ⚠️ Puntos de Dolor Arquitectónicos

1.  **Acoplamiento Fuerte:** La API depende de la estructura exacta de salida (stdout) del script de Python. Cualquier cambio en un `print` puede romper la API.
2.  **Escalabilidad Limitada:**
    -   El procesamiento es síncrono y bloqueante a nivel de CPU.
    -   Cada petición levanta una nueva instancia del intérprete de Python, lo cual es costoso en memoria y tiempo de arranque.
3.  **Estado Efímero:** No hay base de datos. El historial de análisis se pierde al recargar o limpiar la carpeta temporal.
