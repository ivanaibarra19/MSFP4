[![Open in MATLAB Online](https://www.mathworks.com/images/responsive/global/open-in-matlab-online.svg)](https://matlab.mathworks.com/open/github/v1?repo=ivanaibarra19/MSFP4)
# Práctica 4 | Modelado y control del sistema endócrino

## Datos académicos

**Estudiante:** Ibarra Monroy Ivana Valeria

**Número de control:** 23210709 

**Correo institucional:** l23210709@tectijuana.edu.mx

**Carrera:** Ingeniería Biomédica  

**Materia:** Modelado de Sistemas Fisiológicos 

**Institución:** Tecnológico Nacional de México / Instituto Tecnológico de Tijuana

**Profesor:** Dr. Paul Antonio Valle Trujillo; paul.valle@tectijuana.edu.mx

Departamento de Ingeniería Eléctrica y Electrónica. Blvd. Alberto Limón Padilla s/n, Tijuana, C.P. 22454, Baja California, México.

## Presentación de la práctica

Esta práctica estudia la respuesta de un circuito que representa de manera simplificada el transporte, la acumulación y la utilización de una señal hormonal. Se parte de dos condiciones, control y caso, para establecer cómo los valores de los componentes modifican la salida ante un mismo estímulo.

El trabajo integra el análisis con las leyes de Kirchhoff, la obtención de funciones de transferencia y el cálculo del error estacionario y los polos. Las representaciones pueden verificarse en MATLAB, Simulink, Simscape Electrical y Python. La actividad corresponde al apartado 5.6 de la guía de Modelado de Sistemas Fisiológicos.

## Propósito del trabajo

El objetivo es relacionar los procesos del sistema endócrino con un circuito de segundo orden y explicar su comportamiento mediante ecuaciones. Para ello se identifican las variables eléctricas, se construye el modelo matemático y se calculan los resultados de control y caso. También se considera el seguimiento de la referencia mediante un PID y la comparación con el circuito de laboratorio.

## Interpretación fisiológica y representación

Las hormonas actúan como señales que se liberan desde una glándula, circulan por el organismo y producen efectos en un tejido diana. La analogía utiliza elementos eléctricos para representar las propiedades de almacenamiento, oposición al transporte y retardo dinámico de esa señal. Se trata de un modelo académico, sin equivalencia directa entre los valores eléctricos y concentraciones hormonales medidas.

### Analogía entre el sistema fisiológico y el eléctrico

| Proceso o variable fisiológica | Elemento eléctrico | Interpretación |
| --- | --- | --- |
| Estímulo para la secreción hormonal | Ve(t) | Señal de entrada |
| Secreción y transporte hormonal | Fe(t) | Corriente que atraviesa R1 |
| Pérdidas y oposición durante el transporte | R1 | Resistencia de entrada |
| Acumulación temporal de la señal hormonal | C | Almacenamiento de energía |
| Inercia de la respuesta del tejido diana | L | Oposición a cambios rápidos de corriente |
| Utilización o eliminación de la señal | R2 | Disipación en la rama de respuesta |
| Flujo asociado con la respuesta del órgano diana | Fs(t) | Corriente por L y R2 |
| Nivel hormonal efectivo normalizado | Vs(t) | Voltaje del nodo de salida respecto a tierra |


### Construcción del circuito equivalente

El circuito inicia con la fuente Ve(t) y una resistencia R1 en serie. Su terminal de salida llega al nodo N2, compartido por el capacitor y el inductor. C se conecta directamente a tierra; la otra trayectoria hacia tierra está formada por L y R2 en serie. El voltaje medido entre N2 y la referencia es Vs(t).

| Nodo o rama | Conexión |
| --- | --- |
| N1 | Terminal positivo de Ve(t) y entrada de R1 |
| N2 | Unión de R1, C y L; nodo donde se mide Vs(t) |
| N3 | Unión entre L y R2 |
| Tierra | Terminal negativo de la fuente, terminal inferior de C y terminal inferior de R2 |
| Rama de entrada | Ve(t) y R1, con corriente Fe(t) hacia N2 |
| Rama capacitiva | C entre N2 y tierra, con corriente Fc(t) |
| Rama de respuesta | L y R2 en serie, con corriente Fs(t) hacia tierra |

La medición de salida abarca tanto el capacitor como la rama completa L–R2. Por eso se utiliza la relación Vs(t) = L dFs(t)/dt + R2 Fs(t). El voltaje de R2 por separado es una variable diferente y no se emplea como salida de esta función de transferencia.


### Parámetros del circuito equivalente

| Parámetro | Control | Caso |
| --- | ---: | ---: |
| R1 | 1 kΩ | 1 kΩ |
| L | 100 mH | 100 mH |
| C | 1 µF | 1000 µF |
| R2 | 100 kΩ | 1 kΩ |
| Entrada | Escalón unitario | Escalón unitario |
| Intervalo de simulación | 0 a 10 s | 0 a 10 s |

Para los cálculos se utilizan unidades del Sistema Internacional:

$$
R_1=1000\ \Omega,\qquad L=0.1\ \mathrm{H}.
$$

$$
C_{control}=10^{-6}\ \mathrm{F},\qquad R_{2,control}=100000\ \Omega.
$$

$$
C_{caso}=10^{-3}\ \mathrm{F},\qquad R_{2,caso}=1000\ \Omega.
$$


### Diagrama biológico en BioRender

**Espacio reservado para insertar el diagrama biológico del sistema endócrino.**

<!-- Al tener la imagen, súbela a la misma carpeta de este README con el nombre biorender_endocrino.png y elimina los delimitadores de comentario de la siguiente línea. -->
<!-- ![Diagrama biológico del sistema endócrino](biorender_endocrino.png) -->

**Figura 1.** Analogía entre el estímulo para la secreción hormonal, su transporte, el almacenamiento temporal y la respuesta del tejido diana. El diagrama deberá relacionar estos procesos con Ve(t), Fe(t), R1, C, L, R2, Fs(t) y Vs(t).

## Desarrollo del modelo

### Definición de las variables y condiciones iniciales

Se considera un circuito lineal con componentes ideales y constantes. Las condiciones iniciales son nulas:

$$
V_s(0^-)=0,\qquad F_s(0^-)=0.
$$

Se emplean Fe(t) y Fs(t) para las corrientes de entrada y de la rama L–R2, respectivamente. También pueden identificarse como i1(t) e i2(t).

### Ley de corrientes de Kirchhoff en N2

En N2, la corriente que llega desde R1 se distribuye entre las dos ramas:

$$
F_e(t)=F_c(t)+F_s(t).
$$

Para el capacitor:

$$
F_c(t)=C\frac{dV_s(t)}{dt}.
$$

Por lo tanto:

$$
\boxed{F_e(t)=C\frac{dV_s(t)}{dt}+F_s(t)}.
$$

Despejando la derivada de la salida:

$$
\frac{dV_s(t)}{dt}=\frac{F_e(t)-F_s(t)}{C}.
$$

Integrando con condición inicial nula:

$$
\boxed{V_s(t)=\frac1C\int_0^t[F_e(\lambda)-F_s(\lambda)]\,d\lambda}.
$$

### Ley de voltajes de Kirchhoff en la entrada

El recorrido desde la fuente hasta tierra permite escribir la suma de caídas de voltaje:

$$
V_e(t)=R_1F_e(t)+V_s(t).
$$

Sustituyendo la expresión del capacitor:

$$
\boxed{V_e(t)=R_1F_e(t)+\frac1C\int_0^t[F_e(\lambda)-F_s(\lambda)]\,d\lambda}.
$$

### Ley de voltajes de Kirchhoff en la rama L–R2

Al recorrer la rama de respuesta se obtiene:

$$
\boxed{V_s(t)=L\frac{dF_s(t)}{dt}+R_2F_s(t)}.
$$

Combinando con el voltaje del capacitor:

$$
\boxed{\frac1C\int_0^t[F_e(\lambda)-F_s(\lambda)]\,d\lambda
=L\frac{dF_s(t)}{dt}+R_2F_s(t)}.
$$

Estas ecuaciones integro-diferenciales describen las dos ramas y los dos elementos independientes de almacenamiento de energía.

### Modelo de ecuaciones diferenciales ordinarias

La corriente de entrada también puede expresarse como:

$$
F_e(t)=\frac{V_e(t)-V_s(t)}{R_1}.
$$

Al sustituirla en la ley de corrientes:

$$
\boxed{\frac{dV_s(t)}{dt}=\frac{V_e(t)-V_s(t)}{R_1C}-\frac{F_s(t)}{C}}.
$$

De la rama L–R2:

$$
\boxed{\frac{dF_s(t)}{dt}=\frac{V_s(t)-R_2F_s(t)}{L}}.
$$

Los estados se seleccionan como z1 = Vs y z2 = Fs:

$$
\dot{\mathbf z}=
\begin{bmatrix}
-1/(R_1C)&-1/C\\
1/L&-R_2/L
\end{bmatrix}\mathbf z+
\begin{bmatrix}1/(R_1C)\\0\end{bmatrix}V_e,
$$

$$
V_s=\begin{bmatrix}1&0\end{bmatrix}\mathbf z,\qquad D=0.
$$

### Transformada de Laplace

Con las condiciones iniciales establecidas, las relaciones se transforman al dominio de s:

$$
V_e(s)=R_1F_e(s)+\frac{F_e(s)-F_s(s)}{Cs},
$$

$$
\frac{F_e(s)-F_s(s)}{Cs}=(Ls+R_2)F_s(s),
$$

$$
V_s(s)=(Ls+R_2)F_s(s).
$$

De la segunda ecuación:

$$
F_e(s)-F_s(s)=Cs(Ls+R_2)F_s(s).
$$

Despejando Fe(s):

$$
F_e(s)=[1+Cs(Ls+R_2)]F_s(s).
$$

Al introducir esta relación en la ecuación de la fuente:

$$
V_e(s)=R_1[1+Cs(Ls+R_2)]F_s(s)+(Ls+R_2)F_s(s).
$$

Desarrollando:

$$
V_e(s)=[R_1+R_1LCs^2+R_1R_2Cs+Ls+R_2]F_s(s).
$$

Ordenando el polinomio según la potencia de s:

$$
\boxed{V_e(s)=[R_1LCs^2+(L+R_1R_2C)s+(R_1+R_2)]F_s(s)}.
$$

La corriente de la rama de respuesta es:

$$
F_s(s)=\frac{V_e(s)}{R_1LCs^2+(L+R_1R_2C)s+(R_1+R_2)}.
$$

### Función de transferencia general

Como Vs(s) = (Ls + R2)Fs(s):

$$
V_s(s)=\frac{Ls+R_2}{R_1LCs^2+(L+R_1R_2C)s+(R_1+R_2)}V_e(s).
$$

Así se obtiene:

$$
\boxed{G(s)=\frac{V_s(s)}{V_e(s)}=
\frac{Ls+R_2}{R_1LCs^2+(L+R_1R_2C)s+(R_1+R_2)}}.
$$

El denominador es de segundo orden por la presencia del capacitor y del inductor. El numerador contiene un cero real en:

$$
s_z=-\frac{R_2}{L}.
$$

### Ecuación diferencial de segundo orden

De la relación entre entrada y salida en Laplace:

$$
[R_1LCs^2+(L+R_1R_2C)s+(R_1+R_2)]V_s(s)=(Ls+R_2)V_e(s).
$$

Aplicando la transformada inversa:

$$
\boxed{R_1LC\ddot V_s(t)+(L+R_1R_2C)\dot V_s(t)+(R_1+R_2)V_s(t)
=L\dot V_e(t)+R_2V_e(t)}.
$$

Para simular entradas con saltos, la formulación de dos estados evita calcular numéricamente la derivada del escalón.

## Evaluación de las dos condiciones

### Función de transferencia del control

Sustituyendo R1 = 1000 Ω, L = 0.1 H, C = 0.000001 F y R2 = 100000 Ω:

$$
R_1LC=(1000)(0.1)(10^{-6})=0.0001,
$$

$$
L+R_1R_2C=0.1+(1000)(100000)(10^{-6})=100.1,
$$

$$
R_1+R_2=1000+100000=101000.
$$

De este modo:

$$
\boxed{G_{control}(s)=\frac{0.1s+100000}{0.0001s^2+100.1s+101000}}.
$$

### Función de transferencia del caso

Sustituyendo R1 = 1000 Ω, L = 0.1 H, C = 0.001 F y R2 = 1000 Ω:

$$
R_1LC=(1000)(0.1)(0.001)=0.1,
$$

$$
L+R_1R_2C=0.1+(1000)(1000)(0.001)=1000.1,
$$

$$
R_1+R_2=1000+1000=2000.
$$

Así se obtiene:

$$
\boxed{G_{caso}(s)=\frac{0.1s+1000}{0.1s^2+1000.1s+2000}}.
$$

### Análisis de estabilidad

Las raíces del denominador se determinan a partir de:

$$
R_1LCs^2+(L+R_1R_2C)s+(R_1+R_2)=0.
$$

Definiendo:

$$
a=R_1LC,\qquad b=L+R_1R_2C,\qquad c=R_1+R_2,
$$

se aplica la fórmula general:

$$
\boxed{p_{1,2}=\frac{-b\pm\sqrt{b^2-4ac}}{2a}}.
$$

**Control:**

$$
p_{1,2}=\frac{-100.1\pm\sqrt{(100.1)^2-4(0.0001)(101000)}}{2(0.0001)}.
$$

$$
\boxed{p_1\approx-1010.010110,\qquad p_2\approx-999989.989890}\quad\mathrm{s}^{-1}.
$$

**Caso:**

$$
p_{1,2}=\frac{-1000.1\pm\sqrt{(1000.1)^2-4(0.1)(2000)}}{2(0.1)}.
$$

$$
\boxed{p_1\approx-2.000200,\qquad p_2\approx-9998.999800}\quad\mathrm{s}^{-1}.
$$

Los cuatro polos son reales y negativos. Ambas plantas son asintóticamente estables y sus denominadores corresponden a sistemas sobreamortiguados. En el plano complejo, todos los polos se ubican sobre el eje real negativo y tienen parte imaginaria igual a cero.

| Propiedad | Control | Caso |
| --- | ---: | ---: |
| Polo dominante | −1010.010110 s⁻¹ | −2.000200 s⁻¹ |
| Polo rápido | −999989.989890 s⁻¹ | −9998.999800 s⁻¹ |
| Cero | −1000000 s⁻¹ | −10000 s⁻¹ |
| Constante de tiempo dominante aproximada | 0.000990089 s | 0.499950 s |
| Valor final ante escalón de 1 V | 0.990099 V | 0.5 V |
| Error respecto a la entrada | 0.00990099 V | 0.5 V |

Las constantes de tiempo dominantes se estiman mediante tau = 1/|p1|. El caso es considerablemente más lento. El polo rápido y el cero están próximos, pero no se cancelan exactamente; para la implementación se conserva el modelo completo de segundo orden.

### Análisis del error en estado estacionario

Se aplica un escalón unitario:

$$
V_e(t)=u(t),\qquad V_e(s)=\frac1s.
$$

Para cada planta en lazo abierto se define el error entre entrada y salida:

$$
e(t)=V_e(t)-V_s(t).
$$

En Laplace:

$$
E(s)=V_e(s)-V_s(s),\qquad V_s(s)=G(s)V_e(s).
$$

Así se obtiene:

$$
E(s)=\frac{1-G(s)}{s}.
$$

Aplicando el teorema del valor final, válido para las plantas estables analizadas:

$$
e_{ss}=\lim_{s\to0}sE(s)=\lim_{s\to0}[1-G(s)]=1-G(0).
$$

La ganancia estática es:

$$
G(0)=\frac{L(0)+R_2}{R_1LC(0)^2+(L+R_1R_2C)(0)+(R_1+R_2)}
=\frac{R_2}{R_1+R_2}.
$$

De este modo:

$$
e_{ss}=1-\frac{R_2}{R_1+R_2}
=\frac{R_1+R_2-R_2}{R_1+R_2}
=\boxed{\frac{R_1}{R_1+R_2}}.
$$

**Control:**

$$
V_{s,control}(\infty)=\frac{100000}{101000}=\frac{100}{101}\approx0.990099\ \mathrm V,
$$

$$
e_{ss,control}=\frac{1000}{101000}=\frac1{101}\approx0.00990099.
$$

Para una entrada de 1 V, el error equivale a 0.00990099 V, aproximadamente 0.990099 % de la amplitud de entrada.

**Caso:**

$$
V_{s,caso}(\infty)=\frac{1000}{2000}=\frac12=0.5\ \mathrm V,
$$

$$
e_{ss,caso}=\frac{1000}{2000}=\frac12=0.5.
$$

Para una entrada de 1 V, el error equivale a 0.5 V, es decir, 50 %.

**Diferencia entre control y caso:**

La referencia para el controlador es la respuesta del control. La diferencia de lazo abierto entre ambas plantas es:

$$
e_{ref}(t)=V_{s,control}(t)-V_{s,caso}(t).
$$

Su valor estacionario es:

$$
\boxed{e_{ref}(\infty)=\frac{100}{101}-\frac12=\frac{99}{202}\approx0.490099\ \mathrm V}.
$$

Esta diferencia no debe confundirse con el error de cada planta respecto a la entrada unitaria. Tampoco se utiliza aquí la expresión 1/[1+G(0)], que corresponde a otra estructura: una planta dentro de un lazo de realimentación unitaria.

### Efecto de C y R2 sobre la dinámica

El capacitor aumenta de 1 µF a 1000 µF y R2 disminuye de 100 kΩ a 1 kΩ. En conjunto, estas modificaciones producen una respuesta más lenta en el caso. R2 también modifica el valor final, mientras que C afecta la respuesta transitoria.

### Respuesta temporal al escalón

Ante el escalón unitario, el control presenta una subida rápida hacia 0.990099 V y el caso una subida más lenta hacia 0.5 V. Ambas respuestas son no oscilatorias con estos parámetros.

## Simulación y comprobación

Las respuestas se obtienen ante un escalón de 1 V, con energía inicial nula en los elementos de almacenamiento y un intervalo de 0 a 10 segundos. Se utiliza el mismo estímulo para las ecuaciones, las funciones de transferencia y los circuitos de Simscape Electrical, de modo que pueda verificarse la consistencia de las representaciones.

En las figuras se identifican la entrada Ve(t) y las salidas del control y del caso. Si se incluye el sistema controlado, también se muestra Vs,PID(t). El tiempo se expresa en segundos y los voltajes en volts. Los valores calculados en este documento corresponden al modelo analítico; las gráficas y mediciones se incorporan al ejecutar y construir los circuitos.

### Uso del PID

La respuesta del control se toma como referencia para el caso. El PID se considera como una herramienta para disminuir la diferencia entre ambas respuestas. Su desempeño se revisa al comparar la referencia, la salida del caso en lazo abierto y la salida con controlador.

### Observación en laboratorio

La verificación experimental consiste en registrar Ve(t) y Vs(t) con el osciloscopio para cada circuito. Si se utiliza una señal cuadrada como aproximación al escalón, deben registrarse su amplitud, offset y periodo, dejando tiempo suficiente para observar el establecimiento de la salida.

Las posibles diferencias con el modelo ideal se relacionan con las tolerancias de los componentes, la resistencia interna del inductor, las características del capacitor y las impedancias del generador y de las sondas. Las capturas del osciloscopio y fotografías del montaje permiten documentar la prueba junto con los valores realmente utilizados.

## Conclusión

La representación eléctrica permitió describir dos respuestas estables del sistema endócrino. El caso presenta una evolución más lenta y una salida final menor que el control. Las ecuaciones y el análisis de polos explican estas diferencias y sirven como base para estudiar su seguimiento mediante un PID.

## Material que integra el repositorio

1. README con el desarrollo y los resultados analíticos.
2. Diagrama de BioRender y esquemas eléctricos de control y caso.
3. Cuaderno de MATLAB, modelos de Simulink y circuitos de Simscape Electrical.
4. Programa de Python y figuras de las simulaciones.
5. Evidencia del controlador PID utilizado en la práctica.
6. Fotografías del montaje, capturas del osciloscopio y comparación experimental.
7. Archivos complementarios y CITATION.cff.

**Palabras clave:** Sistema endócrino; Circuito equivalente; Kirchhoff; Modelado fisiológico; Polos; Error estacionario; PID; Simulación.

## Referencias


[1] P. A. Valle, *Syllabus para Modelado de Sistemas Fisiológicos*, Tecnológico Nacional de México / Instituto Tecnológico de Tijuana, Tijuana, B.C., México, 2026. Apartado 5.6: Modelado y control del sistema endócrino.

[2] M. C. Khoo, *Physiological Control Systems: Analysis, Simulation, and Estimation*, 2nd ed., IEEE Press, 2018.

[3] F. C. Hoppensteadt y C. S. Peskin, *Modeling and Simulation in Medicine and the Life Sciences*, Springer.

[4] J. E. Hall, *Guyton and Hall Textbook of Medical Physiology*, 14th ed., Elsevier, 2021.

[5] K. Ogata, *Ingeniería de Control Moderna*, 5.ª ed., Pearson Educación, 2010.

[6] N. S. Nise, *Control Systems Engineering*, 8th ed., John Wiley & Sons, 2020.
