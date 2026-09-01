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


---

## Scripts ArcPy pratiques

### Script 1 — Reprojeter des couches administratives en WGS 84

Ce script parcourt deux shapefiles, construit leur chemin d’entrée, puis les reprojette dans une géodatabase avec le système **EPSG:4326**.

```python
import os
import arcpy as ap

# Autoriser le remplacement des sorties existantes
ap.env.overwriteOutput = True

# Données à reprojeter
fclasses_in = (
    "COMMUNE.shp",
    "LIMITE_ADMINISTRATIVE.shp",
)

input_folder = (
    r"D:\A4_arcpy\ROUTE500_3-0__SHP_LAMB93_FXX_2021-11-03 (1)"
    r"\ROUTE500_3-0__SHP_LAMB93_FXX_2021-11-03"
    r"\ROUTE500\1_DONNEES_LIVRAISON_2022-01-00175"
    r"\R500_3-0_SHP_LAMB93_FXX-ED211\ADMINISTRATIF"
)

output_gdb = r"C:\Users\aelmi\Documents\ArcGIS\Projects\arpy\arpy.gdb"

for fclass_in in fclasses_in:
    print(f"Traitement de la donnée : {fclass_in}")

    in_dataset = os.path.join(input_folder, fclass_in)
    output_name = f"{os.path.splitext(fclass_in)[0]}_4326"
    out_dataset = os.path.join(output_gdb, output_name)

    ap.management.Project(
        in_dataset=in_dataset,
        out_dataset=out_dataset,
        out_coor_system=ap.SpatialReference(4326),
    )

ap.env.overwriteOutput = False
```

!!! tip
    `os.path.join()` construit les chemins proprement. `os.path.splitext()` enlève l’extension `.shp` sans découpage manuel.

### Script 2 — Créer des zones tampons de 100 mètres

Ce script crée un buffer dissous de **100 mètres** autour des couches du réseau ferré.

```python
import os
import arcpy as ap

# Autoriser le remplacement des sorties existantes
ap.env.overwriteOutput = True

# Données du réseau ferré
fclasses_in = (
    "NOEUD_FERRE.shp",
    "TRONCON_VOIE_FERREE.shp",
)

input_folder = (
    r"D:\A4_arcpy\ROUTE500_3-0__SHP_LAMB93_FXX_2021-11-03 (1)"
    r"\ROUTE500_3-0__SHP_LAMB93_FXX_2021-11-03"
    r"\ROUTE500\1_DONNEES_LIVRAISON_2022-01-00175"
    r"\R500_3-0_SHP_LAMB93_FXX-ED211\RESEAU_FERRE"
)

output_folder = r"C:\Users\aelmi\Documents\ArcGIS\Projects\arpy"

for fclass_in in fclasses_in:
    print(f"Traitement de la donnée : {fclass_in}")

    in_dataset = os.path.join(input_folder, fclass_in)
    output_name = f"{os.path.splitext(fclass_in)[0]}_buffer_100m.shp"
    out_dataset = os.path.join(output_folder, output_name)

    ap.analysis.Buffer(
        in_features=in_dataset,
        out_feature_class=out_dataset,
        buffer_distance_or_field="100 Meters",
        dissolve_option="ALL",
    )

ap.env.overwriteOutput = False
```

!!! note
    `arcpy.analysis.Buffer` est l’outil standard adapté à des shapefiles locaux dans ArcGIS Pro. `arcpy.gapro.CreateBuffers` appartient aux GeoAnalytics Tools et ne convient pas à tous les environnements ou types de licences.

### Points importants

- Le bloc placé dans la boucle `for` doit être indenté.
- Vérifie que les dossiers et la géodatabase existent avant l’exécution.
- EPSG:4326 correspond au système géographique WGS 84.
- Remets `overwriteOutput` à `False` si tu veux empêcher l’écrasement accidentel des résultats.
