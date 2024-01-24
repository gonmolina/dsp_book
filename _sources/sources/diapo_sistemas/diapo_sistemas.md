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

# Sistemas dinámicos

## Sistemas

```{admonition} Definición
:class: important

**Sistema.** Conjunto delimitado de componentes que interactúan entre sí.
```

En general esta definición es un genérica y no está necesariamente restringidos a sistemas físicos. Pueden ser biológicos, económicos, computacionales, informáticos, sociales, etc.

+++

### Ejemplos de sistema

+++

#### Sistema de Calefacción Solar

+++

```{figure} ./calefa_solar.png
:width: 450px
:name: calefa-solar

Sistema de calefacción solar
```

+++

```{figure} ./esquema_calefa_solar.png
:width: 450px
:name: esquema-calefa-solar

Esquema entrada salida del sistema de calefacción solar
```

+++

#### Ecosistema

Ambiente aislado compuesto por dos clases de individuos: presas (P: población de presas) y depredadores (D: población de depredadores).

Hipótesis del modelo:

1. En ausencia de depredadores (D=0), la población de presas crece exponencialmente.
1. Los depredadores sólo se alimentan de presas, por lo que en ausencia de presas (P=0), la población de depredadores se extingue exponencialmente.

+++

Un posible modelo que verifica estas hipótesis es el de **Lotka-Volterra**

$$
\begin{align*}
\frac{dP(t)}{dt}&=\alpha_1P(t)-\alpha_2D(t)P(t)\\
\frac{dD(t)}{dt}&=\alpha_3P(t)D(t)-\alpha_4D(t)
\end{align*}
$$

#### Ejercicio de análisis entrada/salida

En esta representación del sistema, ¿cuáles les parece podrían que podrían ser las entradas y las salidas?

## Caracterización de sistemas

### Linealidad

Un sistema es **lineal** si verifica el **Principio de Superposición**, tanto para entradas como para condiciones iniciales.

Si para una entrada $u_1$ aplicada en el instante $t_0$, con el sistema en un estado inicial $\sum_0$, la salida es $y_1$ , y para una entrada $u_2$ aplicada en el instante $t_0$, con el sistema en un estado inicial $\sum_0$, la salida es $y_2$
entonces si se aplica una $c_1u_1 +c_2u_2$  entrada en el instante $t_0$, con el sistema en un estado inicial $\sum_0$, la salida será $c_1y_1 +c_2y_2$

+++

```{figure} ./superposicion1.png
:width: 450px
:name: hipo-super

Hipótesis linealidad
```

+++

```{figure} ./superposicion2.png
:width: 450px
:name: req-super

Requerimiento de linealidad
```

* Es claro que esta propiedad, como fue definida, es imposible de verificar en la práctica, ya que no se pueden realizar los ensayos propuestos en el mismo período de tiempo.
* Debemos asumir entonces que el sistema no cambia con el tiempo (propiedad de **estacionariedad** o **invariancia en el tiempo**, que definiremos a continuación), de manera que podemos realizar los ensayos a distintos tiempos a partir del sistema en el mismo estado inicial.
* Un sistema que no verifica el principio de Superposición se denomina **no lineal**.

### Invariancia en el tiempo

Un sistema es invariante en el tiempo si su salida es siempre la misma cada vez que se aplique la misma entrada (para las mismas condiciones iniciales), sin importar el instante en que se aplique la entrada. Para dar una definición más precisa, es necesario definir el operador desplazamiento temporal que notaremos: $z_{\Delta}$  que aplicado a una función del tiempo, la desplaza temporalmente un intervalo $\Delta$, es decir:

```{figure} ./estacionariedad.png
:width: 400px
:name: desp-temp

Operador desplazamiento temporal
```

Por lo tanto, el sistema es estacionario estacionario si para una entrada $u(t)$ aplicada en el instante $t_0$ resulta la salida es $y(t),$ entonces para una entrada = $z^\Delta u(t)$ aplicada en $t_0+\Delta$ la salida es $\tilde y(t) =z^\Delta y(t)$

#### Principio de estacionariedad o invariancia en el tiempo

```{figure} ./estacionariedad2.png
:width: 400px
:name: pri-est

Principio de invariancia en el tiempo
```

### Sistemas Estáticos y Dinámicos

Un sistema en el cual la salida en el instante $t$ depende exclusivamente de la entrada en ese instante $t$ es llamado **Sistema Estático** o sin memoria. En contraposición, un **Sistema Dinámico** es uno en el cual la salida en el instante $t$ depende también de valores pasados y/o futuros de la entrada, además del valor en $t$.

Los sistemas que estudiaremos en esta materia son los sistemas dinámicos, es decir que el valor de la salida actual depende no solo de las entradas actuales sino también de los valores de la entradas pasadas. Aunque diseñaremos también algunos controladores que pueden considerarse estáticos, es decir que cuya salida solo depende de valores presentes de la entrada del controlador.

+++

### Sistemas Causales y No causales

Un sistema es **causal** si su respuesta a una entrada no depende de valores futuros de esa entrada y/o valores futuros de salidas. Un sistema que no verifica esta propiedad es llamado **no causal**. En particular un sistema se dice **anticausal** si su respuesta a una entrada depende exclusivamente de valores futuros de esa entrada y/o valores futuros de salidas.

Todos los sistemas físicos son **causales**.

+++

#### Ejercicio clasificación de sistemas

Clasificar los siguientes sistemas si son:

* **lineales/no lineales**
* **dinámicos/estáticos**
* **variantes/invariantes**

1. Sistema amplificador de audio idealizado 1:

$$y(t) = Ku(t)$$

donde $y(t)$ es la señal de potencia de salida y $u(t)$ es la señal de entrada

2. Es un sistema idéntico al sistema 2, pero se tiene en cuenta una degradación de la ganancia debido al calentamiento al pasar el tiempo, que se modela de la siguiente manera:

$$y(t) = K(t)u(t)$$

3. Sistema masa resorte 1

$$ ma(t) = F + mg - b \dot x(t) - K x(t)$$

donde $F(t)$ es una fuerza aplicada que se puede manipular (es la entrada del sistema). La salida del sistema será la posición $x(t)$.

4. Sistema masa resorte 2

$$ ma(t) = F + mg - b \dot x(t) - K x(t)$$

donde $F(t)$ es una fuerza aplicada que se puede manipular (es la entrada del sistema). La salida del sistema será la posición $x(t)$.

+++

### Representación matemática de los sistemas dinámicos

De los ejercicios anteriores podemos notar que los sistemas dinámicos son representados por ecuaciones diferenciales. En general, si la ecuación diferencial es lineal, la podemos escribir como;

$$
\begin{aligned}
 & a_n\frac{d^ny(t)}{dt^n}+a_{n-1}\frac{d^{n-1}y(t)}{dt^{n-1}}+\cdots+a_{1}\frac{dy(t)}{dt}+a_0y(t) = \dots \\
  &\dots  b_p\frac{d^pu(t)}{dt^p}+b_{p-1}\frac{d^{p-1}u(t)}{dt^{p-1}}+\cdots+b_{1}\frac{du(t)}{dt}+b_0u(t)
\end{aligned}
$$

Si recordamos las clases de matemáticas, la resolución de estas ecuaciones se plantean dividiendo el problema en dos:

1. solución de la ecuación homogénea, es decir con $u(t)=0$
2. solución de la ecuación particular, que depende de la forma de $u(t)$

Cuando la ecuación diferencial es la representación matemática de un sistema, se dice que **la solución de la ecuación homogénea es la respuesta libre del sistema**, ya que describe el comportamiento del sistema cuando no es afectado por ninguna entrada externa; y esta solución depende solo de los valores de la condiciones iniciales del mismo, mientas que la **solución de la ecuación particular es la respuesta forzada del sistema** ya que se debe a la entrada.

+++

#### Solución de la ecuación homogénea

De la ecuación anterior obtenemos el polinomio característico del sistema y lo igualamos a cero:

$$ \begin{aligned}
 & a_n\lambda^n+a_{n-1}\lambda^{n-1}+\cdots+a_{1}\lambda+a_0= 0
\end{aligned}
$$

Si las raíces del polinomio anterior son $\lambda_1 \dots \lambda_n$, entonces la solución de la homogénea es:

$$y_H(t)=K_1e^{\lambda_1t}+K_2e^{\lambda_2t}+\dots+K_ne^{\lambda_nt}$$

Podemos ver que la distintos casos de las raíces de $\lambda$:

1. **raíces reales positivas**: en ese caso la solución de la homogénea tenderá a diverger, ya que poseerá términos que tienden a infinito y en este caso de que aparezca alguna raíz de esta forma se dice que el sistema es inestable
2. **raíces reales negativas**: en ese caso la solución de la homogénea en infinito tienda a cero
3. **raíces complejas** en caso de que el polinomio sea a coeficientes las raíces complejas siempre aparecen con su conjugado, y por lo tanto la solución de esos pares se puede escribir como:

$$y_{Hi}(t) = K_ie^{\sigma_it}\cos(\omega_it+\phi_i)$$

Donde $\sigma_i$ y $\omega_i$ son la parte real e imaginaria respectivamente del par ceros complejo conjugado de la ecuación característica, y $K_i$ y $\phi_i$ salen la resolución de la ecuación de euler para senos y cosenos.

Nuevamente acá vemos que si $\sigma_i$ es positivo la solución va a diverger y si es negativo la solución va a converger. Finalmente en el caso de $\sigma=0$ la solución es una señal armónica pura.

+++

#### Solución particular

Para obtener la solución particular dependemos de la forma de la $u(t)$, y se plantea que la solución particular tiene la misma forma. En general nos interesan 3 formas de $u(t)$:
  
 $$
 \begin{align*}
 u(t)&=K_1 &\implies &y_P(t)=K_2\\
 u(t)&=K_1\sin(wt+\phi_1) &\implies &y_P(t)=K_2\sin(wt+\phi_2)\\
 u(t)&=K_1e^{\lambda t} &\implies &y_P(t)=K_2e^{\lambda t}
 \end{align*}
 $$

En caso de que $u(t)$ sea una función no senoidal periódicas, las podemos descomponer según **serie** de Fourier y, aplicando linealidad, obtener la solución de la ecuación particular por superposición.

+++

#### Conclusión

Podemos notar que la respuesta del sistema siempre es una suma de funciones exponenciales y senoidales de distintas frecuencias:

* algunas frecuencias serán dependientes del sistema (solución homogénea)
* algunas frecuencias serán dependientes de la entrada (solución particular)

La principal herramienta para estudiar la composición armónica de una función no necesariamente periódica es la transformada de Fourier. Como las soluciones (respuesta) de nuestras ecuaciones (sistemas) son fundamentalmente funciones (señales) armónicas, es fácil entender la importancia del la transformada de Fourier.

La desventaja de esta transformada es que "no captura" la información respecto a los transitorios, es decir respecto a las exponenciales. matemáticamente la solución a este problema se obtiene mediante la transformada de Laplace. Sin embargo, la gran ventaja de la transformada de Fourier de un sistema es que puede ser aproximada fácilmente a partir de muestras de la señal usando el algoritmo `fft`, mientras que la transformada de Laplace no tiene forma de obtenerse numéricamente a partir de muestras de la señal.

```{code-cell} ipython3

```
