# AGI - Bloques de Construcción Neuronales: Detectores de Patrones, Secuencias y Contexto

Este repositorio documenta la investigación y los resultados obtenidos en el desarrollo de microcircuitos neuronales fundamentales para una futura Inteligencia Artificial General (AGI). El enfoque se basa en la creación de detectores especializados inspirados en la neurociencia computacional.

---

## Visión General

El objetivo de este proyecto es diseñar, simular y validar los bloques de construcción cognitivos más básicos. En lugar de utilizar redes neuronales monolíticas, proponemos un enfoque modular donde diferentes tipos de "neuronas detectoras" se especializan en reconocer patrones específicos en los datos de entrada. Estos detectores son la base para construir sistemas de procesamiento más complejos.

## Arquitectura de Detectores

Hemos definido y probado tres niveles de detectores jerárquicos:

### G1: Detector de Patrones (N_Detector_Patron)
- **Función:** Es la unidad más básica. Se activa cuando un conjunto específico de neuronas de entrada (un "patrón") dispara simultáneamente.
- **Mecanismo:** Implementa una forma de coincidencia de patrones. Su membrana solo alcanza el umbral de disparo si recibe estímulos de todas sus fuentes predefinidas en una ventana de tiempo muy corta.

### G2: Detector de Secuencias (N_Detector_Secuencia)
- **Función:** Reconoce secuencias temporales de activación de patrones. Por ejemplo, se activa solo si el Patrón A dispara, seguido por el Patrón B, y luego el Patrón C (A -> B -> C).
- **Mecanismo:** Utiliza una constante de tiempo de membrana (`tau_m`) cuidadosamente ajustada. La activación del primer patrón en la secuencia "pre-carga" la neurona, y las activaciones posteriores la empujan por encima del umbral. Si los patrones llegan en el orden incorrecto, el potencial de membrana decae antes de que se complete la secuencia.

### G3: Detector de Contexto (N_Detector_Contexto)
- **Función:** Es el nivel más abstracto. Se activa cuando un conjunto de secuencias específicas (detectadas por G2) ocurren en un contexto determinado. Por ejemplo, la secuencia A->B->C solo es relevante si la secuencia X->Y->Z también ha ocurrido recientemente.
- **Mecanismo:** Integra las salidas de múltiples detectores G2, actuando como un detector de "meta-patrones" temporales.

---

## Logro Clave: Aprendizaje Perfecto de Secuencias

Uno de los hitos más importantes de esta fase fue lograr un **aprendizaje perfecto y determinista** para un `N_Detector_Secuencia123` encargado de reconocer la secuencia de entrada P1 -> P2 -> P3.

Tras una extensa investigación, se identificaron los siguientes factores como críticos para el éxito:

1.  **Inhibición Selectiva:** La neurona detectora de secuencias (`N_Detector_Secuencia123`) **debe ser excluida** del circuito de inhibición general que resetea otras neuronas. Esto le permite mantener su potencial de membrana "cargado" mientras espera los siguientes elementos de la secuencia.

2.  **Constante de Tiempo de Membrana (`tau_m`) Específica:** Se requiere un valor de `tau_m = 10.0 ms` para esta neurona, a diferencia de otras que usan `7.0 ms`. Este valor es crucial para que el potencial decaiga a la velocidad justa para distinguir la secuencia correcta de las incorrectas.

3.  **Peso Sináptico Inicial:** Con los parámetros anteriores, un peso inicial de `0.5` en las sinapsis de entrada permite que la plasticidad sináptica (aprendizaje) alcance su máximo (`1.5`) de forma robusta.

Estos hallazgos se documentan en detalle en el archivo `docs/log_investigacion_inhibicion_G1.md`.

## Demostración

Para visualizar el comportamiento de un agente simple controlado por estos principios, se creó un entorno 2D. El agente aprende a buscar "comida" (puntos de activación).

**Evolución de la Red Neuronal:**

![Evolución de la red neuronal](media/Evolution_food.png)


**Agente en Acción:**

![Agente en acción](media/Food.gif)


