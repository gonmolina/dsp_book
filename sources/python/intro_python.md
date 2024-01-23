---
jupytext:
  encoding: '# -*- coding: utf-8 -*-'
  formats: md:myst,ipynb
  text_representation:
    extension: .md
    format_name: myst
    format_version: 0.13
    jupytext_version: 1.16.1
kernelspec:
  display_name: Python 3 (ipykernel)
  language: python
  name: python3
---

# Introducción a Python

Este cuaderno usará Python como lenguaje de programación. La mayor parte de lo aprendido en este curso puede ser desarrollado y aplicado en cuadernos. Los cuadernos se dividen en celdas donde se puede poner código y luego correrlo haciendo **Mayúscula + Enter**.

Aquí intenta ase intentará mostrarles algunos de los tipo de datos más comunes de Python con el fin de comenzar a interiorizarnos tanto con el lenguaje como con los cuadernos de de Jupyter.

+++

## Números

Python cuenta con varios formatos o tipos de datos para representar números.

### Enteros

Cuando una nueva variable numérica se define Python por defecto lo hace como entero. Por ejemplo:

```{code-cell} ipython3
num = 1
type(num)
```

O podemos pasar  un númeor flotante a entero (con perdidas de información):

```{code-cell} ipython3
num = int(1.6)
type(num)
```

```{code-cell} ipython3
print(num)
```

### Punto flotante

En caso de necesitar números con decimales que no pueden ser representados mediante enteros, Python automáticamente los transforma en números punto flotantes, o `float`:

```{code-cell} ipython3
num = 1.1
type(num)
```

En caso de necesitar que un número entero sea representado como un flotante, se puede resolver agregando un punto detrás de entero, por ejemplo:

```{code-cell} ipython3
num = 1.
type(num)
```

Otra forma de hacerlo, mucho menos común es:

```{code-cell} ipython3
num = float(1)
type(num)
```

### Números complejos

Python tiene soporte nativo para números complejos usando la letra `j` de la siguiente manera:

```{code-cell} ipython3
num = 1+2j
print('La variable num toma el siguiente valor: ', num)
print('La variable num es del tipo: ', type(num))
```

Python soporta ciertas operaciones de forma nativa con los números complejos:

```{code-cell} ipython3
a = 1 + 2j
b = 2 + 5.8j
print(' La suma de a + b es: ', a+b)
```

```{code-cell} ipython3
print('El conjugado de a vale: ', a.conjugate())
```

Fácilmente podemos obtener la parte real e imaginaria

```{code-cell} ipython3
print('La parte real de a es ', a.real, ' y la parte imaginaria es ',
      a.imag)
```

Notar la diferencia entre el **método** `conjugate` y la **propiedad** `real` y la **propiedad** `imag`. Mientras que `conjugate` se comporta como una función propia de número complejo y debemos llamarla usando paréntesis, `imag` y `real` se comportan como datos o variables del número complejo.

+++

## Listas

Ejemplo de lista, se acumulan con un patron

```{code-cell} ipython3
inputs = [2, 1, 3, 2, 4, 3, 6]
result = []  # Comenzamos con una lista vacía
for i in inputs:  # Iteramos sobre la lista inputs
    if i < 4:  # si se cumple esta condición agregamos a result
        result.append(i**2)

result
```

Es común ver en Python incluya una sintaxis algo diferente del `for` y el `if` cuando se tiene que ejecutar una sola línea. El siguiente es un ejemplo con ambos cosas.

```{code-cell} ipython3
result = [i**2 for i in inputs if i < 4]
result.append(1)
result
```

## Diccionarios

Una vez familiarizado con las listas, vemos los diccionarios, que son otro tipo de contenedores ordenados.

```{code-cell} ipython3
lst = [1, 2, 3]
```

Podemos recuperar elementos de la lista mediante _indexado_

```{code-cell} ipython3
lst[1]
```

Un diccionario nos da un contenedor como una lista, pero los índices pueden ser mucho más generales, no solo números, sino cadenas o variables de cualquier tipo, como por ejemplo `sympy` (y una gran cantidad de otros tipos)

```{code-cell} ipython3
dic = {'a': 100, 2: 45, 100: 45}
print(dic['a'], dic[2])
```

```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: ''
tags: [raises-exception]
---
print(dic[0])
```

## Tuplas

Finalmente, otro contenedor es la tupla:

```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: ''
---
x = 1, 2, 3
a, b, c = x
d, e, f = 4, 5, 6
```

```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: ''
---
def f(x):
    Ca, Cb, Cc = x
    return Cb


k = f(x)
print(k)
```

```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: ''
---
li = [1, 2, 3, 4]
type(li)
```

+++ {"editable": true, "slideshow": {"slide_type": ""}}

Las tuplas son como listas, pero se crean con comas:

```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: ''
---
t = 1, 2, 3, 4
type(t)
```

+++ {"editable": true, "slideshow": {"slide_type": ""}}

En algunos casos, es útil usar paréntesis para agrupar tuplas, pero tenga en
cuenta que no son requerimientos en la sintaxis:

```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: ''
---
t2 = (1, 2, 3, 4)
type(t2)
```

+++ {"editable": true, "slideshow": {"slide_type": ""}}

Es importante entender que la coma, no los paréntesis forman tuplas:

```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: ''
---
only_one = (((((((1)))))))
type(only_one)
```

```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: ''
---
only_one = 1,
type(only_one)
```

```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: ''
---
len(only_one)
```

+++ {"editable": true, "slideshow": {"slide_type": ""}}

La única excepción a esta regla es que una tupla vacía se construye con `()`:

```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: ''
---
empty = ()
type(empty)
```

```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: ''
---
len(empty)
```

+++ {"editable": true, "slideshow": {"slide_type": ""}}

Las diferencias entre las tuplas y las listas son que las tuplas son
inmutables (no se pueden cambiar "inplace")

```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: ''
---
li.append(1) # la lista puede modificarse
li
```

+++ {"editable": true, "slideshow": {"slide_type": ""}}

Pero si ejecutamos

```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: ''
tags: [raises-exception]
---
t.append(1) # esto da error, ya que como no puede modificarse no existe el método append
```

+++ {"editable": true, "slideshow": {"slide_type": ""}}

### Expansión de la tupla

Una característica muy útil y general del operador de asignación en Python es que las tuplas se expandirán y asignarán en patrones coincidentes:

```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: ''
---
a, b = 1, 2
```

+++ {"editable": true, "slideshow": {"slide_type": ""}}

Esto es bastante sofisticado y puede manejar estructuras anidadas y
expandirse a listas:

```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: ''
---
[(a, b,), c, d] = [(1, 2), 1, 4]
[f, g, h] = [(1, 2), 1, 4]
```

```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: ''
---
f = 1, 2
print(f)
g = f, 3
print(g)
h = f[0], f[1], 3
print(h)
```

+++ {"editable": true, "slideshow": {"slide_type": ""}}

## El bucle for en Python

Como resumen en Python el bucle `for` lo que hace es recorrer los elementos de objeto del tipo "iterable". Por ejemplo una lista, un diccionario o un array de numpy son iterables.

Para una lista:

```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: ''
---
lista_variada = [1, 2, 3., "hola"]
for e in lista_variada:
    print(e)
```

+++ {"editable": true, "slideshow": {"slide_type": ""}}

Para un diccionario, va a devolver en cada iteración la `key` es decir:

```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: ''
---
mi_diccionario = {"el_1": "Primer item", 2: "segundo item", "3": 45*3.2}
```

```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: ''
---
for key in mi_diccionario:
    print("La key es de tipo ", type(key), "y es ", key)
    print("El items de tipo ", type(mi_diccionario[key]),
          "y es ", mi_diccionario[key])
```

+++ {"editable": true, "slideshow": {"slide_type": ""}}

Finalmente para un `array` de `numpy` me va a devolver cada elemento del
array

```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: ''
---
import numpy as np

n = np.linspace(1, 1000, 7)
for i in n:
    print("Elemento al cuadrado", i**2)
```

+++ {"editable": true, "slideshow": {"slide_type": ""}}

## Funciones

Las funciones en Python se definen a partir de la palabra calve `def`.
Pueden tomar argumentos posicionales, y argumentos con nombres y con valores por defecto. El siguiente es un ejemplo.

```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: ''
---
def cambio_de_escala(x, m=1, h=0):
    y = [i*m+h for i in x]
    return y
```

+++ {"editable": true, "slideshow": {"slide_type": ""}}

El argumento x es un argumento posicional y es obligatorio a la hora
de llamar a la función `cambio_de_escala`. Los otros dos, pueden estar o no presentes a la hora de llamar. Si no están presentes, usan el valor por defecto definido en la función.

```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: ''
---
x = [1, 2, 3, 4, 5]
x1 = cambio_de_escala(x)
print(x1)
```

+++ {"editable": true, "slideshow": {"slide_type": ""}}

Podemos ver que asumió que `m=0` y `h=1`. También podríamos llamarla a la
función de la siguiente manera:

```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: ''
---
x2 = cambio_de_escala(x, m=2, h=1)
print(x2)
```

+++ {"editable": true, "slideshow": {"slide_type": ""}}

O lo que es lo mismo:

```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: ''
---
x2 = cambio_de_escala(x, h=1, m=2)
print(x2)
```

+++ {"editable": true, "slideshow": {"slide_type": ""}}

O equivalentemente:

```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: ''
---
x2 = cambio_de_escala(x, 1, 2)
print(x2)
```

+++ {"editable": true, "slideshow": {"slide_type": ""}}

Pero si intentamos sin darle valores a `x` tendremos:

```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: ''
tags: [raises-exception]
---
x3 = cambio_de_escala(m=1, h=2)
print(x3)
```

+++ {"editable": true, "slideshow": {"slide_type": ""}}

## Paquetes y módulos de Python

En las próximas lineas utilizaré el alias `np` para `numpy` con el fin reducir lo que debo escribir.

Las bibliotecas de Python se organizan en paquetes que tienen a su vez uno o varios módulos y submódulos.

En este curso estaremos usando varios paquetes de Python que describiremos en las próximas secciones.

+++ {"editable": true, "slideshow": {"slide_type": ""}}

### Numpy

Este paquete una gran cantidad de herramientas relacionadas con cáculos numéricos y cuenta con distintos tipos de datos para representar la información numérica. 

## Tipos de datos de numpy

Estaremos hablando de ellos durante el curso, pero los tipos de datos más importante a mencionar:

- `ndarray` es un arreglo de una o más dimensiones cuyos datos son homogéneos, es decir todos del mismo tipo (todos enteros `int` o todos flotantes `np.float32` o todos dobles `np.float64`, etc.). Con este tipo de datos de de una dimensión podemos definir vectores, y de dos dimensiones podemos definir matrices
- `matrix` es una arreglo de 2 dimensiones especialmente diseños para representar matrices. Es menos eficiente que `ndarray` y su uso no es recomendado, aunque al principio puede ser cómodo. La principal diferencia reside en como están definidas las operaciones de multiplicación para uno y otro. Mientras que para que el tipo de datos `matrix` la operación `*` es la multiplicación de matrices como la realizamos normalmente en álgebra, para el tipo de datos `ndarray` la multiplicación con `*` es elemento a elemento. Si quiere realizar la operación multiplicación de matrices con el tipo de datos `ndarray` se debe usar el operador `@`.

Veamos un ejemplo:

```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: ''
---
import numpy as np

Aa=np.array([[1.,2.],[3.,4.]])
Ba=np.array([[5.,6.],[7.,8.]])

print("el tipo de datos de Aa es:", type(Aa), "\n")
# realizamos la operación multiplicación elemento a elemento
print("resultado multiplicación elemento a elemento:\n", A*B, "\n\n")

Am=np.matrix([[1.,2.],[3.,4.]])
Bm=np.matrix([[5.,6.],[7.,8.]])

print("el tipo de datos de Ama es:", type(Am), "\n")
# relizamos la operación multiplicación elemento a elemento
print("resultado multiplicación de matrices usando matrix:\n", Am*Bm, "\n\n")
print("resultado multiplicación de matrices usando ndarray:\n", Am@Bm)
```

+++ {"editable": true, "slideshow": {"slide_type": ""}}

### Módulos

El paquete `numpy` contiene varios módulos. Quizás para nosotros el más importante será el que respecta a algebra lineal, que se encuentra en `linalg`. Por ejemplo calculemos los autovalores de las matrices anteriores:

```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: ''
---
np.linalg.eigvals(Aa)
```

+++ {"editable": true, "slideshow": {"slide_type": ""}}

En este caso usé el tipo de datos `ndarray` pero el resultado con matrices hubiera sido el mismo.

+++ {"editable": true, "slideshow": {"slide_type": ""}, "raw_mimetype": ""}

Es util conocer sabes como se llaman los métodos que debemos utilizar. Para ayudarnos podemos utilizar las opciones de autocompletado de Jupyter. Por ejemplo tipeamos `np.linalg.` y luego presionando la tecla `TAB` el IDE nos mostrará todas las opciones que tenemos dentro del módulo `np.linalg`.

También en `jupyter` parándonos sobre una función o módulo podemos obtener la ayuda presionando `ctrl+I`.

En el Visual Studio Code las opciones de autocompletado y ayuda se deben ver mientras se está tipeando.

+++ {"editable": true, "slideshow": {"slide_type": ""}}

## Scipy

Para `scipy` utilizaré el alias `sc`

Otro paquete importante para hacer cálculos es `scipy`. Utiliza `numpy` debajo para hacer muchos de sus cálculos y se superpone en muchas casos, pero la cantidad de herramientas con las que cuenta es mucho mayor. En este curso estaremos usando principalmente los siguientes módulos:

- `interpolate`: utilidades para interpolación de datos
- `fft`: utilidades para obtener valores de la transformada de Fourier a partir de datos
- `stats` utilidades de estadística. Tiene generadores de números con distintas distribuciones.

Estaremos utilizándolos en lo que sigue de la clase

```{code-cell} ipython3
import scipy as sc

x= np.linspace(0,10, 11) # uso numpy que importé antes
y=x**2

f=sc.interpolate.interp1d(x, y)

urv = sc.stats.uniform() # una variable de distribución uniforme entre 0 y 1
x0= urv.rvs(1)*10 #  obtengo un valor aleatorio entre 0 y 10
y0 = f(x0)

print("el resultado de la interpolación de ", x0, " para los puntos (x,y) es: ", y0, "\n\n")
```

### Matplotlib

Es el paquete más utilizado de Python para realizar figuras. Hay un cuaderno tratando específicamente este paquete. La intención es que usen ese cuaderno como consulta cuando desean realizar figuras más detalladas.

Es importante poder utilizarlo con cierta destreza a este paquete, ya que en general la calidad de las figuras influyen fuertemente en la presentación del trabajo que se está realizando.

+++

### Sympy

Es el paquete de cálculo simbólico de Python. También pondré a disposición un cuaderno con ejemplos del uso básico de `sympy` ya que puede ser una herramienta útil para resolver algunos problemas en los que es necesario el cálculo simbólico.

```{code-cell} ipython3

```
