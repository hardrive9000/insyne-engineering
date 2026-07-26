# esptool

Utilidad oficial de Espressif para comunicarse con dispositivos ESP8266 y ESP32 a través del puerto serie.

## Capacidades

- Instalar firmware
- Crear backups completos
- Borrar completamente la memoria flash
- Obtener información del chip
- Leer la dirección MAC
- Combinar archivos binarios

---

# Referencia Rápida

| Tarea                      | Comando       |
| -------------------------- | ------------- |
| Borrar flash               | `erase-flash` |
| Instalar firmware          | `write-flash` |
| Crear backup               | `read-flash`  |
| Leer MAC                   | `read-mac`    |
| Información de la flash    | `flash-id`    |
| Combinar archivos binarios | `merge-bin`   |

---

# Instalación

**Crear un entorno virtual de Python**:

```bash
python -m venv esptool-env
```

**Activar el entorno virtual**:

Linux

```bash
source esptool-env/bin/activate
```

Windows (CMD)

```cmd
esptool-env\Scripts\activate.bat
```

Windows (PowerShell)

```powershell
esptool-env\Scripts\Activate.ps1
```

**Instalar esptool**:

```bash
pip install esptool
```

**Verificar la instalación**:

```bash
esptool version
```

---

# Activar el entorno virtual

**Antes de utilizar** `esptool` **se debe activar el entorno virtual**:

```bash
source esptool-env/bin/activate
```

---

# Borrar completamente la memoria flash

```bash
esptool -p /dev/ttyUSBx erase-flash
```

## Ejemplos rápidos

```bash
esptool -p /dev/ttyUSB0 erase-flash
```

---

# Instalar firmware

```bash
esptool -p /dev/ttyUSBx -b 115200 write-flash -fm dout <offset1> filename1.bin <offset2> filename2.bin <offset3> filename3.bin
```

## Ejemplos rápidos

```bash
esptool -p /dev/ttyUSB0 -b 115200 write-flash -fm dout 0x00000 firmware.bin
```

```bash
esptool -p /dev/ttyUSB0 -b 115200 write-flash -fm dout 0x1000 bootloader.bin 0x8000 partition-table.bin 0x10000 firmware.bin
```

> **Nota**
> El modo `dout` es requerido por numerosos dispositivos ESP8266 compatibles con Tasmota.

---

# Crear un backup completo

```bash
esptool -p /dev/ttyUSBx -b 115200 read-flash 0x00000 ALL filename.bin
```

## Ejemplos rápidos

```bash
esptool -p /dev/ttyUSB0 -b 115200 read-flash 0x00000 ALL backup.bin
```

---

# Obtener información de la memoria Flash

```bash
esptool -p /dev/ttyUSBx flash-id
```

## Ejemplos rápidos

```bash
esptool -p /dev/ttyUSB0 flash-id
```

---

# Leer la dirección MAC

```bash
esptool -p /dev/ttyUSBx read-mac
```

## Ejemplos rápidos

```bash
esptool -p /dev/ttyUSB0 read-mac
```

---

# Combinar varios archivos binarios en un único archivo

```bash
esptool -c <chip_type> merge-bin -o output_filename.bin -fm dio -ff keep -fs keep <offset1> filename1.bin <offset2> filename2.bin <offset3> filename3.bin
```

## Ejemplos rápidos

```bash
esptool -c esp32 merge-bin -o firmware.bin -fm dio -ff keep -fs keep 0x1000 bootloader.bin 0x8000 partition-table.bin 0x10000 app.bin
```

```bash
esptool -c esp32s2 merge-bin -o firmware.bin -fm dio -ff keep -fs keep 0x1000 bootloader.bin 0x8000 partition-table.bin 0x10000 app.bin
```

---

# Notas

- Cerrar cualquier monitor serie antes de utilizar `esptool`.
- Utilizar un cable USB de buena calidad.
- Algunos dispositivos requieren mantener **GPIO0** conectado a **GND** durante el reinicio para ingresar en modo de programación.
- Reemplazar `/dev/ttyUSBx` por el puerto serie correspondiente (`/dev/ttyUSB1`, `/dev/ttyACM0`, `COM3`, etc.).

---

# Referencias relacionadas

- [[ESP8266 (NodeMCU V3)]]
- [[PowerHub]]