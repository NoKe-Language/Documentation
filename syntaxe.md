# Syntaxe basique

La syntaxe du NoKe est similaire à celle de Python:

En effet, un bloc sera indenté (4 espaces). Comme en Python, le `:` est requis pour marquer le début d'un bloc. De même, aucun `;` n'est requis à la fin d'une ligne, mais il n'est possible d'écrire qu'une instruction par ligne.


<pre noke><div noke><p fun>fun</p> my_function():
    <p kw>if</p> <p tag>true</p>:
        log(<p tag>"Hello world !"</p>)
        <p comment>// some code</p></div></pre>

## Variables

Une variable est toujours **typée** et doit être déclarée avant assignement.

Pour déclarer une variable, la syntaxe est la suivante : 

<pre noke><div noke>ma_variable : <p kw>int</p></div></pre>

Une fois qu'une variable est déclarée, son type est immuable.

Pour assigner une valeur à la variable :

<pre noke><div noke>ma_variable = <p tag>42</p></div></pre>

Il est également possible de déclarer et assigner une variable en une seule ligne:

<pre noke><div noke>ma_variable : <p kw>int</p> = <p tag>42</p></div></pre>

Pour éviter de déclarer explicitement le type comme dans le cas ci dessus, l'opérateur `:=` permet d'automatiquement vérifier le type du membre de droite et de l'utiliser pour la déclaration.


<pre noke><div noke>ma_variable := <p tag>42</p>

<p comment>// Parfaitement égal à</p>
ma_variable : <p kw>int</p> = <p tag>42</p>

<p comment>// Parfaitement égal à</p>
ma_variable : <p kw>int</p>
ma_variable = <p tag>42</p></div></pre>

Ces vérifications sont effectuées lors de la compilation, ainsi le temps d'exécution est identique entre les différentes formulations.

## Valeurs et litéraux

Un **litéral** est la représentation d'une constante, par exemple un nombre ou une chaîne de caractère. Une **valeur** est un objet d'un certain type qui est retourné par un litéral, une opération ou une instruction.

Exemple de litéral : `42`

Exemple de valeur : l'objet de type `int` ayant pour valeur 42

### Nombres

Il y a deux types de nombres : les entiers (`int`) et les nombres décimaux (`float`).

<pre noke><div noke><p comment>// int literal</p>
foo : <p kw>int</p> = <p tag>42</p>

<p comment>// float literal</p>
bar : <p kw>float</p> = <p tag>42.3</p></div></pre>

### Chaînes de caractères

Une chaîne de caractère peut être écrite de différente manière, soit avec des `"` ou des `'`.

<pre noke><div noke><p comment>// string literals</p>
foo : <p kw>string</p> = <p tag>"hello world"</p>

bar : <p kw>string</p> = <p tag>'hello world'</p></div></pre>

### Structures de données

Un array peut être écrit sous la forme

<pre noke><div noke><p comment>// array of int literal</p>
my_array : <p kw>Array</p>&lt;<p kw>int</p>&gt; = [<p tag>4</p>, <p tag>2</p>, <p tag>42</p>]

<p comment>// Avec l'opérateur raccourci</p>
my_array := [<p tag>4</p>, <p tag>2</p>, <p tag>42</p>]

<p comment>// Impossible de détecter le type si l'array est vide</p>
my_array := [] <p comment>// -> erreur</p>
</div></pre>

### Autres

Les constantes booléennes s'écrivent `false` et `true`.


<pre noke><div noke><p comment>// bool literal</p>
is_dev_hungry : <p kw>bool</p> = <p tag>true</p></div></pre>

## Fonctions

Pour définir une fonction, la syntaxe est la suivante:

<pre noke><div noke><p comment>// fonction sans valeur de retour ni paramètre</p>
<p fun>fun</p> ma_fonction():
    <p comment>// some code</p>
    
<p comment>// fonction avec paramètre</p>
<p fun>fun</p> ma_fonction(param : <p kw>int</p>):
    <p comment>// some code</p>

<p comment>// fonction avec paramètre et valeur par défaut</p>
<p fun>fun</p> ma_fonction(param : <p kw>int</p> = <p tag>42</p>):
    <p comment>// some code</p>

<p comment>// il est également possible d'utiliser l'opérateur raccourci</p>
<p fun>fun</p> ma_fonction(param := <p tag>42</p>):
    <p comment>// some code</p>

<p comment>// fonction avec valeur de retour</p>
<p fun>fun</p> ma_fonction() -> <p kw>int</p>:
    <p kw>return</p> <p tag>42</p></div></pre>

Une fonction doit être placée à l'intérieur d'une classe ou d'une autre fonction. Pour la structure d'un projet, voir la page "structure".

## Classes


