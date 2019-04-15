# Renderer

## Introduction

Le rôle du renderer est de convertir les requêtes du bytecode en requêtes systèmes et/ou openGL.

## Fonctionnement

Considérons le bout de programme suivant.

```noke
0x0006 0x0102 0x0042 0x0001
0x0042 0x0042 0x0002 0x000f
0x0102 0x0042
```
Prenons le premier élément : `0x0006`. Selon la table des opcodes, cela correspond à l'instruction get. Dés lors, les deux éléments suivants nous intéresse.

```noke
0x0006 0x0102 0x0042
```

Après extraction, on peut remarquer qu'il s'agit en effet d'une instruction get, avec un target en registre `0x0042` d'une propriété `0x0102`.

```noke
0x0001 0x0042 0x0042 0x0002
```

Il s'agit d'une simple addition dans le cas présent. Nous allons incrémenter la valeur du registre `0x0042` par 2.

```noke
0x000f 0x0102 0x0042
```

Inverse du premier cas, ceci est un set. La valeur modifiée en `0x0042` va être réenregistrée dans la propriété `0x0102`.