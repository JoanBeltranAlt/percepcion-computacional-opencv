# Percepción Computacional · Práctica con OpenCV

![Python](https://img.shields.io/badge/Python-3.x-blue)
![OpenCV](https://img.shields.io/badge/OpenCV-5.0.0-green)
![NumPy](https://img.shields.io/badge/NumPy-2.5-orange)
![Colab](https://img.shields.io/badge/Google%20Colab-ready-yellow)

Práctica guiada de la **Semana 7** del CADI Percepción Computacional
(experiencia IMAGEN). Recorre las operaciones básicas de OpenCV —carga,
inspección como matriz, conversión de color, redimensionamiento, recorte,
filtrado, umbralización, detección de bordes y búsqueda de contornos— sobre
**dos imágenes de naturaleza opuesta**, y compara qué resultados da el mismo
código en cada una.

**Repositorio:** https://github.com/JoanBeltranAlt/percepcion-computacional-opencv
**Entorno:** Google Colab

[![Abrir en Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/JoanBeltranAlt/percepcion-computacional-opencv/blob/main/Practica_OpenCV_Semana7.ipynb)

---

## Objetivo de la práctica

Comprender cómo una imagen deja de ser un elemento visual y pasa a ser una
matriz de datos que un programa puede procesar, y aplicar sobre ella las
operaciones fundamentales de la percepción computacional.

## Las dos imágenes

| | Radiografía de tórax | Monitor de signos vitales |
|---|---|---|
| Tamaño | 800 × 1000 px | 880 × 900 px |
| Naturaleza | transiciones suaves entre tejidos | texto, líneas y recuadros |
| Nitidez (varianza del laplaciano) | 23,9 | 869,2 |
| Píxeles de borde (Canny 80/160) | 0,22 % | 4,53 % |
| Contornos con área > 100 | **0** | **96** |

El mismo código, sin cambiar un solo parámetro, da resultados opuestos. Esa
diferencia es el eje del trabajo.

## Lo que se encontró

Siguiendo el flujo del notebook guiado, la búsqueda de contornos sobre la
radiografía devolvió **18 contornos detectados y 0 que superaran el filtro de
área**. La causa no era un parámetro mal puesto: lo que devuelve Canny son
líneas abiertas, no regiones cerradas, y el área de una línea es casi nula (la
mayor medía 65,5 px²).

Bajar los umbrales de Canny no resuelve el problema —de 1.880 contornos apenas
3 superan el filtro—, así que hubo que cambiar de estrategia: buscar sobre la
máscara de **Otsu invertida**, que sí produce regiones cerradas. Hay que
invertirla porque en una radiografía la región de interés es la zona oscura.
Así aparecen 7 regiones, y las dos mayores (145.525 y 111.862 px²) corresponden
a los **campos pulmonares**.

## Contenido

| Archivo | Descripción |
|---|---|
| `Practica_OpenCV_Semana7.ipynb` | Notebook con la práctica completa |
| `imagen_practica.jpg` | Radiografía de tórax (imagen principal) |
| `imagen_reto.jpg` | Monitor de signos vitales (mini reto) |
| `resultados/` | Salidas de cada operación |

## Cómo ejecutarlo

Ábrelo en Colab con el botón de arriba y ejecuta las celdas en orden. El
notebook instala `opencv-python-headless`, que es la variante sin interfaz
gráfica recomendada para Colab. Sube tus propias imágenes o usa las del
repositorio.

## Referencias

- Canny, J. (1986). A computational approach to edge detection. *IEEE
  Transactions on Pattern Analysis and Machine Intelligence, PAMI-8*(6),
  679–698. https://doi.org/10.1109/TPAMI.1986.4767851
- Otsu, N. (1979). A threshold selection method from gray-level histograms.
  *IEEE Transactions on Systems, Man, and Cybernetics, 9*(1), 62–66.
  https://doi.org/10.1109/TSMC.1979.4310076
- Bradski, G. (2000). The OpenCV Library. *Dr. Dobb's Journal of Software
  Tools*.
- OpenCV. (2026). *OpenCV documentation* (Versión 5.0.0). https://docs.opencv.org/5.x/

---

Joan Schneider Beltrán Delgado · Universidad de Cundinamarca
