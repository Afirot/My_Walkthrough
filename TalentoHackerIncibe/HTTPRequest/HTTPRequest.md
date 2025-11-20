# HTTP request

Lo unico que se nos entrega para completar este desafio es un fichero llamado access.log.

![log](img/image(5).png)

En este fichero log solo encontramos una diferencia, y es que de vez en cuando, los intentos de post, tienen exito, y devuelben un codigo 200.

Despues de revisarlo durante un tiempo, la unica posibilidad que se me ocurre es que los numeros de errores 400 antes de cada 200 tengan un mensaje oculto, asi que programo un script rapido en python que sea capaz de contar cuantos errores 400 hay antes de cada codigo 200.

```python
numeros = []
contador = 0

with open("access.log", "r") as file:
    for linea in file:
        linea = linea.strip()
        if "400" in linea:
            contador += 1
        elif "200" in linea:
            numeros.append(contador)
            contador = 0

print(numeros)
```

La ejecucion de este script devuelbe lo siguiente.
```python
[7, 5, 7, 3, 6, 5, 7, 2, 7, 3, 2, 12, 6, 6, 6, 12, 6, 1, 6, 7, 6, 9, 6, 4, 2, 12, 6, 6, 6, 12, 6, 1, 6, 7, 3, 1, 6, 6, 6, 12, 6, 1, 6, 7, 7, 11, 6, 12, 3, 0, 6, 7, 5, 15, 3, 4, 6, 14, 3, 4, 6, 12, 7, 9, 7, 10, 3, 3, 7, 2, 5, 15, 6, 7, 3, 3, 3, 3, 6, 11, 7, 13]
```

El unico patron que veo en estos numeros, es el hecho de que no hay ni un solo numero mayor a 15, lo cual, corresponde con un codigo hexadecimal, asi que, voy a modificar el script para que convierta los caracteres superiores a 10 en letras y devuelba una sola linea de texto.

```python
numeros = []
contador = 0

with open("access.log", "r") as file:
    for linea in file:
        linea = linea.strip()
        if "400" in linea:
            contador += 1
        elif "200" in linea:
            numeros.append(contador)
            contador = 0

hex_values = [hex(n)[2:].upper() for n in numeros]

hexall = ""

for i in hex_values:
    hexall = hexall + i

print(hexall)
```

La ejecucion de este script devuelbe la siguiente cadena.

```python
75736572732C666C616769642C666C616731666C61677B6C30675F346E346C797A33725F6733336B7D
```

Y, al decodificarlo, obtengo la flag del desafio.

![log](img/image(2).png)