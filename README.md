

---
 Líneas 1–2: Importar librerías

```python
import bpy
import math
```

* `import bpy` → Importa la librería de **Blender** para poder crear y modificar objetos en la escena.
* `import math` → Importa funciones matemáticas (como seno, coseno y conversión a radianes).

---

Líneas 4–6: Limpiar la escena

```python
# Limpiar escena
bpy.ops.object.select_all(action='SELECT')
bpy.ops.object.delete()
```

* `select_all(action='SELECT')` → Selecciona todos los objetos que haya en la escena.
* `delete()` → Borra todos los objetos seleccionados.
Básicamente deja la escena vacía para empezar desde cero.

---

 Líneas 8–11: Parámetros de la figura

```python
radio = 3
angulo_actual = 0
paso_angular = 60  # Cada 60 grados para obtener 6 círculos alrededor
```

Aquí defines variables importantes:

* `radio = 3` → Distancia desde el centro hacia donde se colocarán los círculos.
* `angulo_actual = 0` → Ángulo inicial (empieza en 0 grados).
* `paso_angular = 60` → Cada iteración aumentará 60°.
  Como 360 ÷ 60 = 6 → se crearán 6 círculos alrededor.

---

 Línea 14–15: Círculo central

```python
bpy.ops.mesh.primitive_circle_add(radius=radio, location=(0, 0, 0), vertices=64)
```

Crea un círculo en el centro:

* `radius=radio` → Radio del círculo (3).
* `location=(0, 0, 0)` → Posición en el centro del plano.
* `vertices=64` → Cantidad de vértices (más vértices = más redondo).

---

 Línea 19: Inicio del ciclo while

```python
while angulo_actual < 360:
```

Este ciclo se ejecuta mientras el ángulo sea menor a 360 grados.
Se repetirá 6 veces (0, 60, 120, 180, 240, 300).

---

Líneas 21–22: Calcular posición usando coordenadas polares

```python
x = radio * math.cos(math.radians(angulo_actual))
y = radio * math.sin(math.radians(angulo_actual))
```



1. `math.radians(angulo_actual)` → Convierte grados a radianes (porque seno y coseno trabajan en radianes).
2. `math.cos(...)` → Calcula la posición en X.
3. `math.sin(...)` → Calcula la posición en Y.
4. Multiplica por `radio` para que el punto esté a distancia 3 del centro.

Esto convierte coordenadas polares (radio + ángulo) en coordenadas cartesianas (x, y).

---

Línea 25: Crear el círculo en la nueva posición

```python
bpy.ops.mesh.primitive_circle_add(radius=radio, location=(x, y, 0), vertices=64)
```

Crea un círculo en la posición calculada:

* `location=(x, y, 0)` → Lo coloca alrededor del centro.
* `z = 0` → Todos están en el mismo plano.

Así se forman los círculos alrededor del central.

---

 Línea 28: Actualizar el ángulo

```python
angulo_actual += paso_angular
```

Esto es clave.

* Aumenta el ángulo en 60°.
* Evita que el ciclo sea infinito.
* Permite que los círculos se distribuyan uniformemente.

---

 Línea 30: Mensaje final

```python
print("Patrón completado con éxito.")
```

Imprime en la consola de Blender un mensaje cuando termina el patrón.

---
<img width="1365" height="549" alt="image" src="https://github.com/user-attachments/assets/3d75b3cf-6fa5-4e75-b9ec-6b94366055bf" />

---
 1.- El `while` crea varios círculos

Este pedazo es el responsable:

```python
while angulo_actual < 360:
```

Como:

* Empieza en `0`
* Aumenta de `60` en `60`
* Se detiene antes de `360`

Los valores serán:

```
0°
60°
120°
180°
240°
300°
```

Eso significa que se ejecuta **6 veces**
Y cada vez crea un círculo nuevo

---

2.- Pero además tienes el círculo central

Antes del `while` haces esto:

```python
bpy.ops.mesh.primitive_circle_add(radius=radio, location=(0, 0, 0), vertices=64)
```

Entonces tienes:

* 1 círculo en el centro
* 6 círculos alrededor

Total = **7 círculos**

---

3.- Entonces… ¿por qué parece que hay MUCHÍSIMOS?

Porque el radio del círculo es:

```python
radio = 3
```

Y también usas ese mismo radio para:

* El tamaño del círculo
* La distancia desde el centro

Eso hace que todos los círculos:

* Sean grandes
* Se superpongan
* Se encimen uno sobre otro

Por eso visualmente parece una esfera llena de líneas 

---

 Lo que realmente está pasando

Cada círculo tiene:

```python
vertices=64
```

Eso significa que cada círculo tiene 64 segmentos.

Entonces:

```
7 círculos × 64 vértices = 448 vértices
```

Y Blender dibuja todas esas líneas.

Por eso ves muchas líneas negras.

---
 Si quieres que se vea más claro

Puedes separar tamaño y distancia así:

```python
radio_circulo = 1
distancia = 4
```

Y cambiar:

```python
x = distancia * math.cos(math.radians(angulo_actual))
y = distancia * math.sin(math.radians(angulo_actual))

bpy.ops.mesh.primitive_circle_add(radius=radio_circulo, location=(x, y, 0), vertices=32)
```

Así:

* Los círculos serán pequeños
* Estarán más separados
* Se verá más como una flor 

---
<img width="1365" height="514" alt="image" src="https://github.com/user-attachments/assets/3d4a943c-10fa-4d79-b979-21f19457cadb" />

---
Ahora salen **menos círculos** por un cambio muy específico en tu código.
Mira esta línea:

```python
paso_angular = 80
```

Antes tenías:

```python
paso_angular = 60
```

---

 Cómo afecta eso al `while`

El ciclo es:

```python
while angulo_actual < 360:
```

Y el ángulo va aumentando según `paso_angular`.

---

Antes (paso de 60°)

Los valores del ángulo eran:

```
0°
60°
120°
180°
240°
300°
```

Eso son **6 repeticiones**
6 círculos alrededor + 1 central = **7 círculos**

---

 Ahora (paso de 80°)

Los valores del ángulo son:

```
0°
80°
160°
240°
320°
```

Cuando intenta:

```
320° + 80° = 400°
```

Ya no cumple la condición:

```python
400 < 360  → FALSO
```

Entonces el ciclo se detiene.

Solo se ejecuta **5 veces**

---

##  Resultado final

| Paso angular | Círculos alrededor | Total con el central |
| ------------ | ------------------ | -------------------- |
| 60°          | 6                  | 7                    |
| 80°          | 5                  | 6                    |

---

##  Regla rápida para saber cuántos círculos saldrán

Puedes usar esta fórmula:

```
Número de círculos ≈ 360 ÷ paso_angular
```

Ejemplos:

* 360 ÷ 60 = 6 círculos
* 360 ÷ 45 = 8 círculos
* 360 ÷ 30 = 12 círculos

---

