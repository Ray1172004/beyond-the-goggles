🌐 **Idioma / Language:** **Español** | [English](README_EN.md)

# 01 — Radiocontrol: ExpressLRS 2.4 GHz

[← Volver al proyecto](../README.md) · [Presentación PDF](../presentation/BEYOND_THE_GOGGLES.pdf)

## Objetivo y alcance

Documentar la observación y caracterización RF de un **enlace de radiocontrol ExpressLRS de 2.4 GHz** con HackRF One e Inspectrum. El propósito es analizar la actividad, los tiempos y el comportamiento en frecuencia **sin decodificar el contenido de los paquetes ni modificar el firmware del dron**.

Se distingue entre observaciones directas del experimento y conocimientos generales del protocolo. Encontrar actividad en 2.4 GHz no demuestra, por sí solo, que una señal corresponda a ExpressLRS.

## Equipos y software

| Elemento | Función |
| --- | --- |
| BetaFPV Meteor75 | Dron de pruebas con ExpressLRS 2.4 GHz |
| RadioMaster Pocket | Transmisor de radiocontrol |
| HackRF One | Captura IQ y observación espectral |
| Inspectrum | Análisis temporal y espectral de capturas |
| SDR++ | Visualización adicional de señales |
| M5Stick-C + NRF24L01 | Experimento independiente en 2.4 GHz |
| HackRF + PortaPack H2 + Mayhem | Plataforma RF experimental adicional |

### Evidencia fotográfica

![Control RadioMaster](images/control-radiomaster.jpeg)

![Equipos de la investigación](images/materiales.jpeg)

## 1. Arquitectura experimental

```text
RadioMaster Pocket ─── ExpressLRS 2.4 GHz ───> Meteor75
                              │
                        Observación RF
                              ▼
                          HackRF One
                              │
                         Muestras IQ
                              ▼
                          Inspectrum
                              │
                    Análisis temporal y RF
```

El SDR captura una porción del espectro como muestras IQ. Inspectrum permite inspeccionar la grabación posteriormente. La ruta de observación es independiente del enlace normal de control del dron.

## 2. Actividad RF: transmisor apagado y encendido

Se compararon dos estados:

1. **Apagado:** no se observó la actividad característica asociada al transmisor experimental en la captura.
2. **Encendido:** apareció actividad RF en la banda observada.

La comparación permitió correlacionar el estado del transmisor con la actividad registrada, dentro de las condiciones de prueba. La banda de 2.4 GHz también puede contener emisiones Wi-Fi, Bluetooth y de otros sistemas RC.

![Experimento de 2.4 GHz](images/2.4ghz-poc.jpeg)

## 3. Tasa de paquetes y periodicidad temporal

La captura IQ fue examinada en Inspectrum. Se documentaron los siguientes valores aproximados:

| Medición | Valor |
| --- | --- |
| Período medido | **3.968 ms** |
| Frecuencia derivada | **252.016 Hz** |
| Tasa ELRS configurada | **250 Hz** |
| Intervalo nominal a 250 Hz | **4 ms** |

La relación matemática es:

```text
frecuencia (Hz) = 1 / período (segundos)
1 / 0.003968 ≈ 252.016 Hz
1 / 0.004    = 250 Hz
```

La periodicidad observada es coherente con el modo de 250 Hz configurado, considerando la resolución y las condiciones de medición.

**Precaución:** las líneas brillantes del espectrograma no deben interpretarse automáticamente como paquetes completos. El resultado corresponde a una periodicidad medida, no a paquetes decodificados.

![Observación espectral de 2.4 GHz](images/expectro-2.4ghz.jpeg)

## 4. Salto de frecuencia

ExpressLRS utiliza mecanismos de salto de frecuencia. En el análisis RF puede observarse actividad en distintas posiciones del espectro a lo largo del tiempo. Las características relevantes incluyen:

- Frecuencia central y ancho de banda ocupado.
- Duración de las señales y período de repetición.
- Cambios de frecuencia durante la ventana de observación.
- Nivel de ruido, interferencias y relación señal-ruido.

No se decodificó el contenido de los paquetes ni se reconstruyó una secuencia completa de saltos. Una captura SDR limitada puede mostrar únicamente parte del patrón.

## 5. Captura IQ y análisis

Flujo conceptual:

```text
Emisión RF → HackRF One → Archivo IQ → Inspectrum → Caracterización RF
```

Ejemplo de comando (sustituir los parámetros según el montaje):

```bash
hackrf_transfer -r capture.iq -f <CENTER_FREQUENCY_HZ> -s <SAMPLE_RATE_SPS>
```

Este comando es una **plantilla**, no un registro de los parámetros exactos utilizados en la prueba.

## 6. Experimento con NRF24L01

Se utilizó un **M5Stick-C con NRF24L01** en un experimento independiente para producir u observar actividad en la banda de 2.4 GHz y comparar un transceptor dedicado con una plataforma SDR.

El NRF24L01 trabaja con configuraciones de canales discretos, mientras que un SDR ofrece otras posibilidades de muestreo y procesamiento. Son arquitecturas diferentes y sus efectos no pueden compararse únicamente por potencia o canal.

En determinadas condiciones se observó una degradación de la calidad del enlace ELRS. Esto **no demuestra** que el NRF24L01 haya decodificado ni tomado control de ExpressLRS.

## 7. HackRF + PortaPack / Mayhem

También se utilizó HackRF One con PortaPack H2 y Mayhem. Entre los conceptos de configuración examinados se encuentran:

| Parámetro | Función conceptual |
| --- | --- |
| WIDTH | Espacio de frecuencia configurado |
| TYPE | Modo de señal seleccionado |
| HOP | Movimiento entre segmentos de frecuencia |
| TX | Duración de transmisión |
| SLEEP | Pausa entre operaciones |
| JITTER | Variación temporal |
| GAIN | Ajuste de ganancia |

Son descripciones generales, no una receta universal de configuración. Su funcionamiento exacto depende de la aplicación y versión de Mayhem.

## 8. Comparación experimental ELRS frente a TBS

En una prueba RF independiente, **ELRS presentó una reducción de la calidad de enlace (LQ)**, mientras que el enlace **TBS evaluado no mostró una disminución comparable**.

No debe interpretarse como inmunidad, vulnerabilidad ni superioridad universal. La potencia, sensibilidad del receptor, antenas, distancia, frecuencia, diversidad y geometría del montaje pueden cambiar los resultados.

## 9. Contexto de seguridad y limitaciones

NCC Group publicó en 2022 investigaciones sobre problemas relacionados con la vinculación y la generación de secuencias de salto de frecuencia en ExpressLRS 1.x y 2.x. **Este proyecto no reproduce ese ataque**: se centra en observación y caracterización RF.

### Conclusiones

- HackRF e Inspectrum permitieron caracterizar aspectos observables del enlace experimental de 2.4 GHz.
- El período de **3.968 ms** fue coherente con una tasa configurada de **250 Hz**.
- La frecuencia y la energía observada no bastan para identificar un protocolo.
- Los cambios en la calidad del enlace dependen del equipo y las condiciones de prueba.
- La observación RF es una primera etapa útil para investigar seguridad, pero no equivale a decodificación ni explotación.

---

[← Volver a Beyond the Goggles](../README.md) · [Investigación VTX analógico →](../02-vtx-analog-video/README.md)
