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
- signal
- list

### Registres spéciaux

Ces registres sont réservés et ils ont un nom et un utilisation spéciale.


| **Id** | **Nom**          | **Hex value** | **Description**                                                                       |
| ------ | ---------------- | ------------- | ------------------------------------------------------------------------------------- |
| `$sp`  | Stack pointer    | 0x0000        | Adresse du curseur dans la stack.                                                     | 
| `$ip`  | Instance pointer | 0x0001        | Adresse du début de l'instance actuelle dans la mémoire instancielle.                 |
| `$ra`  | Return address   | 0x0002        | Adresse de retour. (utilisée dans les appels de fonction)                             |
| `$v0`  | Return register  | 0x0003        | Registre contenant la valeur de retour d'une fonction.                                |
| `$rw`  | Root window      | 0x0004        | Registre contenant le signal controllant la fenêtre principale.                       |

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
| **sbind**       | S'abonner à un event d'un signal        | 0x20       | **sig** : signal   | **prop_id** : int   | **target** : label  | 
| **scopy**       | Copier un signal et ses propriétés      | 0x21       | **sig** : signal   | **sig** : signal    |                     |
| **sdel**        | Supprimer un signal                     | 0x22       | **sig** : signal   |                     |                     |
| **sget**        | Obtenir une propriété d'un signal       | 0x23       | **sig** : signal   | **prop_id** : int   |                     |
| **slaunch**     | Lancer le signal                        | 0x24       | **sig** : signal   |                     |                     |
| **smem**        | Stocker dans la mémoire instancielle    | 0x25       | **source** : reg   | **offset** : int    | **address** : reg   |
| **snew**        | Créer un signal                         | 0x26       | **sig** : signal   | **comp_id** : int   |                     |
| **ssb**         | Set une propriété bool d'un signal      | 0x27       | **sig** : signal   | **prop_id** : int   | **value** : bool    |
| **sset**        | Set une propriété d'un signal           | 0x28       | **sig** : signal   | **prop_id** : int   | **value** : reg     |
| **ssf**         | Set une propriété float d'un signal     | 0x29       | **sig** : signal   | **prop_id** : int   | **value** : float   |
| **ssi**         | Set une propriété int d'un signal       | 0x2A       | **sig** : signal   | **prop_id** : int   | **value** : int     |
| **sss**         | Set une propriété string d'un signal    | 0x2B       | **sig** : signal   | **prop_id** : int   | **value** : string  |
| **sst**         | Stocker dans la stack                   | 0x2C       | **source** : reg   | **offset** : int    | **address** : reg   |
| **stop**        | Fin du programme                        | 0x2D       |                    |                     |                     |
| **sub**         | Soustraction                            | 0x2E       | **dest** : reg     | **arg1** : reg      | **arg2** : reg      |
| **subi**        | Soustraction immédiate                  | 0x2F       | **dest** : reg     | **arg1** : reg      | **arg2** : int      |

## Représentation hexadécimale

Attention : Les nombres sont en **big endian** au niveau des octets, lesquels sont en **little endian** au niveau des bits. Par exemple, le nombre 127 en hexadécimal s'écrira `27 01`.

Pour représenter une string, sa représentation classique sera transcrite en hexadécimal mais sera précédée de la longueur de la string en octets,sur deux octets.

Pour encoder "hello world", on va donc écrire `0b 00 68 65 6c 6c 6f 20 77 6f 72 6c 64`. Les deux premiers octets (0b 00) indiquent qu'il y a 11 bytes de string, lesquels sont placés à la suite. Puisque seulement deux octets sont réservés pour la longueur de la chaîne de caractère, il y a un maximum de longueur pour celles-ci à 65536 bytes. (un caractère ascii compte pour 1, un caractère utf-8 compte pour 2). Si le besoin se présente, il y aurait moyen d'élever ce maximum en rajoutant un byte pour la longueur, mais cela impliquerait alors un fichier plus lourd.

Nb: c'est bien le litéral string de l'assembleur qui est limité à 65536 bytes. Pour faire un string plus long, il suffirait à l'assembleur de le diviser en deux et faire une addition sur un registre.

## Signaux

Un signal est une sorte d'objet contenant des paramètres à faire passer au renderer Endiver. L'utilisation typique d'un signal se fait en 3 phases :

1. **Créer le signal** : l'instruction `snew` place un nouveau signal du type demandé dans le registre fourni.

2. **Modifier les propriétés** : les instructions `sset`, `ssi`, `ssb`, `ssf`, `sss` permettent de modifier des propriétés du signal. Ces propriétés sont embarquées dans le signal et seront utilisées lors de son lancement. Si besoin, l'instruction `sget` permet d'obtenir une propriété d'un signal.

3. **Lancer le signal** : l'instruction `slaunch` permet de lancer un signal. Le renderer va alors traiter les propriétés reçues en fonction du type du signal (voir types ci dessous).

Certains signaux permettent de modifier une propriété après le lancement (exemple: changer le titre de la fenêtre principale). D'autres ne le permettent pas (exemple: impossible de changer la couleur d'un rectangle déjà dessiné).

### Types de signaux

#### Fenêtre

- **ID** : 0
- **Propriétés:**

| **ID** | **Nom**             | **Type** |
| ------ | ------------------- | -------- |
| 0      | title               | string   |
| 1      | width               | int      |
| 2      | height              | int      |
| 3      | closed              | label    |
| 4      | render              | label    |

#### Draw rectangle

- **ID** : 1
- **Propriétés:**

| **ID** | **Nom**             | **Type** |
| ------ | ------------------- | -------- |
| 0      | x                   | int      |
| 1      | y                   | int      |
| 2      | z                   | int      |
| 3      | width               | int      |
| 4      | height              | int      |
| 5      | background red      | int      |
| 6      | background green    | int      |
| 7      | background blue     | int      |
| 8      | background alpha    | int      |
| 9      | border red          | int      |
| 10     | border green        | int      |
| 11     | border blue         | int      |
| 12     | border alpha        | int      |
| 13     | border width        | int      |
| 14     | rotation x          | int      |
| 15     | rotation y          | int      |
| 16     | rotation z          | int      |
| 17     | left clicked        | label    |
| 18     | left pressed        | label    |
| 19     | left released       | label    |
| 20     | right clicked       | label    |
| 21     | right pressed       | label    |
| 22     | right released      | label    |