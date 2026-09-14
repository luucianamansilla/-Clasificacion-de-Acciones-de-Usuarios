# 🕹️ Sistema de Clasificación de Acciones en Videojuegos

Trabajo práctico correspondiente a la materia ** Procesamiento de prendizaje Automático**, dictada en la Tecnicatura en Ciencia de Datos e Inteligencia Artificial.

**Institución**: Instituto Tecnológico Beltrán - Avellaneda.
**Carrera**: Tecnicatura en Ciencia de Datos e Inteligencia Artificial.
**Materia**: Procesamiento de Aprendizaje Automático.
**Profesora**: Yanina Escudero.
**Estudiante**: Mansilla Luciana.
**Año académico**: 2º Año.

---

## 📌 ¿De qué trata este proyecto?

El objetivo de este trabajo es poner en práctica un enfoque de **Aprendizaje Supervisado** para poder clasificar, de forma automática, el comportamiento de los usuarios dentro de un videojuego. Para esto se toman como referencia dos datos por cada sesión de juego: la **acción realizada** y el **tiempo que duró** esa acción.

## 🎯 Qué se busca lograr

- Comprender cómo se aplica el aprendizaje supervisado a la clasificación de comportamientos.
- Detectar qué patrones son relevantes dentro de las acciones de un usuario en la plataforma.
- Traducir esos patrones en reglas condicionales (`if / elif / else`) programadas en Python.
- Poner a prueba dichas reglas con datos de ejemplo.
- Analizar qué tan preciso y útil resulta este enfoque.

## 📊 Datos utilizados como ejemplo

| Usuario | Acción              | Duración (segundos) | Resultado       |
|---------|---------------------|----------------------|-----------------|
| user01  | Combate              | 120                  | Victoria        |
| user02  | Exploración          | 300                  | Descubrimiento  |
| user03  | Interacción social   | 180                  | Mensaje enviado |
| user04  | Combate              | 90                   | Derrota         |
| user05  | Exploración          | 240                  | Sin hallazgos   |

## 🧠 Criterios de clasificación

Cada tipo de acción tiene su propia condición de corte según la duración:

- **Combate**
  - Más de 90 segundos → Victoria
  - 90 segundos o menos → Derrota

- **Exploración**
  - Más de 240 segundos → Descubrimiento
  - 240 segundos o menos → Sin hallazgos

- **Interacción Social**
  - 180 segundos o más → Mensaje enviado
  - Menos de 180 segundos → no hay parámetro

## 🐍 Script en Python

```python
def predecir_resultado(accion, duracion):
    if accion == "Combate":
        return "Victoria" if duracion > 90 else "Derrota"
    elif accion == "Exploración":
        return "Descubrimiento" if duracion > 240 else "Sin hallazgos"
    elif accion == "Interaccion Social":
        return "Mensaje enviado" if duracion >= 180 else "no hay parametro"

acciones_validas = ["Combate", "Exploración", "Interaccion Social"]

print("Ingresá la acción y los segundos (ej: Combate 120). Escribí 'salir' para terminar:\n")

while True:
    entrada = input(">> ").strip()

    if entrada.lower() == "salir":
        break

    try:
        # Se separan los dos datos directamente en las variables de la función
        minSeg = entrada.rsplit(' ', 1)

        # 1. Validación de cantidad de elementos
        if len(minSeg) < 2:
            raise ValueError("Escribiste mal la entrada: te faltó ingresar el tiempo o la acción.")

        if minSeg[0].strip() not in acciones_validas:
            raise ValueError(f"Escribiste mal la acción: '{minSeg[0].strip()}' no es una opción válida.")

        # Se envía todo de una a la función convirtiendo los segundos a entero
        resultado = predecir_resultado(minSeg[0].strip(), int(minSeg[1]))
        print(f"--> {resultado}\n")

    except ValueError as e:
        print(f"⚠️ {e}\n")
```

## ⚙️ Instrucciones para correrlo

Este script puede ejecutarse desde Jupyter Notebook, Google Colab, VS Code o directamente desde una terminal.

### Antes de empezar

- Necesitás tener instalado **Python 3** (se recomienda la versión 3.8 en adelante).
- No hace falta instalar ninguna librería adicional: todo el código usa funciones nativas de Python.

### Pasos a seguir

1. **Descargá o cloná el repositorio:**
   ```bash
   git clone https://github.com/luucianamansilla/-Clasificacion-de-Acciones-de-Usuarios.git
   cd -Clasificacion-de-Acciones-de-Usuarios
   ```

2. **Confirmá que Python esté instalado:**
   ```bash
   python3 --version
   ```

3. **Corré el archivo:**
   ```bash
   python3 clasificacion_acciones.py
   ```

4. **Interactuá con el programa desde la consola:**
   - Vas a tener que escribir una acción seguida de la duración en segundos, separadas por un espacio:
     ```
     >> Combate 120
     --> Victoria
     ```
   - Las únicas acciones que el programa reconoce son: `Combate`, `Exploración` e `Interaccion Social`.
   - Para cerrar el programa, escribí `salir`.

### Ejemplo de una sesión completa

```
Ingresá la acción y los segundos (ej: Combate 120). Escribí 'salir' para terminar:

>> Combate 120
--> Victoria

>> Exploración 300
--> Descubrimiento

>> Interaccion Social 180
--> Mensaje enviado

>> salir
```

## 💡 Reflexión final

### 1. ¿Cuáles reglas resultaron más efectivas para clasificar?

Al combinar el tipo de acción con su duración, se logró clasificar correctamente todos los registros de la tabla de ejemplo, ya que cada acción cuenta con un umbral de tiempo bien definido que determina el resultado esperado.

### 2. ¿Qué desventajas tiene clasificar solo con reglas fijas?

- **Poca flexibilidad en los umbrales**: si un usuario tiene una duración exactamente igual al límite (por ejemplo, 240 segundos en Exploración), el resultado puede no ser el más adecuado, ya que las comparaciones son estrictas.
- **Difícil de escalar**: a medida que se suman más acciones o combinaciones posibles, mantener todas las condiciones a mano se vuelve poco práctico.
- **Poca adaptabilidad**: si el juego cambia sus mecánicas (nuevas acciones, otros tiempos), hay que reescribir las reglas manualmente.

### 3. ¿Qué aportaría un modelo de Machine Learning más avanzado?

- **Árboles de decisión** (`DecisionTreeClassifier`): pueden detectar automáticamente cuáles son los mejores puntos de corte, sin necesidad de definirlos manualmente.
- **Más variables en juego**: se podrían sumar datos como el nivel del jugador o su experiencia previa, para lograr una clasificación más precisa.
- **Separación en entrenamiento y prueba** (`train/test split`): permite medir qué tan bien predice el modelo frente a usuarios que no fueron usados durante el entrenamiento.

## 📝 A modo de cierre

Trabajar con reglas fijas es un buen primer acercamiento para entender la lógica detrás de una clasificación, aunque un modelo entrenado con Machine Learning ofrecería mayor flexibilidad y precisión a medida que se suman más datos y casos distintos.
