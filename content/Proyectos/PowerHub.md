# PowerHub
### Rev. 1.0

### Hardware
---------
- [[ESP8266 (NodeMCU V3)]]
- [[Display LCD 1602 Hitachi HD44780]]
- [[Módulo Adaptador I2C Display LCD PCF8574]]
- [[Módulo Relé de 4 Canales]]
- Receptor IR [[VS1838B]]
- Pulsador N.A.

### Firmware
---------
Tasmota 15.5.0 (tasmota-display)

### Autor
-----
hardrive9000 (aka Alto Evolucionario)

# Dirección IP Dinámica (Por defecto)
### Dirección IP de dispositivo
```
IPAddress1 0.0.0.0
```

# Dirección IP Estática
### Dirección IP de dispositivo
```
IPAddress1 192.168.100.250
```
### Dirección IP de gateway
```
IPAddress2 192.168.100.1
```
### Máscara de subred
```
IPAddress3 255.255.255.0
```
### Dirección IP de DNS
```
IPAddress4 192.168.100.1
```

# Deshabilitar Serial Log
```
SerialLog 0
```
### Reiniciar
```
Restart 1
```

# Pinout
```
Display I2C HD44780 PCF8574
D1 (GPIO5) I2C SCL ID:1
D2 (GPIO4) I2C SDA ID:1

Módulo de 4 Relays
D5 (GPIO14) [Relay 1] Relay_i ID:1
D6 (GPIO12) [Relay 2] Relay_i ID:2
D7 (GPIO13) [Relay 3] Relay_i ID:3
D0 (GPIO16) [Relay 4] Relay_i ID:4

Pulsador N.A
TX (GPIO1) Button ID:1

Receptor IR
D4 (GPIO2) IRrecv
```

# Comandos Display
```
DisplayWidth 16 (Or 20 depending on your LCD size)
DisplayHeight 2 (Or 4 depending on your LCD size)

DisplayText [l1c1]Hola mundo (Prints "Hola mundo" starting at Row 1, Column 1)
DisplayText [l2c1]Tasmota (Prints "Tasmota" starting on Row 2, Column 1)
DisplayText [z] (Clears the screen)
```

### En caso de fallar la detección automática
```
I2CDriver3 1 (Enable PCF8574/LCD driver)
DisplayModel 1 (Set to standard HD44780 model)
I2CScan (Scan I2C bus for devices)
DisplayAddress 0x27 (Or 0x3F if your backpack uses a different I2C address)
```

# Rules
### Rule 1
```
Rule1 ON Wifi#Connected DO Backlog DisplayText [z][l1c1]R1 OFF  R2 OFF; DisplayText [l2c1]R3 OFF  R4 OFF; Event SyncDisplay ENDON ON Rules#Timer=2 DO Backlog DisplayText [z][l1c1]R1 OFF  R2 OFF; DisplayText [l2c1]R3 OFF  R4 OFF; Event SyncDisplay ENDON ON Power1#State=0 DO Backlog Var1 OFF; DisplayText [l1c4]OFF ENDON ON Power1#State=1 DO Backlog Var1 ON; DisplayText [l1c4]ON [l1c1] ENDON ON Power2#State=0 DO Backlog Var2 OFF; DisplayText [l1c12]OFF ENDON ON Power2#State=1 DO Backlog Var2 ON; DisplayText [l1c12]ON [l1c1] ENDON ON Power3#State=0 DO Backlog Var3 OFF; DisplayText [l2c4]OFF ENDON ON Power3#State=1 DO Backlog Var3 ON; DisplayText [l2c4]ON [l1c1] ENDON ON Power4#State=0 DO Backlog Var4 OFF; DisplayText [l2c12]OFF ENDON ON Power4#State=1 DO Backlog Var4 ON; DisplayText [l2c12]ON [l1c1] ENDON
```
### Activar Rule 1
```
Rule1 1
```
### Luego ejecutar
```
Backlog SetOption73 1; SetOption1 1; SetOption32 20
```
### Rule 2
```
Rule2 ON Button1#Action=SINGLE DO Backlog Power1 ON; Power2 ON; Power3 ON; Power4 ON ENDON ON Button1#Action=DOUBLE DO Backlog DisplayText [z][l1c1]MAC:; DisplayText [l1c6]2C:3A:E8; DisplayText [l2c6]0C:A7:A0; RuleTimer2 5 ENDON ON Button1#Action=HOLD DO Backlog Power1 OFF; Power2 OFF; Power3 OFF; Power4 OFF ENDON ON IrReceived#Data=0x00FF3AC5 DO Backlog Power1 ON; Power2 ON; Power3 ON; Power4 ON ENDON ON IrReceived#Data=0x00FF02FD DO Backlog Power1 OFF; Power2 OFF; Power3 OFF; Power4 OFF ENDON ON IrReceived#Data=0x00FF1AE5 DO Power1 TOGGLE ENDON ON IrReceived#Data=0x00FF9A65 DO Power2 TOGGLE ENDON ON IrReceived#Data=0x00FFA25D DO Power3 TOGGLE ENDON ON IrReceived#Data=0x00FF22DD DO Power4 TOGGLE ENDON ON IrReceived#Data=0x00FFC837 DO Power5 ON ENDON ON IrReceived#Data=0x00FF48B7 DO Power5 OFF ENDON
```
### Activar Rule 2
```
Rule2 1
```
### Rule 3
```
Rule3 ON Power1#Boot=0 DO Var1 OFF ENDON ON Power1#Boot=1 DO Var1 ON ENDON ON Power2#Boot=0 DO Var2 OFF ENDON ON Power2#Boot=1 DO Var2 ON ENDON ON Power3#Boot=0 DO Var3 OFF ENDON ON Power3#Boot=1 DO Var3 ON ENDON ON Power4#Boot=0 DO Var4 OFF ENDON ON Power4#Boot=1 DO Var4 ON ENDON ON Event#SyncDisplay DO Backlog Var1 %var1%; Var2 %var2%; Var3 %var3%; Var4 %var4% ENDON ON Var1#State=ON DO DisplayText [l1c4]ON [l1c1] ENDON ON Var1#State=OFF DO DisplayText [l1c4]OFF[l1c1] ENDON ON Var2#State=ON DO DisplayText [l1c12]ON [l1c1] ENDON ON Var2#State=OFF DO DisplayText [l1c12]OFF[l1c1] ENDON ON Var3#State=ON DO DisplayText [l2c4]ON [l1c1] ENDON ON Var3#State=OFF DO DisplayText [l2c4]OFF[l1c1] ENDON ON Var4#State=ON DO DisplayText [l2c12]ON [l1c1] ENDON ON Var4#State=OFF DO DisplayText [l2c12]OFF[l1c1] ENDON
```
### Activar Rule 3
```
Rule3 1
```
### Reiniciar
```
Restart 1
```

# Filosofía del proyecto

- Utilizar exclusivamente firmware oficial de Tasmota.
- No recompilar el firmware.
- Mantener el diseño modular.
- Priorizar simplicidad y mantenibilidad.
- Las funciones básicas residen en el dispositivo.
- Las funciones avanzadas residen en el control remoto IR.
- δ∫ₜ₁ᵗ² L dt = 0
- MINIMUM ACTION ENGINEERED