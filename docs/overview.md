# Visión General del Proyecto GeoApps

## 🌍 Introducción
GeoApps es una plataforma de inteligencia geoespacial (GEOINT) diseñada para democratizar el acceso al análisis de imágenes satelitales. Utilizando técnicas avanzadas de Visión por Computadora y Aprendizaje No Supervisado (K-Means Clustering), la plataforma permite a usuarios no técnicos segmentar y clasificar automáticamente coberturas terrestres a partir de imágenes aéreas o satelitales.

## 🎯 Propósito y Alcance
El objetivo principal es reducir la barrera de entrada para el análisis territorial, proporcionando una herramienta web rápida, visual y accesible que elimina la necesidad de software GIS de escritorio pesado y costoso.

### Casos de Uso
-   **Agricultura de Precisión:** Identificación de zonas de cultivo vs. suelo desnudo.
-   **Monitoreo Ambiental:** Detección de cuerpos de agua y seguimiento de sequías.
-   **Planificación Urbana:** Análisis de expansión urbana y zonas verdes.

## 🛠️ Stack Tecnológico (Arquitectura Moderna)

### Frontend (Experiencia de Usuario)
-   **Next.js 16 (App Router):** Framework React para renderizado híbrido y optimización de SEO.
-   **TypeScript:** Tipado estático para robustez y mantenibilidad.
-   **Tailwind CSS 4:** Sistema de diseño utility-first para una UI moderna y responsiva.
-   **Leaflet / React-Leaflet:** Motor de mapas interactivos ligero y extensible.

### Backend (API & Orquestación)
-   **Next.js API Routes:** Serverless functions que actúan como gateway y orquestador.
-   **Node.js Runtime:** Gestión de I/O asíncrono y manejo de archivos.

### Core de Procesamiento (Data Science)
-   **Python 3:** Lenguaje estándar en la industria para ciencia de datos.
-   **OpenCV (cv2):** Librería de visión por computadora de alto rendimiento.
-   **Scikit-Learn / NumPy:** Algoritmos matemáticos y de clustering.
-   **Matplotlib:** Generación de visualizaciones analíticas estáticas.

## 📦 Estado del Proyecto: Estable (v1.0)
El sistema ha superado la fase de prototipo y se encuentra en una versión estable lista para producción.

### Características Implementadas
-   ✅ **Segmentación Automática:** Algoritmo K-Means dinámico (2-10 clusters).
-   ✅ **Visualización Dual:** Comparativa lado a lado (Original vs. Procesada).
-   ✅ **Dashboard Analítico:** Gráficos de distribución porcentual de cobertura.
-   ✅ **Seguridad Robusta:** Protección contra RCE, Path Traversal y DoS.
-   ✅ **Arquitectura Desacoplada:** Comunicación eficiente entre Node.js y Python.

### Limitaciones Conocidas
-   **Persistencia:** Los análisis son efímeros (no se guardan en BD).
-   **Escalabilidad:** El procesamiento es síncrono por petición (adecuado para tráfico bajo/medio).
