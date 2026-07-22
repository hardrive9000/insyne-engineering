# ESP8266 (NodeMCU V3)

ESP8266 (NodeMCU V3) es una placa de desarrollo basada en el microcontrolador ESP8266EX de Espressif Systems. Integra conectividad Wi-Fi IEEE 802.11 b/g/n, interfaz USB-UART mediante CH340 (o CP2102 según el fabricante) y expone la mayoría de los GPIO del ESP8266 en un formato cómodo para prototipado.

---

## Características principales

- Microcontrolador: ESP8266EX
- Arquitectura: Tensilica L106 (32 bits)
- Frecuencia: 80 MHz (160 MHz mediante overclock)
- Memoria RAM: 160 KB
- Memoria Flash: 4 MB (habitualmente)
- Wi-Fi 2.4 GHz IEEE 802.11 b/g/n
- Alimentación:
  - Micro USB (5V)
  - Pin VIN (5V)
  - Pin 3V3 (3.3V regulados) | [[AMS1117]]-3.3 (800mA)
- Nivel lógico de GPIO: 3.3V

---

## Interfaces

- Wi-Fi
- UART
- SPI
- I2C (implementación por software)
- I2S
- PWM
- ADC (1 canal)

---

## GPIO importantes

| GPIO       | Función | Observaciones                                                   |
| ---------- | ------- | --------------------------------------------------------------- |
| GPIO0      | Boot    | Debe permanecer en HIGH durante el arranque normal.             |
| GPIO1 (TX) | UART TX | Puede reutilizarse como GPIO si no se utiliza la consola serie. |
| GPIO2      | Boot    | Debe permanecer en HIGH durante el arranque.                    |
| GPIO3 (RX) | UART RX | Puede utilizarse como GPIO si no se utiliza la consola serie.   |
| GPIO15     | Boot    | Debe permanecer en LOW durante el arranque.                     |
| GPIO16     | Wake-up | Utilizado para despertar desde Deep Sleep.                      |

Los GPIO0, GPIO2 y GPIO15 participan en la secuencia de arranque del ESP8266.

Siempre que sea posible, reservar estos pines para funciones que no alteren sus niveles lógicos durante el boot.

---

## Alimentación

La placa incorpora un regulador de 3.3V, generalmente [[AMS1117]]-3.3 (800mA), que alimenta el ESP8266 a partir de los 5V provenientes del conector Micro USB o del pin VIN.

Los GPIO **no son tolerantes a 5V**.

---

## Hojas de Datos

- [[ESP8266EX_Datasheet.pdf]]
- [[NodeMCU_V3_Datasheet.pdf]]

---

## Utilizado en

Los proyectos que utilicen este componente aparecerán automáticamente mediante los Backlinks de Obsidian / Quartz.

---

# Notas de Ingeniería

## Generales

### GPIO críticos

Los GPIO0, GPIO2 y GPIO15 determinan el modo de arranque del ESP8266.

Durante el diseño de hardware resulta recomendable reservar estos pines para funciones que no modifiquen sus niveles lógicos durante el boot.

---

### Reutilización de UART

Cuando no se utiliza la consola serie, los pines TX (GPIO1) y RX (GPIO3) pueden reutilizarse como GPIO convencionales.

Esta estrategia permite liberar GPIO menos flexibles para periféricos más complejos.

---

### Bus I2C

El ESP8266 no incorpora un periférico I2C dedicado.

La implementación se realiza completamente por software, permitiendo asignar SDA y SCL prácticamente a cualquier GPIO.

---

### GPIO de 3.3 V

Todos los GPIO operan exclusivamente a 3.3 V.

No son tolerantes a 5 V.

Cualquier periférico conectado debe respetar este nivel lógico o utilizar adaptación de niveles.

## [[PowerHub]]

### Pin TX (GPIO1)

En PowerHub se utiliza como entrada para un pulsador normalmente abierto.

Condiciones necesarias:

- SerialLog deshabilitado.
- Consola serie no utilizada.
- Pulsador activo por nivel bajo.

Durante el boot el ESP8266 transmite información por UART, pero esto no afecta el funcionamiento del pulsador una vez iniciado Tasmota.

Esta estrategia permite liberar GPIO menos flexibles para periféricos más complejos.

---

### Pin D4 (GPIO2)

En PowerHub se utiliza para conectar un receptor IR [[VS1838B]] ya que el mismo en ausencia de señal infrarroja presenta en su salida un nivel lógico alto que no interfiere con el proceso de arranque del ESP8266 (GPIO2 debe permanecer en HIGH durante el arranque).

---

### I2C

En PowerHub se asignó:

- GPIO5 → SCL
- GPIO4 → SDA

Ver [[Módulo Adaptador I2C Display LCD PCF8574]] para detalles sobre la implementación del bus I2C.

---

### Firmware

Probado con:

- Tasmota 15.5.0 (tasmota-display)

Funcionamiento estable.

---

## Buenas prácticas

- Evitar utilizar GPIO críticos cuando existan alternativas.
- Documentar siempre la asignación de pines del proyecto.
- Reservar UART únicamente cuando sea necesaria para depuración.
- Deshabilitar SerialLog si se reutiliza TX o RX.

---
# Lecciones Aprendidas

- Liberar GPIOs críticos desde el inicio simplifica futuras ampliaciones del hardware.
- La reutilización inteligente de TX y RX puede evitar rediseños posteriores.
- Documentar el motivo de cada asignación de GPIO resulta tan importante como documentar la asignación misma.
- El conocimiento práctico sobre el comportamiento durante el boot tiene tanto valor como el datasheet oficial.

---
## Referencias

Espressif Systems — ESP8266EX Datasheet

NodeMCU DevKit V3 Documentation