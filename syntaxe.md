# Syntaxe basique

La syntaxe du NoKe est similaire à celle de Python:

En effet, un bloc sera indenté (4 espaces). Comme en Python, le `:` est requis pour marquer le début d'un bloc. De même, aucun `;` n'est requis à la fin d'une ligne, mais il n'est possible d'écrire qu'une instruction par ligne.


<pre noke><div noke><p fun>fun</p> ma_fonction():
    <p kw>if</p> <p tag>true</p>:
        log(<p tag>"Hello world !"</p>)
        <p comment>// some code</p></div></pre>
    
Si un bloc ne contient pas d'instruction, il faut l'indiquer avec le mot clé `skip`.

<pre noke><div noke><p fun>fun</p> ma_fonction():
    <p comment>// Un commentaire ne compte pas comme une instruction</p>
    <p comment>// le mot clé skip est donc requis</p>
    <p fun>skip</p></div></pre>

Cependant, contrairement au python ou d'autres langages indentés, un bloc vide peut simplement être écrit en **n'écrivant pas le symbole** `:`. C'est la méthode recommendée.

<pre noke><div noke><p fun>fun</p> my_function()</div></pre>

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
    <p fun>skip</p></div></pre>

Il est possible de passer des paramètres à une fonction. Par défaut, les variables d'un type basique (`int`, `string`, ...) sont copiées et les objets sont passés par références.

<pre noke><div noke><p comment>// fonction avec paramètre</p>
<p fun>fun</p> ma_fonction(param : <p kw>int</p>):
    <p comment>// some code</p>
    <p fun>skip</p></div></pre>

Il est possible de spécifier un paramètre comme étant facultatif:

<pre noke><div noke><p fun>fun</p> ma_fonction(<p fun>optional</p> param : <p kw>int</p>):
    <p comment>// some code</p>
    <p fun>skip</p></div></pre>

Il est possible de spéficier des valeurs par défaut pour les paramètres. Cela rend automatiquement le paramètre facultatif. Si un paramètre facultatif est omis, sa valeur sera `null`.

<pre noke><div noke><p comment>// fonction avec paramètre et valeur par défaut</p>
<p fun>fun</p> ma_fonction(param : <p kw>int</p> = <p tag>42</p>):
    <p comment>// some code</p>
    <p fun>skip</p>

<p comment>// il est également possible d'utiliser l'opérateur raccourci</p>
<p fun>fun</p> ma_fonction(param := <p tag>42</p>):
    <p comment>// some code</p>
    <p fun>skip</p>
    
<p comment>// les paramètres par défaut et facultatifs sont placés à la fin</p>
<p fun>fun</p> ma_fonction(param := <p tag>42</p>, name : <p kw>string</p>): <p comment>// ---> erreur</p>
    <p comment>// some code</p>
    <p fun>skip</p></div></pre>

Si une fonction retourne une valeur, la syntaxe est la suivante:

<pre noke><div noke><p comment>// fonction avec valeur de retour</p>
<p fun>fun</p> ma_fonction() -> <p kw>int</p>:
    <p fun>return</p> <p tag>42</p></div></pre>

Une fonction doit être placée à l'intérieur d'une classe ou d'une autre fonction. Pour la structure d'un projet, voir la page "structure".

## Classes

Une **classe** permet de définir de nouveaux types d'objets. Une classe a toujours un constructeur `_init`. Le constructeur est exécuté lors de la création d'un objet (on parlera d'instanciation).

**Nb**: Contrairement au Python, il ne faut pas écrire `self` dans les paramètres de chaque fonction de la classe.

Syntaxe basique pour une classe :

<pre noke><div noke><p fun>class</p> <p kw>Ship</p>:
    <p fun>fun _init</p>():
        <p comment>// some code</p>
</div></pre>

### Propriétés

Une classe peut contenir des propriétés (= variables qui sont propre à chaque objet de la classe). Par exemple, un bâteau peut avoir un nom.

Des propriétés sont idéalement déclarées dans le corps de la classe, en premier.

Par ailleurs, le mot clé `this` réprésente l'objet actuel, comme montré dans le code ci-dessous. Il existe également une notation raccourcie pour accéder à une propriété, en utilisant le préfixe `$` avant le nom de cette propriété. Les deux notations sont parfaitement équivalentes.

<pre noke><div noke><p fun>class</p> <p kw>Ship</p>:

    <p comment>// Propriétés</p>
    name : <p kw>string</p>
    length : <p kw>int</p>

    <p fun>fun _init</p>():
        <p fun>this</p>.name = <p tag>"Titanic"</p>
        <p comment>// raccourci</p>
        $length = <p tag>269</p></div></pre>

Ainsi, il est possible d'assigner aux propriétés d'un objet des paramètres passés au constructeur, voire même de directement rediriger un paramètre vers une propriété (en utilisant le préfixe `$`).


<pre noke><div noke><p fun>class</p> <p kw>Ship</p>:

    <p comment>// Propriétés</p>
    name : <p kw>string</p>
    length : <p kw>int</p>
    width : <p kw>int</p>

    <p fun>fun _init</p>(name : <p kw>string</p>, $width = <p tag>100</p>, length := <p tag>42</p>):
        $name = name
        $length = length</div></pre>

Si des valeurs par défaut sont données aux propriétés, le compilateur le comprendra comme un assignement placé au début du constructeur. Écraser ce paramètre dans le constructeur écrase donc la valeur par défaut.

<pre noke><div noke><p fun>class</p> <p kw>Ship</p>:
    name : <p kw>string</p> = <p tag>"Titanic"</p>
    <p fun>fun _init</p>()

<p comment>// Équivalent à</p>
<p fun>class</p> <p kw>Ship</p>:
    name := <p tag>"Titanic"</p>
    <p fun>fun _init</p>()

<p comment>// Équivalent à</p>
<p fun>class</p> <p kw>Ship</p>:
    name : <p kw>string</p>
    <p fun>fun _init</p>():
        $name = <p tag>"Titanic"</p></div></pre>

Il est important de savoir qu'une propriété n'est **pas obligatoirement** déclarée dans le corps d'une classe. Le compilateur se chargera en effet de récupérer les déclarations du code des fonctions. Pour une meilleure lisibilité, il est toutefois conseillé de déclarer chaque propriété dans le corps de la classe.

<pre noke><div noke><p fun>class</p> <p kw>Ship</p>:
    <p fun>fun _init</p>($length := <p tag>269</p>):
        $name := <p tag>"Titanic"</p>

<p comment>// Équivalent à</p>
<p fun>class</p> <p kw>Ship</p>:
    name : <p kw>string</p>
    length : <p kw>int</p>
    <p fun>fun _init</p>(length := <p tag>269</p>):
        $name = <p tag>"Titanic"</p>
        $length = length</div></pre>

### Fonctions

Une classe peut également contenir des fonctions, qui sont conventionnellement placées après le constructeur.
Pour appeler une fonction de la classe depuis l'intérieur de celle-ci, le mot-clé `this` ou le préfixe `$` peuvent être utilisés.

<pre noke><div noke><p fun>class</p> <p kw>Ship</p>:
    <p fun>fun _init</p>():
        $send_sos(<p tag>"On se les gèle"</p>)
        
    <p fun>fun</p> send_sos(message : <p kw>string</p>):
        log(message)</pre></div>

## Objets

Pour instancier une classe, la syntaxe est la suivante :

<pre noke><div noke><p comment>// Supposons que la classe Ship est la suivante</p>
<p fun>class</p> <p kw>Ship</p>:
    name : <p kw>string</p>
    length : <p kw>int</p>
    <p fun>fun _init</p>($name, $length)
    <p fun>fun</p> send_sos():
        log(<p tag>"SOS"</p>)

<p comment>// Instanciation</p>
my_ship := <p kw>Ship</p>(<p tag>"Titanic"</p>, <p tag>269</p>)
<p comment>// Équivalent à</p>
my_ship : <p kw>Ship</p> = <p kw>Ship</p>(<p tag>"Titanic"</p>, <p tag>269</p>)
<p comment>// Équivalent à</p>
my_ship : <p kw>Ship</p> = <p kw>Ship</p>(name = <p tag>"Titanic"</p>, length = <p tag>269</p>)</div></pre>

Il est ensuite possible d'accéder aux propriétés et aux fonctions de cet objet.

<pre noke><div noke>log(my_ship.name)
<p comment>// output: "Titanic"</p>
my_ship.name = <p tag>"Olympic"</p>
log(my_ship.name)
<p comment>// output: "Olympic"</p>
my_ship.send_sos()
<p comment>// output: "SOS"</p>
</div></pre>

Il existe une syntaxe alternative pour instancier un objet:

<pre noke><div noke>my_ship := <p kw>Ship</p>:
    name <p tag>"Titanic"</p>
    length <p tag>269</p></div></pre>

Cette syntaxe est très utilisée lorsque de nombreux objets possédant de nombreux paramètres doivent être instanciés, parce exemple lorsque l'on décrit une interface utilisateur. 

**Remarque**: Les paramètres utilisés dans la syntaxe alternative sont les paramètres du constructeur. Si un paramètre du constructeur est facultatif, il peut également être omis dans la syntaxe alternative.

Il existe d'autres fonctionnalités avec les classes et objets, cf. la partie "Syntaxe avancée".