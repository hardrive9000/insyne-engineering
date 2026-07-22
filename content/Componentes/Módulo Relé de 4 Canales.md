# Módulo Relé de 4 Canales

El Módulo Relé de 4 Canales es una placa de expansión que integra cuatro relés electromecánicos independientes junto con su electrónica de accionamiento, permitiendo controlar cargas de mayor tensión o corriente desde un microcontrolador.

Habitualmente incorpora optoacopladores, transistores de potencia, diodos de rueda libre y LEDs indicadores de estado para cada canal.

---

## Características principales

- 4 relés independientes
- Bobinas de 5 V
- Entradas compatibles con microcontroladores
- Salidas mediante contactos secos
- LEDs indicadores por canal
- Diodos de protección
- Optoaislamiento (según modelo)

---

## Especificaciones

| Parámetro            | Valor         |
| -------------------- | ------------- |
| Canales              | 4             |
| Alimentación         | 5 V           |
| Corriente por bobina | ~70 mA        |
| Contactos            | COM / NO / NC |
| Tipo de relé         | SPDT          |

---

## Pinout

Cada relé dispone de tres contactos:

| Pin | Descripción         |
| --- | ------------------- |
| COM | Común               |
| NO  | Normalmente Abierto |
| NC  | Normalmente Cerrado |

---

## Entradas

Habitualmente:

| Pin | Descripción         |
| --- | ------------------- |
| VCC | Alimentación lógica |
| GND | Tierra              |
| IN1 | Canal 1             |
| IN2 | Canal 2             |
| IN3 | Canal 3             |
| IN4 | Canal 4             |

---

## Funcionamiento

Cada entrada controla una bobina independiente.

Al activarse:

- se energiza la bobina;
- cambia el estado de los contactos;
- se enciende el LED indicador correspondiente.

---

## Alimentación

El módulo requiere una alimentación estable de 5 V.

Cada relé consume aproximadamente 70 mA cuando la bobina está energizada.

Con los cuatro relés activados simultáneamente el consumo puede superar los 250 mA.

---

## Hojas de Datos

- [[4_Channel_Relay_Module_Datasheet.pdf]]

---

## Utilizado en

Los proyectos que utilicen este componente aparecerán automáticamente mediante los Backlinks de Obsidian / Quartz.

---

# Notas de Ingeniería

## Generales

### Active LOW vs Active HIGH

Muchos módulos comerciales utilizan lógica activa por nivel bajo.

Siempre verificar este comportamiento antes de desarrollar el firmware.

---

### Consumo

Las bobinas representan el mayor consumo del módulo.

Conviene dimensionar la fuente considerando el peor caso, con todos los relés energizados simultáneamente.

---

### Contactos

Los contactos COM, NO y NC son eléctricamente independientes de la electrónica de control.

Esto permite conmutar tensiones distintas a la alimentación lógica del módulo, respetando siempre las especificaciones del fabricante.

---

### Vida útil

La vida útil del relé depende principalmente del tipo de carga conmutada.

Las cargas inductivas producen un desgaste considerablemente mayor que las cargas resistivas.

## [[PowerHub]]

### Lógica invertida

El módulo utilizado en PowerHub es de **entrada activa por nivel bajo (Active LOW)**.

En Tasmota fue necesario configurar:

- Relay1_i
- Relay2_i
- Relay3_i
- Relay4_i

para invertir la lógica de accionamiento.

---

### Alimentación

Los cuatro relés son alimentados desde la misma fuente de 5 V que el resto del sistema.

Durante las pruebas no se observaron caídas de tensión ni reinicios del ESP8266.

---

### Asignación de GPIO

Configuración utilizada:

| Canal   | GPIO   |
| ------- | ------ |
| Relay 1 | GPIO14 |
| Relay 2 | GPIO12 |
| Relay 3 | GPIO13 |
| Relay 4 | GPIO16 |

---

### Integración con Tasmota

El firmware oficial reconoce el módulo sin necesidad de modificaciones.

Toda la lógica de funcionamiento fue implementada mediante Rules.

No fue necesario desarrollar firmware personalizado.

---

### Display

Cada cambio de estado genera una actualización inmediata del display LCD.

La sincronización se realiza mediante eventos y variables internas de Tasmota.

---

### Estado después del reinicio

PowerHub utiliza:

```
PowerOnState 3
```

permitiendo restaurar automáticamente el último estado conocido de los cuatro relés.

---

### Control

Los relés pueden accionarse mediante:

- Pulsador físico
- Control remoto IR
- Interfaz Web
- HTTP
- MQTT (si se habilita)

Todas las interfaces mantienen el display sincronizado.

---

# Buenas prácticas

- Identificar claramente cada canal.
- Verificar el tipo de lógica (Active HIGH / Active LOW).
- Mantener separados los conductores de potencia y señal.
- Revisar periódicamente el torque de los bornes.
- Documentar la asignación de cada relé dentro del proyecto.

---

# Lecciones Aprendidas

- Los módulos Active LOW simplifican la protección frente a estados indeterminados durante el arranque.
- La lógica de inversión puede resolverse completamente desde Tasmota sin hardware adicional.
- El uso de Rules permite desacoplar la interfaz de usuario de la lógica de control.
- Sincronizar el display con los eventos de los relés mejora significativamente la experiencia de uso.

---

# Referencias

Songle — SRD-05VDC-SL-C Relay Datasheet.

Tasmota Documentation.