# ***Código para realizar el magic 8 ball***

```python 

import random  #Importamos el modulo de random.

suerte = random.randint(1, 9)   #variable que almacena el número random.

Question = str(input("Realiza una pregunta a la suerte: "))
#Apartado de Lógica de elección.

#Versión con listas:
list = ["Yes - definitely.", "It is decidedly so.", "Without a doubt.", "Reply hazy, try again.", "Ask again later.",
"Better not tell you now.", "My sources say no.", "Outlook not so good.", "Very doubtful."]
print(list[suerte - 1])


#versión con match x:
#match suerte:
    #case 1:
     #   print("Yes - definitely.")
    #case 2:
      #  print("It is decidedly so.")
    #case 3:
    #    print("Without a doubt.")
    #case 4:
    #    print("Reply hazy, try again.")
    #case 5:
    #    print("Ask again later.")
    #case 6:
    #    print("Better not tell you now.")
    #case 7:
    #    print("My sources say no.")
    #case 8:
    #    print("Outlook not so good.")
    #case 9:
    #    print("Very doubtful.")

```
