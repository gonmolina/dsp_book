---
jupytext:
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

+++ {"editable": true, "slideshow": {"slide_type": "slide"}}

# FILTROS

+++ {"editable": true, "slideshow": {"slide_type": "slide"}}

## Temario

1. Transformada de Laplace
2. Funciones transferencias
3. Caracterización de los filtros
    1. Pasa-bajos
    2. Pasa-altos
    3. Pasa-banda
    4. Elimina-banda
    5. Pasa-todo
4. Tipos de filtros con LTI. Butterworth, Chebyshev I y II, Elípticos (Cauer), Bessel
5. Obtención de la función transferencia de un filtro con SciPy  (`sicpy.signal`)
6. Simulación del filtro en Python
7. Presentación de herramienta web para diseño de filtros electrónicos activos: [Web Bench Filter Design Tool](https://webench.ti.com/filter-design-tool/).

+++ {"editable": true, "slideshow": {"slide_type": "slide"}}

## Transformada de Laplace

+++ {"editable": true, "slideshow": {"slide_type": "notes"}}

Hasa el momento en la materia estuvimos trabajando con la transformada de Fourier.
A esta transformada la podemos pensar como la correlación de la señal que se desea transformar con senos y cosenos de diferentes frecuencias que hacemos variar entre $-\infty$ e $\infty$.

+++ {"editable": true, "slideshow": {"slide_type": "notes"}}

La principal desventaja de está transformada, por la características periódicas del seno y del coseno, es que no permite capturar las componentes transitorias de las señales.
Para salvar esta dificultad se define la transformada de Laplace.
Se define a transformada de Laplace $F(s)$ de una función definida para $t\in\mathbb R \ge 0$ como:

+++ {"editable": true, "slideshow": {"slide_type": "fragment"}}

$$F(s) = \int_{0}^{\infty} e^{-st}f(t)dt$$

La función de trasformación anterior se la conoce como transformada unilateral de Laplace.

+++ {"editable": true, "slideshow": {"slide_type": "slide"}}

Analizando la expresión anterior y sabiendo que la variable $s$ es un número complejo que podemos escribir como:

$$s=\sigma  \pm j\omega$$

Podemos notar que la transformada de Laplace es una generalización de la transformada de Fourier tomando $\sigma=0$ .

+++ {"editable": true, "slideshow": {"slide_type": "slide"}}

## Tabla de transformadas de Laplace

+++ {"editable": true, "slideshow": {"slide_type": "notes"}}

Para funciones típicas, existen tablas de transformada de Laplace que nos permiten rápidamente ir del dominio $t$ (en general tiempo) al dominio de Laplace $s$, que se lo conoce como dominio de frecuencia generalizada.

+++ {"editable": true, "slideshow": {"slide_type": "fragment"}}

![Tabla de transformadas de Laplace](bg2.png)

+++ {"editable": true, "slideshow": {"slide_type": "slide"}}

### Propiedades

La trasformada de Laplace tiene propiedades similares a la de la transformada de Fourier.

Siendo $\alpha$, $\beta$ $\in$ $\mathbb R$ y $f(t)$ $g(t)$ dos funciones definidas para $t \ge 0$ entonces la transformada de Laplace satisface las siguientes propiedades:

+++ {"editable": true, "slideshow": {"slide_type": "subslide"}}

#### Linealidad

$$\mathcal {L}\{\alpha f(t)+\beta g(t)\}=\alpha {\mathcal {L}}\{f(t)\}+\beta {\mathcal {L}}\{g(t)\}$$

+++ {"editable": true, "slideshow": {"slide_type": "subslide"}}

#### Primer teorema de traslación

Esta propiedad se obtiene aplicando la definición de transformada y a través del cambio de variable $u=s-a$.

 $$\mathcal {L}\{e^{at}f(t)\}=F(s-a)$$

+++ {"editable": true, "slideshow": {"slide_type": "subslide"}}

#### Segundo teorema de traslación

Esta propiedad se demuestra por definición y teniendo en cuenta la definición de la función escalón unitario

$$\mathcal {L}\{f(t-a)u(t-a)\}=e^{-as}\mathcal {L}\{f\}$$

+++ {"editable": true, "slideshow": {"slide_type": "subslide"}}

#### Transformada de una derivada de primer orden

$\mathcal{L}\left\{\dfrac{df(t)}{dt}\right\}=sF(s)-f(0)$

+++ {"editable": true, "slideshow": {"slide_type": "subslide"}}

#### Transformada de una derivada de segundo orden

- $\mathcal{L} \left\{ \dfrac{d^2f(t)}{dt^2} \right\}=s^2F(s)-sf(0)-f'(0)$

+++ {"editable": true, "slideshow": {"slide_type": "subslide"}}

#### Transformada de una derivada de orden n

$\mathcal{L} \left\{ \dfrac{d^nf(t)}{dt^n} \right\}= s^n F(s)- s^{n-1}f(0) - s^{n-2}f'(0) - \ldots - sf^{n-2}(0)-f^{n-1}(0)$

+++ {"editable": true, "slideshow": {"slide_type": "subslide"}}

#### Transformada de la Integral de una función

$$\mathcal{L} \left\{ \int_0^tf(\tau)d\tau \right\} =\dfrac{F(s)}{s}$$

+++ {"editable": true, "slideshow": {"slide_type": "subslide"}}

#### Valor final de una función: teorema de valor final

$$f(\infty)=\lim_{s\rightarrow 0}sF(s)$$

+++ {"editable": true, "slideshow": {"slide_type": "subslide"}}

#### Valor inicial de una función: teorema de valor inicial

$$f(0)=\lim_{s\rightarrow \infty}sF(s)$$

+++ {"editable": true, "slideshow": {"slide_type": "notes"}}

Usando la tabla anterior junto con las propiedades, podemos obtener rápidamente la transformada de Laplace de un conjunto muy grande de funciones temporales.

+++ {"editable": true, "slideshow": {"slide_type": "slide"}}

## Función transferencia

+++ {"editable": true, "slideshow": {"slide_type": "slide"}}

Para definir la función transferencia vamos a comenzar estudiando un ejemplo mecánico simple: un sistema lineal de una masa y un resorte. Vamos a estudiar la velocidad de la masa según una entrada fuerza aplicada a la misma.

![Sistema masa resorte](ejemplo_sis_mec.png)

+++ {"editable": true, "slideshow": {"slide_type": "slide"}}

Las ecuaciones que gobiernan el movimiento de la masa son principalmente las leyes de Newton:

$$f(t) - bv(t) - kx(t) = ma(t)$$

A esta misma ecuación la podemos expresar como:

$$ f(t) - b\frac{\text{d}x(t)}{dt} - kx(t) = m \frac{\text{d}^2x(t)}{dt^2} $$

donde $f(t)$ es la fuerza aplicada a la masa (entrada del sistema) y $x(t)$ es la posición de la masa (salida del sistema).

+++ {"editable": true, "slideshow": {"slide_type": "notes"}}

Obtener la respuesta temporal del sistema a partir de esta ecuación puede resultar complejo ya que requiere resolver la ecuación diferencial anterior. Por otro lado, es difícil de estudiar en forma genérica que pasa para diferentes funciones de la entrada $f(t)$.
Aquí es donde la transformada de Laplace comienza a ser de gran utilidad.

+++ {"editable": true, "slideshow": {"slide_type": "notes"}}

Suponiendo que existen las transformadas de Laplace de $f(t)$ y $x(t)$ y que las mismas son $F(s)$ y $X(s)$ respectivamente, podemos escribir la misma ecuación anterior en el dominio transformado de Laplace utilizando las propiedades vistas anteriormente:

+++ {"editable": true, "slideshow": {"slide_type": "slide"}}

$$ F(s) - b sX(s) - kX(s) = m s^2 X(s)$$

+++ {"editable": true, "slideshow": {"slide_type": "notes"}}

Podemos notar que pasamos de una ecuación diferencial en el dominio temporal a una ecuación algebraica en el dominio temporal. Además, a la ecuación anterior la podemos reescribir como:

+++ {"editable": true, "slideshow": {"slide_type": "fragment"}}

$$ \frac{X(s)}{F(s)} = \frac{1}{ms^2+bs+k} $$

+++ {"editable": true, "slideshow": {"slide_type": "fragment"}}

Esta última ecuación es la descripción del sistema escrita como la relación entre la salida y la entrada del sistema en el dominio transformado de Laplace. A esta realación de la conoce como **función transferencia** del sistema.

+++ {"editable": true, "slideshow": {"slide_type": "slide"}}

## Ejercicio. Función de transferencia (5 minutos)

Usando la tabla de propiedades, obtener la función transferencia del sistema si la salida es la velocidad $v(t)$

+++ {"editable": true, "slideshow": {"slide_type": "slide"}}

## Anti tranformada de Laplace de la función transferencia

+++ {"editable": true, "slideshow": {"slide_type": "notes"}}

Dijimos que la transformada de Laplace es, en general, la transformada de señales temporales.
Entonces, podríamos preguntarnos, ¿qué señal temporal es la antitransformada de la función transferencia?

Para responder a esta pregunta lo que haremos es analizar que excitación se necesita el sistema para que la salida sea, en el dominio de Laplace, la función transferencia. Dejando la salida n función de la entrada, tenemos que:

+++ {"editable": true, "slideshow": {"slide_type": "fragment"}}

$$X(s)=\frac{1}{ms^2+bs+k}F(s)$$

+++ {"editable": true, "slideshow": {"slide_type": "notes"}}

Si $F(s)$ es 1 entonces la respuesta del sistema es la función transferencia. Ahora, de la tablas de transformadas de Laplace podemos ver que la señal temporal cuya transformada es 1 es el impulso unitario. Por lo tanto tenemos que:

+++ {"editable": true, "slideshow": {"slide_type": "slide"}}

$$X_{\delta}(s)=\frac{1}{ms^2+bs+k}$$

+++ {"editable": true, "slideshow": {"slide_type": "notes"}}

donde $X_{\delta}(s)$ es la transformada de Laplace de la salida del sistema cuando la entada es un impulso unitario.

+++ {"editable": true, "slideshow": {"slide_type": "fragment"}}

Por lo tanto podemos decir que **la función transferencia es la transformada de Laplace  de la respuesta al impulso de un sistema LTI**.

+++ {"editable": true, "slideshow": {"slide_type": "slide"}}

## Ejercicio de simulación del sistema mecánico

+++ {"editable": true, "slideshow": {"slide_type": "fragment"}}

El paquete de control instalado con las herramientas del curso nos brinda la posibilidad de implementar sistemas descritos a partir de su función transferencia y simularlos frente a entradas.

+++ {"editable": true, "slideshow": {"slide_type": "notes"}}

El paquete de control aprovecha la particularidad de que las funciones transferencias transferencias son, en general, relaciones de polinomios en $s$, por lo tanto se definen con los coeficientes del polinomio numerador y los coeficientes del polinomio numerador.

```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: slide
---
from control import tf, step_response
m=1
b=0.2
k=0.5

H=tf([1],[m,b,k]) # primer arg coef. de pol. den y seg arg coef. del pol. den.
H
```

```{code-cell} ipython3
---
editable: true
raw_mimetype: ''
slideshow:
  slide_type: fragment
---
r = step_response(H) # simula al sistema anterior con una entrada escalón 
                     # unitario en t=0
r
```

+++ {"editable": true, "slideshow": {"slide_type": ""}}

En `r` se encuentra la información relevante de la respuesta al escalón del este sistema. Si queremos graficar la señal de salida del sistema, luego del escalón podremos hacerlo usando `matplotlib`.

```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: skip
---
import matplotlib.pyplot as plt
%matplotlib inline
```

```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: slide
---
plt.figure(figsize=(8,3))
plt.plot(r.t, r.y[0,0])
plt.grid()
plt.title('Respuesta al escalón')
plt.xlabel('Tiempo [s]')
plt.ylabel('Desplazamiento[m]');
```

+++ {"editable": true, "slideshow": {"slide_type": "notes"}}

Si se observa el código detenidamente podrán ver que la salida se usa con el indice [0,0]. Para entender esto veamos la forma que tiene el `y` de la respuesta al escalón.

```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: slide
---
r.y.shape
```

+++ {"editable": true, "slideshow": {"slide_type": "notes"}}

Podemos ver que el la respuesta tiene 3 dimensiones.
En tercera dimensión se encuentran los puntos de la respuesta al escalón, las primeras dos dimensiones se usan en casos de que el sistema simulado tenga muchas entras/salidas.
Con la primera dimensión se selección la salida, y con la segunda dimensión la entrada a la cual se aplica el escalón.
En este caso como el sistema tiene solo una entrada y una salida debemos seleccionar obligatoriamente la salida 0 para la primer dimensión y la entrada 0 para la segunda dimensión.

+++ {"editable": true, "slideshow": {"slide_type": "slide"}}

## Ejercicio. Respuesta al impulso (5 minutos)

Usando la función `impulse_response` graficar la respeta al impulso (o lo que es lo mismo, la antitransformada de la función transferencia)

+++ {"editable": true, "slideshow": {"slide_type": "slide"}}

## Ejercicio. Transformada de Laplace. (15 minutos)

Obtener la transformada de Laplace del sistema del pizarrón sabiendo que la entrada es la tensión $u(t)$ en bornes de la fuente, la salida es la tensión $y(t)$ en bornes de la resistencia, y que la ecuación que gobierna la tensión en un capacitor es

$$v_c(t) = \frac{1}{C}\int_0^ti_C(t)dt$$

y la tensión en la resistencia es:

$$v_R(t)=i_R(t)R$$

Ayudas:

1. Aplicar ley de Kirchoff de tensiones teniendo en cuenta que $i_R(t) = i_C(t)$ y que $u(t)= v_c(t)+v_R(t)$
2. Transformar por Laplace la ecuación anterior y obtener la función transferencia $\frac{I_R(s)}{U(s)}$
3. A partir de la ecuación anterior se puede obtener $\frac{V_C(s)}{U(s)}$ y $\frac{V_R(s)}{U(s)}$

+++ {"editable": true, "slideshow": {"slide_type": "slide"}}

## Filtros analógicos

+++ {"editable": true, "slideshow": {"slide_type": "notes"}}

Vamos a describir las características de los tipos de filtros más habituales.
Por otra parte, un cierto conocimiento de filtrado analógico es necesario para diseñar las etapas de adquisición (filtros antialising) y reproducción (filtros de reconstrucción) en sistemas de procesamiento digital.
En la siguiente figura mostramos las respuestas en frecuencia que definen a los filtros analógicos más empleados.

En general, para poder sintetizar un filtro analógico, primero vamos a obtener su función transferencia.
Según los objetivos del filtro existen distintos tipos entre los cuales podemos elegir para obtener la función transferencia.

+++ {"editable": true, "slideshow": {"slide_type": "slide"}}

## Tipos de filtros según su rango de frecuencias de paso

- pasa-bajos: permite el paso de bajas frecuencias eliminando las altas
- pasa-altos: permite el paso de las altas frecuencias eliminando las altas
- pasa-banda: permite el paso de un rango de frencuencias, elimando las altas y las bajas
- elimina-banda: rechaza un rango de frecuencias, permitiendo el paso de frecuencias altas y bajas
- pasa-todos: son filtro que lo que buscar es modificar la fase más que la ganancia del sistema.

+++ {"editable": true, "slideshow": {"slide_type": "slide"}}

## Parámetros de los filtros

- frecuencia de corte: frecuencia a la cual la ganancia cae 3 db respecto a la ganancia en la frecuencia de paso: (frecuencia infinita para el pasa alto, frecuencia 0 para el pasa bajo y frecuencia media para el pasa banda)
- ancho de banda: tiene sentido su definición para el pasa-banda y es la resta entre la frecuencia de corte superior y la inferior del filtro. En un pasa-bajos el ancho de banda es la frecuencia de corte del sistema.
- atenuación mínima: en general se requiere una atenuación mínima en un rango de frecuencias determinado en la banda de rechazo. Por ejemplo para un filtro pasabajos de frecuencia de corte 100 Hz se le puede pedir una atenuación (inversa de la ganancia) mínima de 100 para frecuencias mayores a 500 Hz.

+++ {"editable": true, "slideshow": {"slide_type": "slide"}}

## Tipos de filtros según su forma en la banda de paso y de rechazo

+++ {"editable": true, "slideshow": {"slide_type": "fragment"}}

![Tipos de filtro según su forma](tipos_filtros.png)

+++ {"editable": true, "slideshow": {"slide_type": "notes"}}

- Filtro de butterworth: Respuesta máximamente plana en la banda de paso y en la banda de eliminación.
- Filtro de   filtros Tchebyshev tipo I: consiguen una caída más abrupta a frecuencias bajas en base a permitir un rizado de la respuesta en frecuencia en alguna de las bandas:
  - Son filtros solo polos
  - Presentan rizado constante en la banda pasante
  - Presentan una caída monotónica en la banda no pasante

+++ {"editable": true, "slideshow": {"slide_type": "notes"}}

- Filtro de filtros Tchebyshev tipo II: consiguen una caída más abrupta a frecuencias bajas en base a permitir un rizado de la respuesta en frecuencia en alguna de las bandas:
  - Son filtros polos y ceros
  - Presentan rizado constante en la banda atenuación
  - Presentan una caída monotónica en la banda de paso
- Filtro de Cauer: consiguen una caída más abrupta en base a permitir rizados constantes en ambas bandas.
- Filtro de Bessel: son filtros solo polos que no pretenden optimizar la anchura de la zona de transición sino asegurar fase lineal en toda la bandas pasante, ya que los filtros anteriores la degradaban progresivamente con la frecuencia.

+++ {"editable": true, "slideshow": {"slide_type": "slide"}}

## Herramienta de diseño de filtro

El paquete de `scipy` de Python cuenta con varias herramientas para el diseño de filtros dentro del módulo `signal`.

Vamos a mostrar aquí las funciones para un filtro butterworth:

- `scipy.signal.buttord`: sirve para obtener el orden del filtro de butterwoth que cumple con los requerimientos en la banda de paso y en la banda de rechazo. Toma como argumento las banda de paso de y rechazo y las ganancias de para esas bandas.
- `scipy.signal.butt`: sirve para obtener el filtro a partir de las frecuencias de corte y el orden obtenido con la función anterior.

+++ {"editable": true, "slideshow": {"slide_type": "slide"}}

El mismo par de funciones anteriores se puede encontrar para los otros tipos de filtros: Cauer, Chebyshev I y II, y Bessel.

Otras dos funciones útiles par el diseño de filtros pueden ser:

- `scipy.signal.iirdesign`
- `scipy.signal.iirfilter`

Todas estas funciones están pensadas para obtener la función transferencia de un filtro, pero se utilizan de manera diferente. Siempre es conveniente chequear la documentación de `scipy.signal` al respecto.

+++ {"editable": true, "slideshow": {"slide_type": "slide"}}

### Ejemplo de diseño de filtro pasa-banda

Supongamos que queremos realizar un filtro pasa-banda con frecuencias de corte 100 y 500 rad/seg, con ganancia 1 en la banda de paso y una atenuación 20 para ganancias menores a 10 rad/seg y mayores 2500 rad/seg.

Para diseñar el filtro en cuestión utilizaremos `sicpy`. Sin embargo, para estudiar las respuesta de este filtro voy a utilizar el paquete de `control` instalado con las herramientas dadas.

```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: slide
---
from control import tf, bode, forced_response
from scipy.signal import iirdesign, iirfilter
```

+++ {"editable": true, "slideshow": {"slide_type": "fragment"}}

Para diseñar utilizaremos la función `iirdesign`. Los argumentos de esta función son:

1. frecuencias de corte para la banda de paso
1. frecuencia donde comienza la banda de atenuación
1. atenuación máxima en db en la banda de paso
1. atenuación mínima en db requerida en la banda de atenuación

```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: subslide
---
b,a= iirdesign([100, 500], [10, 2500], 3, 26, True, 'butter', 'ba')
G=tf(b,a)
G
```

+++ {"editable": true, "slideshow": {"slide_type": "notes"}}

Como discutimos en clase, el análisis de la respuesta en frecuencia respecto a su comportamiento en regimen estacionario (luego de que transcurre mucho tiempo), se puede estudiar a partir de la función transferencia evaluando a la misma en $s=j\omega$.
Para esto vamos a definir un vector de frecuencias `w`, distribuidos logaritimicamente.
En la siguiente función generamos un vector con 1001 elementos entre $10^0$ y $10^4$ distribuidos en forma logarítmica.

```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: subslide
---
import numpy as np
w = np.logspace(0, 4, 1001)
```

+++ {"editable": true, "slideshow": {"slide_type": "notes"}}

En el módulo de control, la evaluación de una función transferencia en un punto del planto $s$ se realiza de la misma forma que la evaluación de una función cualquiera, es decir con el nombre y el argumento entre paréntesis. Para evaluar en el vector de frecuencias Python nos permite hacer lo siguiente:

```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: fragment
---
m=np.abs(G(1j*w))
```

```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: skip
---
import matplotlib.pyplot as plt
%matplotlib widget
```

```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: subslide
---
f,a = plt.subplots(1,1, figsize=(7,4))
a.semilogx(w, m)
a.grid()
a.set_xlabel('Frecuencia [rad/seg]')
a.set_ylabel('Magnitud [unidades físicas ganancia]');
```

+++ {"editable": true, "slideshow": {"slide_type": "notes"}}

Es normal usar escala logarítmica en el eje de frecuencias. En el eje y, dependiendo de lo que se quiera estudiar se usa escala logarítmica o lineal. Personalmente para estudiar el comportamiento de filtros prefiero la escala lineal en el eje y para evitar las fugas a menos infinito que no permiten ver los valores de ganancia 0 o de alta atenuación.

Cuando se utilizan en el eje y escala logaritmica, en general se usan el dB como unidad. el db se define como $x|_{dB}=20\log(x)$.

Entonces, la misma gráfica en dB la podemos hacer como:

```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: subslide
---
f,a = plt.subplots(1,1, figsize=(7,4))
a.semilogx(w, 20*np.log(m))
a.grid()
a.set_xlabel('Frecuencia [rad/seg]')
a.set_ylabel('Magnitud [dB]');
```

+++ {"editable": true, "slideshow": {"slide_type": "notes"}}

Esta forma de graficar es muy común de verla en el análisis de sistemas realimentados.

Vamos ahora a analizar la respuesta de sistema a entradas senoidales, pero ahora a partir de hacer la simulación del sistema. Para esto, vamos a generar una señal senoidal de una frecuencia determinada por `ws` y de tiempo `tf`.

```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: subslide
---
ws = 200
tf= 2
t=np.linspace(0, tf, 10001)

u=np.sin(ws*t)
```

+++ {"editable": true, "slideshow": {"slide_type": "notes"}}

Para obtener la respuesta utilizaremos la función `forced_response` del paquete de control.

```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: fragment
---
r=forced_response(G,t,u)
```

```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: subslide
---
f,a = plt.subplots(1,1, figsize=(9,4))
a.plot(t, r.y[0])
a.grid()
a.set_xlabel('Tiempo [s]')
a.set_ylabel('Magnitud [unidad física de la salida]');
```

+++ {"editable": true, "slideshow": {"slide_type": "slide"}}

## Función trnasferencia y la respuseta al impulso

+++ {"editable": true, "slideshow": {"slide_type": ""}}

En la primera parte del temario de filtros vimos una herramienta de modelado de sistemas dinámicos: *la función transferencia*. Vimos que la función transferencia la podemos pensar de dos formas:

- La transformada de Laplace de la salida del sistema sobre la transformada de Laplace de la entrada del sistema.
- La transformada de Laplace de la respuesta al impulso del sistema.

+++ {"editable": true, "slideshow": {"slide_type": "notes"}}

Sin embargo, hemos dejado pasar una propiedad de la transformada de Laplace muy importante, pero que si hemos visto en la transformada de Fourier, y es la convolución. Está propiedad nos dice que la multiplicación de dos funciones en el dominio transformado de Laplace es la convolución en el dominio sin transformar. Es decir:

+++ {"editable": true, "slideshow": {"slide_type": "fragment"}}

$$G_1(s).G_2(s)=g_1(t) * g_2(t)=\int_0^t g(\tau)h_1(t-\tau)\text d\tau$$

+++ {"editable": true, "slideshow": {"slide_type": "slide"}}

## Cálculo de la respuesta temporal de un sistema en el dominio temporal

+++ {"editable": true, "slideshow": {"slide_type": "fragment"}}

Recordando lo visto en la clase anterior podemos notar que la salida del sistema en el dominio de Laplace lo podemos calcular como:

$$Y(s)=H(s)U(s)$$

+++ {"editable": true, "slideshow": {"slide_type": "notes"}}

donde $H(s)$ es la función transferencia, $U(s)$ es la transformada de Laplace de la entrada y $Y(s)$ es la transformada de Laplace de la salida.

Utilizando la propiedad de convolución se puede ver muy fácilmente que:

+++ {"editable": true, "slideshow": {"slide_type": "fragment"}}

$$y(t) = h(t)*u(t)=\int_0^th(\tau)u(t-\tau)\text d \tau$$

La salida de un sistema lineal e invariante en el tiempo se puede calcular como la convolución de la respuesta al impulso y la entrada aplicada.

+++ {"editable": true, "slideshow": {"slide_type": "notes"}}

De esta manera podemos que el la respuesta al impulso también caracteriza la sistema, es decir, que conociendo la respuesta al impulso podemos obtener la respuesta a cualquier entrada arbitraría.

+++ {"editable": true, "slideshow": {"slide_type": "slide"}}

## Ejemplo respuesta al impulso del sistema masa resorte

+++ {"editable": true, "slideshow": {"slide_type": "fragment"}}

Volvamos al sistema masa resorte anterior. Recordemos que la función transferencia es:

$$X_{\delta}(s)=\frac{1}{ms^2+bs+k}$$

+++ {"editable": true, "slideshow": {"slide_type": "notes"}}

Para poder antitransformada fácilmente, lo que se suele hacer es separar la función en fracciones simples. Para hacer estor debemos tener en cuenta la posición de los polos (ceros del denominador) del sistema. Podemos separar el problema en dos casos:

+++ {"editable": true, "slideshow": {"slide_type": "slide"}}

1. Si el sistema tiene polos complejos conjugados, entonces las fracciones simples se pueden escribir como:

$$H(s)=\frac{As+B}{(s-(-\sigma+j\omega))(s-(-\sigma-j\omega))}$$

+++ {"editable": true, "slideshow": {"slide_type": "fragment"}}

2.  El caso de se tengan polos reales simples:

$$H(s)=\frac{A}{s-p_1}+\frac{B}{s-p_2}$$

+++ {"editable": true, "slideshow": {"slide_type": "notes"}}

Luego aplicando superposición podemos resolver los dos términos de la respuesta la transformada de la respuesta al impulso mediante la suma de las transformadas de cada una de las fracciones simples.

+++ {"editable": true, "slideshow": {"slide_type": "slide"}}

```{note}
Todas las funciones transferencias de sistemas físicos tienen el orden del denominado mayor o igual que el del denominador, por lo tanto todos son factibles de aplicar fracciones, y encontrar de esta manera su antitransformada o respuesta al impulso 
```

+++ {"editable": true, "slideshow": {"slide_type": "notes"}}

Volvamos al caso de una masa y un resorte, y obtengamos la antitransformada para una combinación de valores de $m$, $b$ y $k$ en que sean complejos conjugados y otro para el caso en que sean reales simples.

Supondremos que saben aplicar fracciones simples y utilizar las tablas (son problemas matemáticos que no son cubiertos en este curso), lo haremos mediante el paquete de simbólico de Python `sympy`.

+++ {"editable": true, "slideshow": {"slide_type": "subslide"}}

### Caso 1: Polos complejos conjugados

Supongamos que $m=1, b=0.2 y k=5$. Entonces los polos resultan:

```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: subslide
---
import numpy as np
m=1
b=0.2
k=0.5

np.roots([m,b,k]) 
```

+++ {"editable": true, "slideshow": {"slide_type": "fragment"}}

Para resolver la antitransformada entonces:

```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: fragment
---
import sympy as sp
```

```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: fragment
---
s, t=sp.symbols('s, t')
```

```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: subslide
---
H = 1/(m*s**2+b*s+k)
sp.apart(H, s)
```

```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: fragment
---
sp.inverse_laplace_transform(H, s, t)
```

+++ {"editable": true, "slideshow": {"slide_type": "slide"}}

### para el caso en que los polos sean reales simples

Hagamos que $b=2$

```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: fragment
---
b2 = 2
np.roots([m, b2, k])
```

```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: subslide
---
H2 = 1/(m*s*s+b2*s+k)
H2
```

+++ {"editable": true, "slideshow": {"slide_type": ""}}

Ahora la separación por facciones simples es:

```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: fragment
---
H2a = sp.apart(H2, s, full=True).doit()
H2a
```

+++ {"editable": true, "slideshow": {"slide_type": "skip"}}

Hagamos la antitranformada de H2

```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: fragment
---
sp.inverse_laplace_transform(H2, s, t)
```

```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: fragment
---
sp.inverse_laplace_transform(H2a, s, t)
```

+++ {"editable": true, "slideshow": {"slide_type": "notes"}}

Podemos ver que aunque `sympy` resuelve correctamente la antitransformada, a veces es conveniente procesar la funciones para que el cálculo simbólico sea más simple, como por ejemplo es realizar la separación por fracciones simples.

+++ {"editable": true, "slideshow": {"slide_type": ""}}

```{important}
La forma numérica de calcular la antitransformada de Laplace de la función transferencia es simplemente calcular la respuesta al impulso
```

+++ {"editable": true, "slideshow": {"slide_type": "slide"}}

## Sistemas en tiempo discreto

**HACER FIGURA DE ENTRADA SALIDA DE UN SISTEMA DISCRETO**

+++ {"editable": true, "slideshow": {"slide_type": "slide"}}

Tenemos dos formas de analizar la respuesta a cualquier entrada de un sistema discreto:

+++ {"editable": true, "slideshow": {"slide_type": "fragment"}}

1. Resolviendo explicitamente la ecuación que describe el comportamiento del sistema. En general para sistemas LTI es:

$$y(n) = -\sum_{k=1}^N a_k y(n-k)+ \sum_{k=0}^M b_k(n-k)$$

+++ {"editable": true, "slideshow": {"slide_type": "notes"}}

donde ${a_k}$ y ${b_k}$ son parámetros constantes del sistema.

+++ {"editable": true, "slideshow": {"slide_type": "fragment"}}

Esta forma se denomina **ecuación en diferencias**.

+++ {"editable": true, "slideshow": {"slide_type": "slide"}}

2. Si el sistema es lineal, podemos descomponer la señal de entrada en una combinación lineal de señales elementales para las cuales sea más fácil calcular la respuesta, y luego aplicamos superposición. Entonces matemáticamente sería:

$$u(n) = \sum_k c_k u_k(n)$$

+++ {"editable": true, "slideshow": {"slide_type": "fragment"}}

donde $c_k$ son los coeficientes de la combinación lineal y $u_k(n)$ son las señales elementales.

+++ {"editable": true, "slideshow": {"slide_type": "subslide"}}

Llamamos  $y_k(n)$ a la respuesta del sistema a la señal $u_k(n)$, entonces:

$$y_k(n) = H(u_k(n))$$

+++ {"editable": true, "slideshow": {"slide_type": "subslide"}}

Y la salida la podemos escribir como la combinación lineal de las respuestas:

$$
\begin{align*}
y(n) &= H(u(n))\\
y(n) &= H\left(\sum c_ku_k(n)\right)\\
y(n) &= \sum\left(H\left[c_ku_k(n)\right]\right)\\
y(n) &= \sum c_k\left(H\left[u_k(n)\right]\right)\\
y(n) &= \sum c_ky_k(n)
\end{align*}
$$

+++ {"editable": true, "slideshow": {"slide_type": "subslide"}}

La señal que podemos elegir para descomponer la señal podría ser el impulso. En tiempo discreto el impulso se puede definir:

$$
\delta(n)= \left\{
    \begin{align*}
    1,\qquad &\text{si } n=0\\
    0,\qquad &\text{si } n\neq k
    \end{align*}
\right.
\implies
\delta(n-k)= \left\{
    \begin{align*}
    1,\qquad &\text{si } n=k\\
    0,\qquad &\text{si } n\neq k
    \end{align*}
\right.
$$

+++ {"editable": true, "slideshow": {"slide_type": "subslide"}}

Entonces adoptamos:

$$u_k(n)=\delta(n-k)$$

+++ {"editable": true, "slideshow": {"slide_type": "fragment"}}

debemos encontrar los $c_k$ para cada uno de los $u_k$,

$$u(n) = \sum c_ku_k(n) = \sum c_k \delta(n-k)$$

+++ {"editable": true, "slideshow": {"slide_type": "fragment"}}

Podemos notar que

$$ x(n) = \delta(n-k)= x(k)\delta(n-k) $$

+++ {"editable": true, "slideshow": {"slide_type": "fragment"}}

Entonces la señal $u(n)$ resulta

$$u(n) = \sum_{k=-\infty}^{\infty}u(k)\delta(n-k)$$

+++ {"editable": true, "slideshow": {"slide_type": "subslide"}}

La respuesta a $u(n)$ será  

$$ y(n) = H\left[u(n)\right] = H \left[\sum_{k=-\infty}^\infty u(k)\delta(n-k)\right]$$

+++ {"editable": true, "slideshow": {"slide_type": "fragment"}}

Aplicamos superposición:

$$ y(n) = \sum_{k=-\infty}^\infty H \left[u(k)\delta(n-k)\right]$$

$$ y(n) = \sum_{k=-\infty}^\infty u(k) H \left[\delta(n-k)\right]$$

+++ {"editable": true, "slideshow": {"slide_type": "subslide"}}

Definimos la repuesa al impulso aplicado en $k$ en el instante $n$ como:

$$ h(n,k) \triangleq H \left[\delta(n-k)\right]$$

+++ {"editable": true, "slideshow": {"slide_type": "fragment"}}

y la respuesta es ahora:

$$y(n)=\sum_{k=-\infty}^{\infty} u(k)h(n,k)$$

+++ {"editable": true, "slideshow": {"slide_type": "subslide"}}

Si el sistema es invariante en el tiempo entonces:

$$\delta(n) \implies h(n,0) = h(n)$$

$$\delta(n-k)\implies h(n,k) = h(n-k)$$

+++ {"editable": true, "slideshow": {"slide_type": "subslide"}}

Con lo que resulta:

$$y(n) = \sum_{k=-\infty}^{\infty} u(k)h(n-k)$$

Que es lo que se conoce como suma de convolución.

+++ {"editable": true, "slideshow": {"slide_type": ""}}

## Ecuaciones en diferencias

### Sistemas IIR y FIR

Un sistema FIR es un sistema que tiene un número finitos de términos en su respuesta al impulso.

$$h(n)=0 \text{ para } n<0 \text{ y } n>N$$

$$ y(n) = \sum_{k=0}^{N-1} h(n)u(n-k) $$

**HACER DIBUJO DE DIAGRAMA DE BLOQUES DE UN SISTEMA FIR**

- $q$: operador dezplazamiento directo
- $q^{-1}$: operador dezplazamiento inverso (retardo)

$$qx(n) = x(1+n)$$
$$q^{-1}x(n) = x(n-1)$$

+++ {"editable": true, "slideshow": {"slide_type": ""}}

IIR causal significa que $h(n) = 0$ para $n<0$ y

$$h(n)=\sum_{k=0}^{\infty}h(k)u(n-k)$$

A cualquier sistema IIR los podemos aproximar hasta el coeficiente $k$ y partir de ahi lo representamos como un sistema FIR. Esto representación se conoce como implementación FIR no recursiva.

Existe para los sistemas

+++ {"editable": true, "slideshow": {"slide_type": ""}}

Utilizando esta herramienta vimos que un sistema masa resorte, donde la entrada es la fuerza y la salida la posición del sistema se puede pensar como un filtro pasa-bajos.
