# Assembleur NoKe

L'**assembleur NoKe** est un langage assembleur spécial conçu pour être interprété par le renderer **Endiver**. Librement inspiré du MIPS, il est toutefois bien plus haut niveau pour les raisons suivantes:

- Les registres ne sont pas limités à 32bits et peuvent contenir n'importe quel **type primaire**.
- Il y a maximum 65536 registres au lieu de 32.
- La mémoire est divisée en deux parties distinctes, la **stack** et la **mémoire instancielle**. Cette division permet de faciliter la gestion de la programmation orientée objet.

## Registres

Il y a 65536 registres au maximum. La raison est que dans le bytecode, un registre est écrit, par exemple, comme ceci : `0x002A`. La limite est donc 16^n où n est le nombre de chiffres hexadécimaux.

Contrairement au MIPS, les registres ne sont que des pointeurs vers un array haut niveau en Crystal, et ils peuvent contenir n'importe quelle valeur d'un **type primaire**.

La liste des **types primaires** est la suivante :
- int
- float
- pointer
- string
- bool

### Registres spéciaux

Ces registres sont réservés et ils ont un nom et un utilisation spéciale.


| **Id** | **Nom**          | **Hex value** | **Description**                                                       |
| ------ | ---------------- | ------------- | --------------------------------------------------------------------- |
| `$sp`  | Stack pointer    | 0x0000        | Adresse du curseur dans la stack.                                     | 
| `$ip`  | Instance pointer | 0x0001        | Adresse du début de l'instance actuelle dans la mémoire instancielle. |
| `$ra`  | Return address   | 0x0002        | Adresse de retour. (utilisée dans les appels de fonction)             |
| `$v0`  | Return register  | 0x0003        | Registre contenant la valeur de retour d'une fonction.                |

### Autres registres

Les autres registres sont divisés en deux catégories:

- Registres temporaires. Ils ont pour id `$si` où i est un nombre naturel.
- Registres paramétriques. Ils ont pour id `$ai` où i est un nombre naturel.

Cette distinction est purement conventionnelle et permet la simplification de fonctions (toujours les mêmes registres pour les paramètres).

Il peut y avoir autant de ces registres que nécessaire, à condition qu'il reste des registres disponible.

## Instructions

Voici le résumé des instructions supportées par l'assembleur NoKe :
  
| **Instruction** | **Description**                         | **Opcode** | **Arg1**           | **Arg2**            | **Arg3**            | 
| --------------- | --------------------------------------- | ---------- | ------------------ | ------------------  | ------------------- |
| **add**         | Addition                                | 0x00       | **dest** : reg     | **arg1** : reg      | **arg2** : reg      |
| **addi**        | Addition immédiate                      | 0x01       | **dest** : reg     | **arg1** : reg      | **arg2** : int      |
| **beq**         | Branch if equal                         | 0x02       | **arg1** : reg     | **arg2** : reg      | **target** : label  |
| **beqz**        | Branch if equal to zero                 | 0x03       | **arg** : reg      | **target** : label  |                     |
| **bge**         | Branch if greater or equal              | 0x04       | **arg1** : reg     | **arg2** : reg      | **target** : label  |
| **bgez**        | Branch if greater than or equal to zero | 0x05       | **arg** : reg      | **target** : label  |                     |
| **bgt**         | Branch if greater than                  | 0x06       | **arg1** : reg     | **arg2** : reg      | **target** : label  |
| **bgtz**        | Branch if greater than zero             | 0x07       | **arg** : reg      | **target** : label  |                     |
| **ble**         | Branch if less or equal                 | 0x08       | **arg1** : reg     | **arg2** : reg      | **target** : label  |
| **blez**        | Branch if less than or equal to zero    | 0x09       | **arg** : reg      | **target** : label  |                     |
| **blt**         | Branch if less than                     | 0x0A       | **arg1** : reg     | **arg2** : reg      | **target** : label  |
| **bltz**        | Branch if less than zero                | 0x0B       | **arg** : reg      | **target** : label  |                     |
| **bne**         | Branch if not equal                     | 0x0C       | **arg1** : reg     | **arg2** : reg      | **target** : label  |
| **bnez**        | Branch if not equal to zero             | 0x0D       | **arg** : reg      | **target** : label  |                     |
| **del**         | Supprimer la valeur + offset            | 0x0E       | **first** : reg    | **offset** : int    |                     |
| **div**         | Division                                | 0x0F       | **dest** : reg     | **arg1** : reg      | **arg2** : reg      |
| **divi**        | Division immédiate                      | 0x10       | **dest** : reg     | **arg1** : reg      | **arg2** : int      |
| **inst**        | Obtenir adresse avec assez d'espace     | 0x11       | **nb** : int       |                     |                     |
| **j**           | Saut                                    | 0x12       | **target** : label |                     |                     |
| **jr**          | Saut avec registre                      | 0x13       | **target** : reg   |                     |                     |
| **la**          | Copier un registre                      | 0x14       | **dest** : reg     | **source** : reg    |                     |
| **lb**          | Charger un bool dans un registre        | 0x15       | **dest** : reg     | **source** : bool   |                     |
| **lf**          | Charger un float dans un registre       | 0x16       | **dest** : reg     | **source** : float  |                     |
| **li**          | Charger un int dans un registre         | 0x17       | **dest** : reg     | **source** : int    |                     |
| **lmem**        | Charger depuis la mémoire instancielle  | 0x18       | **dest** : reg     | **offset** : int    | **address** : reg   |
| **log**         | Afficher un string dans la console      | 0x19       | **content** : reg  |                     |                     |
| **lst**         | Charger depuis la stack                 | 0x1A       | **dest** : reg     | **offset** : int    | **address** : reg   |
| **lstr**        | Charger un string dans un registre      | 0x1B       | **dest** : reg     | **source** : string |                     |
| **mod**         | Modulo                                  | 0x1C       | **dest** : reg     | **arg1** : reg      | **arg2** : reg      |
| **modi**        | Modulo immédiat                         | 0x1D       | **dest** : reg     | **arg1** : reg      | **arg2** : int      |
| **mul**         | Multiplication                          | 0x1E       | **dest** : reg     | **arg1** : reg      | **arg2** : reg      |
| **muli**        | Multiplication immédiate                | 0x1F       | **dest** : reg     | **arg1** : reg      | **arg2** : int      |
| **smem**        | Stocker dans la mémoire instancielle    | 0x20       | **source** : reg   | **offset** : int    | **address** : reg   |
| **sst**         | Stocker dans la stack                   | 0x21       | **source** : reg   | **offset** : int    | **address** : reg   |
| **stop**        | Fin du programme                        | 0x22       |                    |                     |                     |
| **sub**         | Soustraction                            | 0x23       | **dest** : reg     | **arg1** : reg      | **arg2** : reg      |
| **subi**        | Soustraction immédiate                  | 0x24       | **dest** : reg     | **arg1** : reg      | **arg2** : int      |

### Détails sur la représentation hexadécimale

