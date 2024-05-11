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

# Filtros (parte 2)

+++

En la primera parte del temario de filtros vimos una herramienta de modelado de sistemas dinámicos: *la función transferencia*. Vimos que la función trnasferencia la podemos pensar de dos formas:

- La transformada de Laplace de la salida del sistema sobre la transformada de Laplace de la entrada del sistema.
- La transformada de Laplace de la respuseta al impulso del sistema.

Sin embargo, hemos dejado pasar una propiedad de la transformada de Laplace muy importante, pero que si hemos visto en la transformada de Fourier, y es la convolución. Está propiedad nos dice que la multiplicación de dos funciones en el dominio transformado de Laplace es la convolución en el dominio sin transformar. Es decir:

$$G_1(s).G_2(s)=g_1(t) * g_2(t)=\int_0^t g(\tau)h_1(t-\tau)\text d\tau$$

+++


## Cálculo de la respuesta temporal de un sistema en el dominio temporal

Recordando lo visto en la clase anterior podemos notar que la salida del sistema en el dominio de Laplace lo podemos calcular como:

$$Y(s)=H(s)U(s)$$

donde $H(s)$ es la función transferencia, $U(s)$ es la transformada de Laplace de la entrada y $Y(s)$ es la transformada de Laplace de la salida.

Utilizando la propiedad de convolución se puede ver muy facilmente que:

$$y(t) = h(t)*u(t)=\int_0^th(\tau)u(t-\tau)\text d \tau$$

La salida de un sistema lineal e invariente en el tiempo se puede calcular como la convolución de la respuesta al impulso y la entrada aplicada.

De esta manera podemos que el la respuesta al impulso también caraceriza la sistema, es decir, que conociendo la respuesta al impulso podemos obtener la respuesta a cualquier entrada arbitraría.

+++


## Ejemplo respuesta al impulso del sistema masa resorte

Volvamos al sistema masa resorte anterior. Recordemos que la función trnasferencia es:

$$X_{\delta}(s)=\frac{1}{ms^2+bs+k}$$

Para poder antitransformarla facilmente, lo que se suele hacer es separar la función en fracciones simples. Para hacer estor debemos tener en cuenta la posición de los polos (ceros del denominador) del sistema. Podemos separar el problema en dos casos:

+++


1. Si el sistema tiene polos complejos conjugados, entonces las freacciones simples se pueden escribir como:

$$H(s)=\frac{As+B}{(s-(-\sigma+j\omega))(s-(-\sigma-j\omega))}$$

2.  El caso de se tengan polos reales simples:

$$H(s)=\frac{A}{s-p_1}+\frac{B}{s-p_2}$$

Luego aplicando superopisción podemos resovler los dos términos de la respuesta la transformada de la respuesta al impulso mediante la suma de las transformadas de cada una de las fracciones simples.

```{note}
Todas las funciones transferencias de sistemas físicos tienen el orden del denominado mayor o igual que el del denominador, por lo tanto todos son factibles de aplicar fracciones, y encontrar de esta manera su antitransformada o respuesta al impulso 
```

Volvamos al caso de una masa y un resorte, y obtengamos la antitrasformada para una combinación de valores de $m$, $b$ y $k$ en que sean complejos cojugados y otro para el caso en que sean reales simples.

Supondremos que saben aplicar fracciones simples y utilizar las tablas (son problemas matemáticos que no son cubiertos en este curso), lo haremos mediante el paquete de simbólico de Python `sympy`.

+++

### Caso 1: Polos complejos conjugados

Supongamos que $m=1, b=0.2 y k=5$. Entonces los polos resultan:

```{code-cell} ipython3
import numpy as np
m=1
b=0.2
k=0.5

np.roots([m,b,k]) 
```

Para resolver la antitransformada entonces:

```{code-cell} ipython3
import sympy as sp

s, t=sp.symbols('s, t')
```

```{code-cell} ipython3
H = 1/(m*s**2+b*s+k)

sp.apart(H, s)
```

```{code-cell} ipython3
sp.inverse_laplace_transform(H, s, t)
```

### para el caso en que los polos sean reales simples 

Hagamos que $b=2$

 

```{code-cell} ipython3
b2 = 2
np.roots([m, b2, k])
```

```{code-cell} ipython3
H2 = 1/(m*s*s+b2*s+k)
```

Ahora la separación por facciones simples es:

```{code-cell} ipython3
H2a = sp.apart(H2, s, full=True).doit()
H2a
```

Hagamos la antitranformada de H2

```{code-cell} ipython3
sp.inverse_laplace_transform(H2, s, t)
```



```{code-cell} ipython3
sp.inverse_laplace_transform(H2a, s, t)
```

Podemos ver que aunque `sympy` resuelve correctamente la antitrasformadas, a veces es conveniete procesar la funciones para que el cálculo simbólico sea más simple, como por ejemplo es realizar la separación por fracciones simples.

+++

```note
La forma numérica de calcular la antiformada de Laplace de la función trnasferencia es simpemente calcular la respuesta al impulso
```

+++

## Sistemas en tiempo discreto

Cuando representamos nuestros sistema masa resorte en tiempo continuo, las ecuaciones resultantes eran ecuaciones diferenciales.
 
En tiempo discreto estas ecuaciones no puede ser usadas para describir nuestro sistema.  En general un sistema discreto se representa como un conjunto de ecuaciones en difrencia. Es decir:

+++






Utilizando esta herramienta vimos que un sistema masa resorte, donde la entrada es la fuerza y la salida la posición del sistema se puede pensar como un filtro pasa-bajos.

+++








Utilizando esta herramienta vimos que un sistema masa resorte, donde la entrada es la fuerza y la salida la posición del sistema se puede pensar como un filtro pasa-bajos.


+++
