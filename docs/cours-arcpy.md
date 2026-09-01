# Cours Python & ArcPy

## Commentaires

Un commentaire commence par `#` et n’est pas exécuté par Python.

```python
# Ceci est un commentaire
a = 1  # commentaire en fin de ligne
```

## Variables et types

```python
a = 1                              # entier : int
b = 1.0                            # décimal : float
c = "text"                         # texte : str
d = [1, 2.0, "truc", a, b, c]     # liste : list
e = (1, 2.0, "truc", a, b, c)     # tuple : tuple
f = {"key1": "valeur1", "r": 45}   # dictionnaire : dict
g = set([1, 1, 2, 3])              # ensemble : {1, 2, 3}
```

La fonction `type()` indique le type d’une valeur et `dir()` liste ses attributs et méthodes.

```python
type(a)  # <class 'int'>
dir(a)
```

### Chaînes de caractères

```python
c = "text"

c[1]       # 'e'
c[1:3]     # 'ex'
c[1:]      # 'ext'
c[-1]      # 't'
c[::-1]    # 'txet'
```

Les indices commencent à `0`. Une tranche suit la forme `[début:fin:pas]`.

### Liste et tuple

Une liste est **modifiable** :

```python
d = [1, 2.0, "truc"]
d[0] = 45
print(d)  # [45, 2.0, 'truc']
```

Un tuple est **immuable** :

```python
e = (1, 2.0, "truc")
# e[0] = 45  # TypeError
```

### Dictionnaire et ensemble

```python
f = {"key1": "valeur1", "r": 45}
print(f["key1"])  # valeur1

g = {1, 1, 2, 3}
print(g)          # {1, 2, 3}
print(type(g))    # <class 'set'>
```

Un ensemble supprime les doublons. La fonction correcte est `type(g)`, et non `stype(g)`.

## Importer ArcPy

```python
import arcpy as ap

print(ap)
```

!!! note
    ArcPy est fourni avec ArcGIS Pro. Il faut exécuter le code dans l’environnement Python associé à ArcGIS Pro.

## Structures conditionnelles

L’indentation est obligatoire en Python.

```python
a = 5
b = 6

if a < b:
    print("a est inférieur à b")
elif a > b:
    print("a est supérieur à b")
else:
    print("a est égal à b")
```

## Boucles

### Boucle `for`

```python
for valeur in [1, 2, 3]:
    print(valeur)
```

### Boucle `while`

```python
compteur = 0

while compteur < 3:
    print(compteur)
    compteur += 1
```

### `break` et `continue`

- `break` arrête immédiatement la boucle ;
- `continue` passe directement à l’itération suivante.

```python
for nombre in range(10):
    if nombre == 2:
        continue
    if nombre == 6:
        break
    print(nombre)
```

## Environnement noté

Les essais ont été réalisés avec Python 3.13.7, distribué par Anaconda, sous Windows 64 bits.
