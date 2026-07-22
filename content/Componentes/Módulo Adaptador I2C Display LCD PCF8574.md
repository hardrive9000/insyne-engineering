# Módulo Adaptador I2C Display LCD PCF8574

El Módulo Adaptador I2C Display LCD PCF8574 permite controlar displays LCD compatibles con el controlador Hitachi HD44780 utilizando únicamente dos líneas del bus I2C (SDA y SCL), reduciendo considerablemente la cantidad de GPIO necesarios.

El módulo está basado en el expansor de entradas/salidas PCF8574 de NXP (o compatibles), el cual proporciona ocho líneas de propósito general accesibles mediante el bus I2C.

---

## Características principales

- Circuito integrado: PCF8574
- Expansor de E/S: 8 bits
- Comunicación: I2C
- Alimentación: 2.5 V – 6 V
- Compatible con displays LCD HD44780
- Control del backlight mediante transistor integrado
- Consumo extremadamente bajo

---

## Direcciones I2C

El PCF8574 permite configurar la dirección I2C mediante los puentes A0, A1 y A2.

### PCF8574

| A2 | A1 | A0 | Dirección |
|----|----|----|-----------|
| 0 | 0 | 0 | 0x20 |
| 0 | 0 | 1 | 0x21 |
| 0 | 1 | 0 | 0x22 |
| 0 | 1 | 1 | 0x23 |
| 1 | 0 | 0 | 0x24 |
| 1 | 0 | 1 | 0x25 |
| 1 | 1 | 0 | 0x26 |
| 1 | 1 | 1 | 0x27 |

### PCF8574A

| A2 | A1 | A0 | Dirección |
|----|----|----|-----------|
| 0 | 0 | 0 | 0x38 |
| 0 | 0 | 1 | 0x39 |
| 0 | 1 | 0 | 0x3A |
| 0 | 1 | 1 | 0x3B |
| 1 | 0 | 0 | 0x3C |
| 1 | 0 | 1 | 0x3D |
| 1 | 1 | 0 | 0x3E |
| 1 | 1 | 1 | 0x3F |

---

## Pinout

| Pin | Descripción  |
| --- | ------------ |
| GND | Tierra       |
| VCC | Alimentación |
| SDA | Bus I2C      |
| SCL | Bus I2C      |

---

## Compatibilidad

Compatible con:

- [[Display LCD 1602 Hitachi HD44780]]
- [[ESP8266 (NodeMCU V3)]]
- ESP32
- Arduino
- Raspberry Pi

---

## Alimentación

El módulo puede alimentarse entre 2.5 V y 6 V.

Habitualmente se alimenta con 5 V cuando se utiliza junto con displays HD44780.

---

## Hojas de Datos

- [[PCF8574_Datasheet.pdf]]

---

## Utilizado en

Los proyectos que utilicen este componente aparecerán automáticamente mediante los Backlinks de Obsidian / Quartz.

---

# Notas de Ingeniería

## Generales

### Direcciones I2C

Siempre verificar la dirección del dispositivo mediante un escaneo del bus.

No todos los módulos comerciales utilizan 0x27.

---

### Pull-Up

Antes de conectar el backpack a un microcontrolador de 3.3 V verificar siempre dónde están conectadas las resistencias pull-up.

Muchos módulos comerciales elevan SDA y SCL directamente a VCC.

---

### Compatibilidad

El PCF8574 simplifica notablemente el cableado, pero introduce una ligera sobrecarga respecto a la interfaz paralela.

Para aplicaciones de interfaz de usuario esta diferencia resulta despreciable.

## [[PowerHub]]

### Dirección I2C

El backpack utilizado en PowerHub fue detectado automáticamente por Tasmota en la dirección:

- **0x27**

No fue necesario modificar los puentes A0, A1 y A2.

---

### Alimentación

El backpack permanece alimentado con **5 V**.

El display HD44780 también permanece alimentado con **5 V**.

---

### Bus I2C a 3.3 V

Los GPIO del ESP8266 **no son tolerantes a 5 V**.

Por este motivo se realizaron las siguientes modificaciones:

- Se removieron las resistencias pull-up originales del backpack.
- Se instalaron nuevas resistencias pull-up sobre la placa [[ESP8266 (NodeMCU V3)]], conectando SDA y SCL a 3.3 V.

De esta manera:

- El display continúa funcionando a 5 V.
- El bus I2C opera a 3.3 V.
- Los niveles lógicos son completamente compatibles con el ESP8266.

Esta modificación elimina la posibilidad de aplicar 5 V sobre los GPIO del microcontrolador.

---

### Detección automática

Con Tasmota 15.5.0 el módulo fue detectado automáticamente mediante:

```
I2CScan
```

sin necesidad de especificar manualmente la dirección del dispositivo.

---

### Driver Tasmota

Configuración utilizada:

```
I2CDriver3 1
DisplayModel 1
```

No fue necesario realizar modificaciones adicionales.

---

# Modificaciones de Hardware

## Remoción de resistencias Pull-Up

Muchos backpacks comerciales incorporan resistencias pull-up conectadas directamente a la alimentación del módulo.

Cuando el módulo es alimentado con 5 V y se conecta a un microcontrolador de 3.3 V, estas resistencias elevan las líneas SDA y SCL hasta 5 V.

### Backpack original

![Backpack original](backpack_pullups_original.png)

### Backpack modificado

![Backpack modificado](backpack_pullups_removed.png)

Las resistencias pull-up originales fueron removidas para evitar que las líneas SDA y SCL fueran elevadas a 5 V.

Las nuevas resistencias pull-up fueron instaladas sobre la placa [[ESP8266 (NodeMCU V3)]], conectando ambas líneas a 3.3 V.

Esta modificación permite:

- Mantener el display alimentado con 5 V.
- Operar el bus I²C completamente a 3.3 V.
- Garantizar compatibilidad eléctrica con el ESP8266.

---

# Buenas prácticas

- Verificar la dirección I2C mediante un escaneo antes de asumir un valor.
- No asumir que todos los backpacks utilizan la dirección 0x27.
- Revisar la tensión de las resistencias pull-up antes de conectar el módulo a un microcontrolador de 3.3 V.
- Mantener el display alimentado con la tensión recomendada por el fabricante.
- Documentar cualquier modificación física realizada sobre el backpack.

---

# Lecciones Aprendidas

- No todos los backpacks son eléctricamente compatibles con micros de 3.3 V.
- Una simple modificación de las resistencias pull-up permite reutilizar el mismo hardware de forma segura.
- El escaneo automático del bus I²C evita errores de configuración.
- Documentar las modificaciones físicas del hardware tiene tanto valor como documentar el firmware.

---

# Referencias

NXP Semiconductors — PCF8574 Remote 8-bit I/O Expander for I²C Bus.

Hitachi HD44780 LCD Controller.