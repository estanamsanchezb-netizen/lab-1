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


<img width="599" height="446" alt="image" src="https://github.com/user-attachments/assets/61c66984-6773-462d-8425-c6dc2654befc" />
<br>



### Parte C 
¿Qué es la relación señal-ruido?
La relación señal-ruido es una medida que indica qué tan fuerte es una señal útil en comparación con el ruido que la afecta.
