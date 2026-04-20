 # ***Código para prácticar árbol de decisiones***
```python
#Creación de variables y almacenamiento mediante input

height = int(input("Introduzca su altura en cm: "))
credits = int(input("Introduzca sus créditos: "))

#Almacenamos las respuestas acorde a los futuros parámetros que usemos.

respuestas = ["Enjoy the ride!", "You are not tall enough to ride.", "You don't have enough credits.", "You don't have met the requirements"]

#print(len(respuestas)) dejamos la línea anterior solo para comprobar el largo de nuestra lista.

#Apartado de árbol de decisiones.


if height < 130:
  print(respuestas[1])
elif height < 130 and credits < 2:
  print(respuestas[1])
elif height > 130 and credits >= 2:
  print(respuestas[0])
elif height > 130 and credits < 2:
  print(respuestas[2])
elif height < 130 and credits > 2:
  print(respuestas[1])
else:
  print(respuestas[3])
```
## Mejoras posteriores.
```python
# Entrada de datos
try:
    height  = int(input("Introduzca su altura en cm: "))
    credits = int(input("Introduzca sus créditos: "))
except ValueError:
    print("Error: introduzca solo números enteros.")
    exit()

# Mensajes de respuesta
respuestas = {
    "ok":          "Enjoy the ride!",
    "low_height":  "You are not tall enough to ride.",
    "low_credits": "You don't have enough credits.",
}

# Árbol de decisiones
if height < 130:
    print(respuestas["low_height"])
elif credits < 2:
    print(respuestas["low_credits"])
else:
    print(respuestas["ok"])
```
  
