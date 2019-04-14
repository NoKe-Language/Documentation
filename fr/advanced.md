# Syntaxe avancée

## Flagger

Un **flagger** est un concept spécial de NoKe qui permet d'améliorer la lisibilité d'une instanciation.

Pour bien comprendre le concept, considérons la situation suivante :

<pre noke><div noke><p fun>class</p> <p kw>Button</p>:
    <p comment>// Coordonnées du point supérieur gauche</p>
    x : <p kw>int</p>
    y : <p kw>int</p>
    <p comment>// Dimensions</p>
    width : <p kw>int</p>
    length : <p kw>int</p>
    
    <p fun>fun _init</p>($x, $y, $width, $length)

mon_bouton := <p kw>Button</p>:
    x <p tag>10</p>
    y <p tag>10</p>
    width <p tag>100</p>
    height <p tag>100</p>
</pre></div>

Pour initialiser un grand nombre de boutons, la syntaxe classique rend rapidement le code très lourd. Grâce au flagger, il serait possible d'initaliser le bouton de cette manière :

<pre noke><div noke>mon_bouton := <p kw>Button</p>:
    pos x@<p tag>10</p>:<p tag>110</p> y@auto</div></pre>

Décomposons la deuxième ligne :
* `pos`: Nom du flagger dans les paramètres du constructeur de la classe.
* `x@10:110`: Premier **flag**.
    * `x`: id du flag
    * `@`: Symbole séparateur
    * `10:110`: valeur du flag, ici un `range` literal.
* `y@auto`: Deuxième **flag**.
    * `y`: id du flag
    * `@`: Symbole séparateur
    * `auto`: valeur du flag, ici un `string` literal. (S'il ne comporte pas d'espace, les `"` peuvent être omis)

Il peut y avoir autant de flags que l'on souhaite, mais il faut que l'id soit déclaré dans le flagger.

Si le type de la valeur d'un flag est `void`, seule l'id de celui-ci peut être écrit (en omettant donc le symbole séparateur et la valeur).

Pour ajouter un flagger dans une classe, la syntaxe est la suivante :

<pre noke><div noke><p fun>class</p> <p kw>Ship</p>:
    <p comment>// Propriétés</p>
    name : <p kw>string</p>
    length : <p kw>int</p>

    ???
    <p fun>fun _init</p>($pos):

</div></pre>

mouais

