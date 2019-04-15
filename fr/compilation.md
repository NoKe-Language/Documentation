# Compilation

## Format

Le compilateur Nolfaris compile le NoKe haut niveau en une série de requête bas-niveau après l'avoir préablement optimisé. Le format d'export est un bytecode composé de toutes les instructions nécessaires à la bonne exécution du programme.

## Opcode

| **Instruction** | **Opcode** | **Arg1**   | **Arg2**  | **Arg3** | **Length** |
| --------------- | ---------- | ---------- | --------- | -------- | ---------- |
| add             | *0x0000*   | target     | var1      | var2     | 4          |
| bind            | *0x0001*   | component  |           |          | 2          |
| div             | *0x0002*   | target     | var1      | var2     | 4          |
| exit            | *0x0003*   |            |           |          | 1          |
| get             | *0x0004*   | property   | target    |          | 3          |
| goto            | *0x0005*   | name       |           |          | 2          |
| if              | *0x0006*   | var1       | var2      | goto     | 4          |
| img             | *0x0007*   | img        | target    |          | 3          |
| mult            | *0x0008*   | target     | var1      | var2     | 4          |
| new             | *0x0009*   | component  | type      |          | 3          |
| nope            | *0x000A*   |            |           |          | 1          |
| pause           | *0x000B*   | in pause ? |           |          | 2          |
| set             | *0x000C*   | property   | new value |          | 3          |
| sound           | *0x000D*   | sound      | target    |          | 3          |
| sub             | *0x000E*   | target     | var1      | var2     | 4          |
| switch          | *0x000F*   | view       |           |          | 2          |

*This list is a non exhaustive concept art.*

## Instructions

### add target, var1, var2

Stocker le résultat de l'addition de *var1* et de *var2* dans *target*.

### bind component

Définir l'élément *component* en tant qu'élément actif. Toutes les modifications qui suivent seront portées à ce *component* à moins qu'un autre n'a été bind entre temps.

## Registers

Comme en assembleur, il y a un nombre limité de variables temporaires enregistrées dans les registres. Les ids étant des bytes de longueur 4, il y a 65536 registres différents. Cette valeur est facilement extensible en augmentant la longueur des bytecodes. De même que 65536 éléments peuvent être créés, 65536 vues, 65536 variables, 65536 images, etc. Créé un élément d'un type ne modifie pas le nombre d'élément d'un autre type que vous pouvez créé.

Des registres à portée globale sont pourtant réservés sur 65536.

### Registres réservés

| Id       | Description         |
| -------- | ------------------- |
| *0x0000* | Plein écran         |
| *0x0001* | Curseur visible     |
| *0X0002* | Titre de la fenêtre |

