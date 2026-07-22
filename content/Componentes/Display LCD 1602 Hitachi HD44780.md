# Display LCD 1602 Hitachi HD44780

El Display LCD 1602 es un módulo alfanumérico capaz de mostrar 2 líneas de 16 caracteres cada una. Está basado en el controlador Hitachi HD44780 (o compatibles), ampliamente utilizado en sistemas embebidos debido a su simplicidad, bajo consumo y amplia disponibilidad.

El display puede operar mediante interfaz paralela de 4 u 8 bits, o mediante un expansor de E/S como el [[Módulo Adaptador I2C Display LCD PCF8574]], reduciendo considerablemente la cantidad de GPIO necesarios.

---

## Características principales

- Controlador: Hitachi HD44780 (o compatible)
- Resolución: 16 × 2 caracteres
- Alimentación:
  - Lógica: 5 V
  - Backlight: 5 V
- Interfaz paralela de 4 u 8 bits
- Compatible con módulos I2C mediante [[Módulo Adaptador I2C Display LCD PCF8574]]
- Memoria CGRAM para caracteres personalizados

---

## Especificaciones

| Parámetro | Valor |
|-----------|-------|
| Filas | 2 |
| Columnas | 16 |
| Caracteres | ASCII + CGRAM |
| Alimentación | 5 V |
| Corriente lógica | Muy baja |
| Retroiluminación | LED |

---

## Pinout

| Pin | Nombre | Descripción |
|-----|---------|-------------|
| 1 | VSS | GND |
| 2 | VDD | +5 V |
| 3 | VO | Ajuste de contraste |
| 4 | RS | Register Select |
| 5 | RW | Read / Write |
| 6 | E | Enable |
| 7-14 | D0-D7 | Bus de datos |
| 15 | LED+ | Backlight + |
| 16 | LED- | Backlight - |

---

## Modos de operación

### Bus paralelo de 8 bits

Máximo rendimiento.

Requiere once GPIO aproximadamente.

---

### Bus paralelo de 4 bits

Reduce el número de GPIO utilizando únicamente D4–D7.

Es el modo más utilizado cuando el display se conecta directamente al microcontrolador.

---

### Interfaz I2C

Mediante un [[Módulo Adaptador I2C Display LCD PCF8574]] el display requiere únicamente:

- SDA
- SCL

Esta configuración simplifica notablemente el cableado.

---

## Alimentación

El módulo puede permanecer alimentado permanentemente sin inconvenientes.

La retroiluminación constituye el principal consumo del conjunto.

---

## Hojas de Datos

- [[HD44780_Datasheet.pdf]]

---

## Utilizado en

Los proyectos que utilicen este componente aparecerán automáticamente mediante los Backlinks de Obsidian / Quartz.

---

# Notas de Ingeniería

## Generales

### Legibilidad

El contraste determina en gran medida la calidad visual del display.

Conviene ajustarlo antes del montaje definitivo.

---

### Interfaz

Cuando la cantidad de GPIO disponibles es limitada, el uso de un backpack I2C representa la solución más práctica.

---

### Robustez

El HD44780 es uno de los controladores LCD más difundidos de la industria y posee un comportamiento extremadamente estable.

## [[PowerHub]]

### Alimentación

En PowerHub el display permanece alimentado permanentemente con 5 V.

No se observaron problemas de estabilidad durante el funcionamiento continuo.

---

### Interfaz

El display se controla mediante un [[Módulo Adaptador I2C Display LCD PCF8574]].

Configuración utilizada:

- SDA → GPIO4
- SCL → GPIO5

---

### Contraste

El potenciómetro del backpack fue ajustado para obtener el mejor compromiso entre contraste y legibilidad.

No fue necesario realizar modificaciones adicionales.

---

### Retroiluminación

La retroiluminación permanece alimentada de forma independiente mediante el botón auxiliar (Power5) creado por Tasmota.

Esto permite:

- Encender o apagar el backlight sin afectar el funcionamiento del display.
- Reducir el consumo cuando no se requiere iluminación.
- Mantener visible la información mediante iluminación ambiental.

La conmutación se realiza mediante el transistor incorporado en el [[Módulo Adaptador I2C Display LCD PCF8574]].

---

### Tasmota

Driver utilizado:

```
DisplayModel 1
```

Funcionamiento estable con:

- Tasmota Display Driver

---

### Tiempo de operación

No se observaron:

- caracteres corruptos;
- pérdidas de comunicación;
- bloqueos del controlador.

El funcionamiento resultó completamente estable.

---

# Buenas prácticas

- Alimentar el display con 5 V.
- Ajustar el contraste antes del montaje definitivo.
- Evitar cables I2C excesivamente largos.
- Utilizar un backpack I2C cuando la cantidad de GPIO sea limitada.
- Documentar la dirección I2C del backpack asociado.

---

# Lecciones Aprendidas

- Separar la lógica del display de la retroiluminación ofrece una mayor flexibilidad de uso.
- La interfaz I2C simplifica enormemente el cableado.
- Un correcto ajuste del contraste mejora más la legibilidad que aumentar el brillo del backlight.
- El HD44780 continúa siendo una excelente opción para interfaces simples y robustas.

---

# Referencias

Hitachi — HD44780 LCD Controller Datasheet.