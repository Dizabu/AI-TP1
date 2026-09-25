# TP 1: Optimización

Implementación en PyTorch de algoritmos de optimización (Descenso del Gradiente, RMSprop y Enjambre de Partículas) aplicados a tres funciones de prueba, con calibración de hiperparámetros, análisis de convergencia y comparación de resultados.

**Curso:** Reconocimiento de Patrones / Machine Learning — PARMA-Group, ITCR
**Autor(es):** _(completar nombres del grupo)_
**Fecha de entrega:** Domingo 20 de setiembre
**Modo de trabajo:** Grupo de 2–3 personas

---

## 1. Descripción

Este proyecto implementa y compara tres algoritmos de optimización sobre tres funciones objetivo definidas en el dominio $x_1, x_2 \in [-10, 10]$:

| Función | Descripción | Tipo |
|---|---|---|
| `f0(x, y) = x² + y²` | Esfera | Convexa |
| `f1(x, y)` | Ackley | No convexa, múltiples mínimos locales |
| `f2(x, y) = (x² + y − 11)² + (x + y² − 7)²` | Himmelblau | No convexa, 4 mínimos globales |

Los algoritmos a implementar son:

1. **Descenso del gradiente** (vanilla gradient descent)
2. **RMSprop**
3. **Optimización por enjambre de partículas (PSO)**

Todos ejecutados con `P = 25` iteraciones para el análisis exploratorio inicial, y hasta 50 iteraciones para las pruebas de convergencia con 10 corridas por función.

---

## 2. Estructura del repositorio

```
.
├── README.md
├── notebook.ipynb              # Implementación completa (entregable obligatorio)
├── docs/
│   ├── informe.tex             # Documento LaTeX (documentación externa)
│   ├── informe.pdf             # PDF generado a partir del .tex (entregable)
│   └── figuras/                # Gráficas exportadas (contour, curvas de aprendizaje, etc.)
├── src/
│   ├── functions.py            # f0, f1, f2 y sus gradientes en PyTorch
│   ├── gradient_descent.py     # Implementación vectorizada
│   ├── rmsprop.py               # Implementación vectorizada
│   ├── pso.py                   # Implementación vectorizada
│   ├── calibration.py           # Scripts de calibración con Optuna / W&B
│   └── utils.py                 # Meshgrid, contour plots, curvas de nivel, convergencia
├── results/
│   ├── tablas/                  # Tablas de convergencia (10 corridas x algoritmo x función)
│   └── mejores_hiperparametros.json
├── tests/
│   └── test_*.py                # Al menos 2 pruebas unitarias por algoritmo, documentadas
└── requirements.txt
```

> Ajusta esta estructura al entregable real: lo obligatorio según el enunciado es el **notebook de Jupyter** (implementación) y el **PDF generado por LaTeX** (documentación externa).

---

## 3. Requisitos

- Python ≥ 3.10
- PyTorch
- Optuna **o** Weights & Biases (para calibración de hiperparámetros)
- Matplotlib (meshgrid, contour, curvas de nivel y de aprendizaje)
- Jupyter Notebook
- Distribución LaTeX (para compilar `informe.tex` a PDF)

Instalación sugerida:

```bash
python -m venv venv
source venv/bin/activate       # En Windows: venv\Scripts\activate
pip install -r requirements.txt
```

`requirements.txt` sugerido:

```
torch
matplotlib
numpy
optuna
jupyter
```

---

## 4. Uso

### 4.1 Ejecutar el notebook

```bash
jupyter notebook notebook.ipynb
```

El notebook debe incluir, por cada método (Gradiente Descendente, RMSprop, PSO):

- Explicación teórica del método.
- Al menos **2 pruebas unitarias documentadas**.
- Gráficas `meshgrid` + `contour` de `f0`, `f1`, `f2`, identificando si son convexas, sus mínimos y posibles puntos/regiones silla, verificados con PyTorch.
- Proceso de calibración de hiperparámetros (Optuna o W&B) con gráficas de aprendizaje.
- Tabla de 10 corridas por función: iteraciones hasta convergencia (o indicar si no convergió).
- Promedio del valor de la función minimizada y promedio de iteraciones hasta convergencia.
- Gráfico de curvas de nivel con los puntos visitados en la mejor corrida, y su curva de aprendizaje.
- Comparativa final entre los tres algoritmos, con fuentes externas citadas.

### 4.2 Calibración de hiperparámetros

```bash
python src/calibration.py --algo rmsprop --funcion f1 --trials 50
```

Guarda los mejores valores encontrados en `results/mejores_hiperparametros.json`.

### 4.3 Correr pruebas unitarias

```bash
pytest tests/
```

### 4.4 Compilar el informe LaTeX

```bash
cd docs
latexmk -pdf informe.tex
```

---

## 5. Consideraciones importantes del enunciado

- **Mismos puntos iniciales:** los 10 puntos de partida usados para PSO deben ser exactamente los mismos usados para el descenso del gradiente y RMSprop, para que la comparación sea válida.
- **Documentación interna:** seguir el estándar de Doxygen ([guía de referencia](https://tinyurl.com/55hxcd7r)) en el código fuente.
- **Documentación externa:** debe entregarse como PDF generado desde LaTeX, no como PDF exportado directamente del notebook.
- **Entrega:** digital, vía TEC-digital, antes del domingo 20 de setiembre.

---

## 6. Rúbrica resumida (100 puntos)

| Sección | Puntos |
|---|---|
| 1. Graficación y análisis de convexidad/mínimos/puntos silla | 20 |
| 2. RMSprop (calibración, pruebas, comparación con gradiente descendente) | 40 |
| 3. Enjambre de partículas (calibración, pruebas, comparación) | 30 |
| 4. Comparación final entre todos los algoritmos con fuentes citadas | 10 |

---

## 7. Preguntas conceptuales a responder en el informe

- ¿Por qué RMSprop es más efectivo que el descenso del gradiente para evitar quedar atascado en puntos silla?
- ¿Cómo se podría combinar el descenso del gradiente con *simulated annealing*? ¿Qué beneficios traería?

---

## 8. Licencia / Créditos

Trabajo práctico académico — ITCR, Escuela de Ingeniería en Computación, PARMA-Group. Uso exclusivamente educativo.
