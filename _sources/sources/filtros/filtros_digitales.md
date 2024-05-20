---
jupytext:
  formats: ipynb,md:myst
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

## Sistemas en tiempo discreto

En la figura siguiente podemos ver una representación de un sistema discreto:

![Sistema en tiempo discreto](sis_z.png)

+++ {"editable": true, "slideshow": {"slide_type": "slide"}}

Tenemos dos formas de analizar la respuesta a cualquier entrada de un sistema discreto:

+++ {"editable": true, "slideshow": {"slide_type": "fragment"}}

1. Resolviendo explicitamente la ecuación que describe el comportamiento del sistema. En general para sistemas LTI es:

$$y(n) = -\sum_{k=1}^N a_k y(n-k)+ \sum_{k=0}^M b_k(n-k)$$

+++ {"editable": true, "slideshow": {"slide_type": "notes"}}

donde ${a_k}$ y ${b_k}$ son parámetros constantes del sistema.

+++ {"editable": true, "slideshow": {"slide_type": "fragment"}}

A la forma de la ecuación anterior se la denomina **ecuación en diferencias**.

+++ {"editable": true, "slideshow": {"slide_type": "slide"}}

2. Si el sistema es lineal, podemos descomponer la señal de entrada en una combinación lineal de señales elementales para las cuales sea más fácil calcular la respuesta, y luego aplicamos superposición. Entonces matemáticamente sería:

$$u(n) = \sum_k c_k u_k(n)$$

+++ {"editable": true, "slideshow": {"slide_type": "fragment"}}

donde $c_k$ son los coeficientes de la combinación lineal y $u_k(n)$ son las señales elementales.

+++ {"editable": true, "slideshow": {"slide_type": "subslide"}}

Si llamamos  $y_k(n)$ a la respuesta del sistema a la señal $u_k(n)$, entonces:

$$y_k(n) = H(u_k(n))$$

+++ {"editable": true, "slideshow": {"slide_type": "subslide"}}

Y la salida la podemos escribir como la combinación lineal de las respuestas:

\begin{align*}
y(n) &= H(u(n))\\
y(n) &= H\left(\sum c_ku_k(n)\right)\\
y(n) &= \sum\left(H\left[c_ku_k(n)\right]\right)\\
y(n) &= \sum c_k\left(H\left[u_k(n)\right]\right)\\
y(n) &= \sum c_ky_k(n)
\end{align*}

+++ {"editable": true, "slideshow": {"slide_type": "subslide"}}

Una señal util que podemos elegir para descomponer $u(k)$ podría ser el impulso. En tiempo discreto el impulso se define:

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

$$ x(n)\delta(n-k)= x(k)\delta(n-k) $$

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

Que es lo que se conoce como **suma de convolución**.

+++ {"editable": true, "slideshow": {"slide_type": ""}}

## Ecuaciones en diferencias

### Sistemas IIR y FIR

Un sistema FIR es un sistema que tiene un número finitos de términos en su respuesta al impulso.

$$h(n)=0 \text{ para } n<0 \text{ y } n>N$$

$$ y(n) = \sum_{k=0}^{N-1} h(n)u(n-k) $$

![Diagrama de Bloques de un Sistemas FIR](bd_sisz.png)

- $q$: operador dezplazamiento directo
- $q^{-1}$: operador dezplazamiento inverso (retardo)

$$qx(n) = x(1+n)$$
$$q^{-1}x(n) = x(n-1)$$

+++ {"editable": true, "slideshow": {"slide_type": ""}}

IIR causal significa que $h(n) = 0$ para $n<0$ y

$$h(n)=\sum_{k=0}^{\infty}h(k)u(n-k)$$

A cualquier sistema IIR los podemos aproximar hasta el coeficiente $k$ y partir de ahi lo representamos como un sistema FIR. Esto representación se conoce como implementación FIR no recursiva.

Es importante destacar a no cualquier sistema IIR puede ser representado como ecuaciones en direferencias.

+++

## Implemetación de filtros digitales a partir de ecuaciones en diferencias

Si escribimos la ecuación en diferencia en función del operador retardo, la misma resulta:

$$y(n) = -\sum_{k=1}^N a_k q^{-k} y(n)+ \sum_{k=0}^M b_k q^{-k}u(n)$$

La función transferencia la podemos escribir en tiempo discreto como:

$$\frac{y(n)}{u(n)} = \dfrac{\sum_{k=0}^M b_k q^{-k}}{\sum_{k=1}^N a_k q^{-k}} = q^{n-m} \dfrac{\sum_{k=0}^M b_k q^{m-k}}{\sum_{k=1}^N a_k q^{n-k}}$$

Esta funcionestransferencia las hemos obtenido haciendo unas trampa. Hmeos considerado el operador $q^{-1}$ como el retardo. Este operador no existe en el dominio temporal. Sin embargo, existe una transformada de señales en tiempo discreto, en la que la variable del dominio transformado resulta en el translado temporal. Esta transformada se la conoce como transformada $Z$. Por lo tanto, la forma correcta de escribir la función anterior sería utilizando las expresiones en este dominio, que resultan:

$$\frac{Y(z)}{U(z)} = z^{n-m} \dfrac{\sum_{k=0}^M b_k z^{m-k}}{\sum_{k=1}^N a_k z^{n-k}}$$

Esta transformada $Z$ es en el dominio de tiempo discreto análoga a la transformda de Laplace en el dominio de tiempo continuo.

La implemtanción de un filtro discreto se puede hacer facilmente con un algoritmo que calcule la salida $y$ para cada instante $n$ a partir del valor actual y los $m$ valores anteriores de la entrada junto con los $n$ valores anteriores de la salida.

De esta manera, hemos mucho más directo o cercano a la imlementación, la función transferencia en tiempo discreto que lo que es la función transferencia en el dominio continuo.

```{code-cell} ipython3

```
