# Modelo Dinámico Discreto de un Sistema de Control Difuso para Temperatura y Humedad

## Descripción del Proyecto
[cite_start]Este proyecto implementa un modelo dinámico en tiempo discreto para describir la evolución de la temperatura y la humedad en un recinto[cite: 1, 3]. [cite_start]El sistema considera la acción de un ventilador cuya velocidad es determinada por un controlador difuso[cite: 4]. 

[cite_start]El objetivo principal de este modelo es analizar la interacción entre las variables físicas, las perturbaciones externas y la acción de control[cite: 5]. [cite_start]Es importante destacar que el ventilador no modifica directamente la temperatura ni la humedad final, sino que actúa como un acelerador del equilibrio con el ambiente exterior, haciendo que el sistema converja más rápidamente hacia las condiciones externas conforme aumenta la velocidad[cite: 41, 42, 51, 53, 55].

## Requisitos de Software
* **MATLAB** (Probado con la versión R20XXx)
* **Simulink**
* **Fuzzy Logic Toolbox** (Requerido para el bloque *Fuzzy Logic Controller*)

## Ecuaciones Principales
[cite_start]El sistema de control difuso toma como entradas los errores de seguimiento para decidir el valor de la señal de control del ventilador[cite: 13]:
* [cite_start]**Error de Temperatura:** $e_{T}[k]=T[k]-T_{d}[k]$ [cite: 8]
* [cite_start]**Error de Humedad:** $e_{H}[k]=H[k]-H_{d}[k]$ [cite: 8]
* [cite_start]**Ley de Control Difuso:** $u_{f}[k]=\mathcal{F}(e_{T}[k],e_{H}[k])$, donde el valor está normalizado entre 0 (apagado) y 1 (velocidad máxima)[cite: 16, 19, 20].

## Parámetros del Sistema (Caso de Prueba Básico)
[cite_start]El modelo está configurado para un escenario simple con perturbaciones constantes usando los siguientes parámetros[cite: 64, 113]:

**Dinámica del Sistema:**
* [cite_start]Periodo de muestreo: $\Delta t=1$ [cite: 114]
* [cite_start]Constantes de tiempo: $\tau_{T}=30$, $\tau_{H}=40$ [cite: 116]
* [cite_start]Coeficientes del ventilador: $\alpha_{T}=0,08$, $\alpha_{H}=0,06$ [cite: 118]
* [cite_start]Coeficientes de fuentes internas: $\beta_{T}=0,02$, $\beta_{H}=0,03$ [cite: 120]

**Condiciones y Referencias:**
* [cite_start]Condiciones Iniciales: $T[0]=30$ °C, $H[0]=70$ % [cite: 123, 124]
* [cite_start]Referencias Deseadas: $T_{d}=24$ °C, $H_{d}=50$ % [cite: 127, 128]
* [cite_start]Condiciones Externas: $T_{ext}=22$ °C, $H_{ext}=45$ % [cite: 131, 132]
* [cite_start]Perturbaciones Internas: $Q_{int}=2$, $W_{int}=1$ [cite: 135, 136]

## Estructura de Simulink
El archivo `.slx` contiene un lazo cerrado de control estructurado de la siguiente manera:
1. **Cálculo de Error:** Diferencia entre los estados actuales ($T[k]$, $H[k]$) y las referencias ($T_d$, $H_d$).
2. **Controlador Difuso:** Bloque que procesa los errores y emite la señal de control $u_f[k]$.
3. **Planta Dinámica:** Un bloque `MATLAB Function` que calcula el siguiente estado ($T[k+1]$, $H[k+1]$) usando las ecuaciones en diferencias del sistema.
4. **Memoria:** Bloques *Unit Delay* (`1/z`) que almacenan el estado y lo retroalimentan al sistema con sus respectivas condiciones iniciales.

## Instrucciones de Uso
1. **Definir Variables:** Antes de abrir Simulink, ejecuta el siguiente script en la ventana de comandos de MATLAB para cargar los parámetros en el Workspace:
   ```matlab
   dt = 1; 
   tau_T = 30; tau_H = 40; 
   alpha_T = 0.08; alpha_H = 0.06; 
   beta_T = 0.02; beta_H = 0.03;
