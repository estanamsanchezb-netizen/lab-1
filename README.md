# Lab-1
## Objetivo de la preactica 
Analizar estadísticamente una señal fisiológia, calcular sus principales estadísticos, y comparar los resultados entre señales adquiridas mediante DAQ y descargada por PhysioNet, aplicando el concepto señal-ruido.
## Procedimiento 
### Parte A
1. Se desacrgo una señal fisiológica de ecg mediante la pagina physioNet (https://physionet.org/content/norwegian-athlete-ecg/1.0.0/). Se uso la señal del Norwegian Endurance Athlete ECG Database.
```python
import numpy as np
import matplotlib.pyplot as plt

!pip install wfdb
import wfdb
from scipy.stats import kurtosis
from scipy.stats import gaussian_kde
import pandas as pd
from scipy.stats import skew



from google.colab import drive
drive.mount('/content/drive')



signals,fields=wfdb.rdsamp('/content/ath_001')
signals
vaca=signals[:,1]
plt.plot(vaca)
plt.xlabel("Numero de muestras")
plt.ylabel("Voltaje (mV)")
plt.title("Grafica A")
plt.axis([0,2000,-1,1])
plt.show()
```
<br>
<img width="631" height="450" alt="image" src="https://github.com/user-attachments/assets/7ab0d389-58e0-4d40-9f68-d72217ba2cb5" />


2. La señal fue graficada en el dominio del tiempo.
3. Cálculo de estadísticos descriptivos:

   -Media:  -0.19830211599999997
   
   -Mediana:  -0.2217
   
   -Desviación estándar de la población: 0.12021093861742593
   
   -Desviación estándar de la muestra :0.12022296151475233
   
   -Coeficiente de variación: -60.62009878776379 %
   
   -Curtosis: 14.570386479062549
```python
suma=0
for n in vaca:
  suma= suma+n
contador=0
for c in vaca:
  contador= contador+1
media=suma/contador
print(f"La media es:{media}")

suma2 = 0
contador2 = 0
for num in vaca:
    suma2= suma2+(num-media) ** 2
for c in vaca:
  contador2= contador2+1

varianza =suma2 / (contador2)
desviacion_p=(varianza)**(1/2)
print(f"La Desviacion estandar de la población es: {desviacion_p}")

suma3 = 0
contador3 = 0
for numer in vaca:
    suma3= suma3+(numer-media) ** 2
for c in vaca:
  contador3= contador3+1

varianza =suma3 / (contador3-1)
desviacion_m= (varianza)**(1/2)
print(f"La Desviacion estandar de la muestra es: {desviacion_m}")

c_variacion=(desviacion_p/media)*100
print(f"El coeficiente de variación es: {c_variacion} %")

asimetria = np.mean(((vaca - media) / desviacion_p)**3)
print(f"La asimetría es igual a {asimetria}")
```
4. Se grafico el histograma y la función de probabilidad
 ```python
cuentas, bins, _=plt.hist(vaca, bins=50, color='pink', edgecolor='magenta', density=True)
#funcion de probabilidad
funcion_prob=0.5*(bins[1:]+ bins[:-1])
plt.plot(funcion_prob, cuentas, color='purple', linewidth=1, label="Funcion de probabilidad")

#histograma
plt.title("Histograma de la señal ECG, descargada")
plt.ylabel("Densidad de probabilidad (1/V)")
plt.xlabel("Amplitud de la señal (V)")
plt.grid()
plt.legend()
plt.show()
```


<img width="599" height="446" alt="image" src="https://github.com/user-attachments/assets/61c66984-6773-462d-8425-c6dc2654befc" />
<br>


<img width="157" height="894" alt="image" src="https://github.com/user-attachments/assets/2eee55fc-0fb8-4d5c-9268-b3c8f3367f45" />
<br>
Imagen 1. Diagrama de flujo parte A

### Parte B 
1. Se generó una señal ECG utilizando el generador de señales.
2. Se realizó la captura mediante el uso del sistema DAQ. En Spyder se ejecutó el código de captura y se guardó el archivo en formato TXT.
3. La señal fue graficada en el dominio tiempo vs voltaje 

```
import numpy as np
import matplotlib.pyplot as plt

ruta = "/content/captura_señal_fs2000_duracion5.txt"

datos = np.loadtxt(ruta, comments="#", encoding="latin1")  # <-- clave

t = datos[:, 0]
v = datos[:, 1]

plt.figure()
plt.plot(t, v)
plt.title("Señal vs Tiempo")
plt.xlabel("Tiempo (s)")
plt.ylabel("Señal (V)")
plt.grid(True)
plt.tight_layout()
plt.show()
```


4. Se calculo los estadísticos.
 
```

media2=np.mean(v)
mediana2=np.median(v)
desviacion_poblacion2=np.std(v)
desviacion_muestra2=np.std(v,ddof=1)
curtosis2=kurtosis(v)
coefvar2=(desviacion_poblacion/media)*100
asimetria2 = skew(v)




print(f"media igual a {media2}")
print(f"mediana igual a {mediana2}")
print(f"la desviacion estandar de la poblacion es igual a {desviacion_poblacion2}")
print(f"la desviacion estandar de la muestra es igual a {desviacion_muestra2}")
print(f"la curtosis es igual a {curtosis2}")
print(f"el coeficiente de variacion es igual a {coefvar2} %")
print(f"La asimetría es igual a {asimetria2}")


```
5. Se grafico el histograma
  ``` 
import matplotlib.pyplot as plt

datos = v  # tu señal

plt.figure(figsize=(10,5))
plt.hist(datos, bins=50, edgecolor="black", linewidth=0.6, alpha=0.85)
plt.title("Histograma de la señal")
plt.xlabel("Amplitud (V)")
plt.ylabel("Frecuencia")
plt.grid(True, alpha=0.3)
plt.tight_layout()
plt.show()
```

### Parte C 
¿Qué es la relación señal-ruido?
La relación señal-ruido es una medida que indica qué tan fuerte es una señal útil en comparación con el ruido que la afecta.
1.a. Se contaminó la señal con ruido gaussiano

```
import numpy as np
import matplotlib.pyplot as plt

import numpy as np
import matplotlib.pyplot as plt


ruido = np.random.normal(9, 9, size=v.shape)
y = v + ruido

# SNR
Psignal = np.mean(v**2)
Pnoise  = np.mean((y - v)**2)
SNR_dB  = 10 * np.log10(Psignal / Pnoise)
print("SNR (dB):", SNR_dB)

# Plot
plt.figure(figsize=(10,5))
plt.plot(t, v, label="Señal original")
plt.plot(t, y, label="Señal con ruido", alpha=0.7)
plt.title(f"Señal con y sin ruido (SNR ≈ {SNR_dB:.2f} dB)")
plt.xlabel("Tiempo (s)")
plt.ylabel("Amplitud")
plt.legend()
plt.grid(True, alpha=0.3)
plt.tight_layout()
plt.show()

```

<img width="599" height="446" alt="image" src="https://github.com/user-attachments/assets/61c66984-6773-462d-8425-c6dc2654befc" />
<br>


1.b. Se contaminó la señal con ruido tipo impulso:

```
import numpy as np
import matplotlib.pyplot as plt


x = v

prob = 0.01                 # porcentaje de puntos alterados (5%)
amplitud = np.max(np.abs(x)) * 0.5   # tamaño del impulso

mask = np.random.rand(len(x)) < prob
ruido = np.zeros_like(x)
ruido[mask] = np.random.choice([+amplitud, -amplitud], size=mask.sum())
y = x + ruido

# SNR
Psignal = np.mean(x**2)
Pnoise  = np.mean((y - x)**2)
SNR_dB  = 10*np.log10(Psignal / Pnoise)
print(f"SNR (dB): {SNR_dB:.2f}")

# Plot
plt.figure(figsize=(10,5))
plt.plot(t, x, label="Señal original")
plt.plot(t, y, label="Señal con ruido impulso", alpha=0.8)
plt.xlabel("Tiempo (s)")
plt.ylabel("Amplitud")
plt.title(f"Señal con ruido impulso (SNR ≈ {SNR_dB:.1f} dB)")
plt.legend()
plt.grid(True, alpha=0.3)
plt.tight_layout()
plt.show()

```

1.c. Se contaminó la señal con ruido tipo artefacto:


```
import numpy as np
import matplotlib.pyplot as plt

# Asumiendo que ya tienes:
# t = tiempo
# v = señal

t = t
x = v

f_bw = 0.3                 # Hz (deriva de línea base)
A_bw = 0.3*np.std(x)       # amplitud de deriva (ajusta 0.1–0.5)

f_pl = 60.0                # Hz (interferencia de red; en algunos países es 50)
A_pl = 0.1*np.std(x)       # amplitud de red (ajusta 0.05–0.3)

artefacto = A_bw*np.sin(2*np.pi*f_bw*t) + A_pl*np.sin(2*np.pi*f_pl*t)
y = x + artefacto

# SNR
Psignal = np.mean(x**2)
Pnoise  = np.mean((y - x)**2)
SNR_dB  = 10*np.log10(Psignal / Pnoise)
print(f"SNR (dB): {SNR_dB:.2f}")

# Plot
plt.figure(figsize=(12,5))
plt.plot(t, x, label="Señal original", linewidth=1.2)
plt.plot(t, y, label="Con artefacto", alpha=0.8, linewidth=1.0)
plt.xlabel("Tiempo (s)")
plt.ylabel("Amplitud")
plt.title(f"Señal con ruido tipo artefacto (SNR ≈ {SNR_dB:.1f} dB)")
plt.legend()
plt.grid(True, alpha=0.3)
plt.tight_layout()
plt.show()


```
