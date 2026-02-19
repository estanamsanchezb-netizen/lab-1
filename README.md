# Lab-1
## Objetivo de la preactica 
Analizar estadísticamente una señal fisiológia, calcular sus principales estadísticos, y comparar los resultados entre señales adquiridas mediante DAQ y descargada por PhysioNet, aplicando el concepto señal-ruido.
## Procedimiento 
### Parte A
1. Se desacrgo una señal fisiológica de ecg mediante la pagina physioNet (https://physionet.org/content/norwegian-athlete-ecg/1.0.0/). Se uso la señal del Norwegian Endurance Athlete ECG Database.
```python
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

5. Se grafico el histograma y la función de probabilidad 
### Parte B 
### Parte C 
¿Qué es la relación señal-ruido?
La relación señal-ruido es una medida que indica qué tan fuerte es una señal útil en comparación con el ruido que la afecta.
