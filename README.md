---

## Cómo Ejecutar el Script

### Sintaxis
```bash
python nombre_del_script.py <dirección_IP>
```
### Ejemplo de Uso
```
python validador_ip.py 192.168.1.1
```
Si no se incluye una dirección IP como argumento, el script mostrará un mensaje de error indicando que falta un argumento.

## Funcionamiento del Script
### Flujo de Validación
#### **1. Sin Argumentos:** Si no se proporciona una dirección IP al ejecutar el script, este retornará el mensaje:

```
Debes agregar una dirección IP como argumento al ejecutar el script.
```
#### **2. Formato de Octetos:** El script verifica que la dirección IP tenga exactamente cuatro octetos separados por puntos. Si no cumple esta condición, retornará:

```
La dirección IP no tiene cuatro octetos. Ejemplo válido: '192.168.1.1'
```
#### **3. Contenido de los Octetos:** Se comprueba que cada octeto sea un número. Si se detectan caracteres no numéricos, el mensaje será:

```
La dirección IP contiene caracteres que no son números.
```
#### **4. Rango de los Octetos:** Cada octeto debe estar dentro del rango válido (0-255). Si algún octeto está fuera de este rango, aparecerá:

```
La dirección IP tiene octetos fuera del rango permitido (0-255).
```
#### **5. Validación Exitosa:** Si la IP pasa todas las validaciones, el script continúa con la clasificación de la dirección IP.

### Determinación de la Clase IP
**· Clase A:** Primer octeto entre 0 y 127.
**· Clase B:** Primer octeto entre 128 y 191.
**· Clase C:** Primer octeto entre 192 y 223.
**· Clase D:** Primer octeto entre 224 y 239.
**· Clase E:** Primer octeto entre 240 y 255.
## Mensajes de Salida
Errores
|Código de Retorno	|Mensaje
|-------------------|------------------------------------------------|
|0	                |Debes agregar una dirección IP como argumento.
|1	                |La dirección IP no tiene cuatro octetos.
|2	                |La dirección IP contiene caracteres no numéricos.
|4	                |La dirección IP tiene octetos fuera de rango.
## Éxito
Cuando la dirección IP es válida, se muestra la siguiente información:

```
·IP: 192.168.1.1 ✔
·Estructura correcta.✔
·Todos los octetos están compuestos por números.✔
·Cada octeto tiene un valor entre 0 y 255.✔
·Clase C.✔
```
## Casos de Uso
### Validación Exitosa
```
python validador_ip.py 10.0.0.1
```
#### Salida:

```
·IP: 10.0.0.1 ✔
·Estructura correcta.✔
·Todos los octetos están compuestos por números.✔
·Cada octeto tiene un valor entre 0 y 255.✔
·Clase A.✔
```
### Error en el Formato
```
python validador_ip.py 192.168.1
```
#### Salida:

```
La dirección IP no tiene cuatro octetos. Ejemplo válido: '192.168.1.1'
```
### Octetos Fuera de Rango
```
python validador_ip.py 192.300.1.1
```
#### Salida:

```
La dirección IP tiene octetos fuera del rango permitido (0-255).
```
## Código Fuente Completo
```
import sys

# Función para validar una dirección IP
def pasar_ip():
    """
    Verifica si la dirección IP proporcionada es válida.
    Retorna:
        0: No se proporcionó ninguna IP.
        1: La IP no tiene cuatro octetos.
        2: La IP contiene caracteres no numéricos.
        4: Algún octeto está fuera del rango permitido (0-255).
        3: La IP es válida.
    """
    if len(sys.argv) == 2:
        entrada = sys.argv[1]
        if len(entrada.split(".")) != 4:
            return 1
        else:
            octetos = entrada.split(".")
            for octeto in octetos:
                if not octeto.isdigit():
                    return 2
                elif int(octeto) > 255 or int(octeto) < 0:
                    return 4
            return 3
    else:
        return 0

# Función para determinar la clase de una IP
def clase_ip(ip):
    """
    Determina la clase de una IP según su primer octeto.
    Retorna:
        0: Clase A
        1: Clase B
        2: Clase C
        3: Clase D
        4: Clase E
    """
    octetos = ip.split(".")
    primer_octeto = int(octetos[0])
    if primer_octeto >= 0 and primer_octeto <= 127:
        return 0
    elif primer_octeto > 127 and primer_octeto <= 191:
        return 1
    elif primer_octeto > 191 and primer_octeto <= 223:
        return 2
    elif primer_octeto > 223 and primer_octeto <= 239:
        return 3
    elif primer_octeto > 239 and primer_octeto <= 255:
        return 4

# Función principal
def main():
    resultado = pasar_ip()
    if resultado == 0:
        print("Debes agregar una dirección IP como argumento al ejecutar el script.")
    elif resultado == 1:
        print("La dirección IP no tiene cuatro octetos. Ejemplo válido: '192.168.1.1'")
    elif resultado == 2:
        print("La dirección IP contiene caracteres que no son números.")
    elif resultado == 4:
        print("La dirección IP tiene octetos fuera del rango permitido (0-255).")
    elif resultado == 3:
        ip = sys.argv[1]
        clase = ""

        # Determinamos la clase de la IP
        if clase_ip(ip) == 0:
            clase = "Clase A.✔"
        elif clase_ip(ip) == 1:
            clase = "Clase B.✔"
        elif clase_ip(ip) == 2:
            clase = "Clase C.✔"
        elif clase_ip(ip) == 3:
            clase = "Clase D.✔"
        elif clase_ip(ip) == 4:
            clase = "Clase E.✔"

        # Mostramos los resultados al usuario
        print(
            f"\n·IP: {ip} ✔"
            f"\n·Estructura correcta.✔"
            f"\n·Todos los octetos están compuestos por números.✔"
            f"\n·Cada octeto tiene un valor entre 0 y 255.✔"
            f"\n·{clase}"
        )

# Llamamos a la función principal si el script es ejecutado directamente
if __name__ == "__main__":
    main()
```
