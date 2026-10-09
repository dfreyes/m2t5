# Análisis de Redes y Sistemas de Recomendación

Módulo 2 - Taller 3

El objetivo principal es aplicar la teoría de grafos para analizar redes de interacción usuario-película y desarrollar modelos predictivos de recomendación colaborativa.

---

## 👤 Autor
* **Ing. Diego F. Reyes Y. Mgtr.**
* **Fecha:** Octubre 2026

---

## 🎯 Objetivos del Proyecto

1. **Análisis de Redes:**
   * Entender y aplicar la teoría de grafos para analizar los datos de interacciones entre usuarios y películas.
   * Identificar nodos centrales, comunidades y otras métricas de red utilizando `NetworkX`.
   * Visualizar la estructura del grafo e interpretar los resultados.

2. **Sistemas de Recomendación:**
   * Implementar sistemas de recomendación colaborativa utilizando la librería `Surprise`.
   * Evaluar diferentes algoritmos (SVD, KNN) y comparar métricas de rendimiento (RMSE, MAE).
   * Ajustar hiperparámetros para mejorar la precisión de las recomendaciones.

---

## 🛠️ Tecnologías y Materiales

* **Lenguaje:** Python 3.10+
* **Dataset:** [MovieLens 100K (ml-100k)](https://grouplens.org) (100,000 calificaciones de 943 usuarios en 1,682 películas).
* **Librerías Clave:**
  * **Análisis de Redes y Visualización:** `networkx`, `matplotlib`, `seaborn`
  * **Machine Learning & Recomendación:** `scikit-surprise`, `scikit-learn`, `pandas`, `numpy`

---

## 🚀 Estructura del Código

El proyecto se divide en dos fases principales dentro del Notebook:

### Parte 1: Análisis de Redes con NetworkX
* **Carga y Fusión de Datos:** Uso de `pandas` para unificar la matriz de interacciones (`u.data`) con el catálogo de películas (`u.item`).
* **Modelado del Grafo Bipartito:** Creación de una red disjunta donde los nodos son Usuarios (Capa 0) y Películas (Capa 1), conectados por aristas que representan la calificación (*rating*).
* **Métricas de Centralidad:** Cálculo de centralidad de grado para descubrir a los usuarios más activos y las películas más populares (*Star Wars*, *Contact*, *Fargo*).
* **Detección de Comunidades:** Aplicación del algoritmo jerárquico de **Girvan-Newman** para segmentar la red en comunidades basadas en la intermediación de aristas.

### Parte 2: Sistema de Recomendación con Surprise
* **Preparación del Dataset:** Configuración del `Reader` nativo de Surprise según la escala de 1 a 5 estrellas.
* **División de Datos:** Split balanceado (80% entrenamiento / 20% prueba).
* **Benchmark de Modelos:** Evaluación comparativa entre **SVD (Factorización de Matrices)** y **KNN Basic (Vecindarios)**.
* **Optimización (Hyperparameter Tuning):** Uso de `GridSearchCV` para encontrar la mejor combinación de factores latentes, épocas y regularización para el SVD.
* **Predicción Personalizada:** Generación de un **Top 5 de recomendaciones** exclusivas para películas que el usuario no ha visto.

---

## 📈 Resultados Destacados

### 1. Centralidad de Red (Top Películas)
Las películas con mayor centralidad de grado (más calificadas) fueron:
1. *Star Wars (1977)*
2. *Contact (1997)*
3. *Fargo (1996)*

### 2. Comparativa de Modelos de Recomendación

| Algoritmo | RMSE (Menor es mejor) | MAE (Menor es mejor) |
| :--- | :---: | :---: |
| **SVD** (Optimizado) | **0.9245** | **0.7280** |
| **KNN Basic** | 1.0194 | 0.8038 |

> **Conclusión:** El modelo SVD optimizado mediante la cuadrícula de parámetros logró reducir el error cuadrático medio (RMSE) de manera significativa, demostrando ser el más eficiente para capturar las relaciones latentes entre usuarios y películas.

---

2. Descarga el dataset MovieLens 100k y colócalo en la carpeta raíz con el nombre `ml-100k/`.
---

## 📁 Metadatos y Esquema de Datos

Para este proyecto se utilizó el dataset clásico **MovieLens 100K**, el cual presenta una matriz esparcida de interacciones con una densidad de aproximadamente el **6.37%**. A continuación, se detallan los metadatos y el modelo de datos unificado mediante `pandas`:

### 📄 Perfil del Dataset
* **Cardinalidad de Nodos de Usuario (|U|):** 943 usuarios únicos.
* **Cardinalidad de Nodos de Película (|V|):** 1,664 películas calificadas (de un catálogo total de 1,682).
* **Total de Aristas (Interacciones):** 99,693 aristas cargadas en la red bipartita.
* **Granularidad:** Un registro por cada calificación emitida por un usuario a una película.

### 📊 Diccionario de Datos

El algoritmo procesa y fusiona de forma limpia las siguientes tablas utilizando tipos de datos optimizados:

#### 1. Matriz de Calificaciones (`u.data`)

| Campo | Tipo | Descripción |
| :--- | :---: | :--- |
| **`user_id`** | `int64` | Identificador único y anónimo del usuario (Rango: 1 a 943). |
| **`movie_id`** | `int64` | Identificador único de la película (Rango: 1 a 1682). |
| **`rating`** | `int64` | Escala ordinal del 1 al 5 (1: Mala, 5: Excelente). |
| **`timestamp`** | `int64` | Código de tiempo Unix (segundos) en que se emitió el voto. |

#### 2. Catálogo de Películas (`u.item`)

| Campo | Tipo | Descripción |
| :--- | :---: | :--- |
| **`movie_id`** | `int64` | Clave primaria que conecta con la matriz de calificaciones. |
| **`movie_title`**| `string`| Nombre oficial de la película con su año de estreno (Codificación `latin-1`). |

---
