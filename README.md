# Toboggan

Voici le code source de mon [TIPE](https://fr.wikipedia.org/wiki/Travail_d%27initiative_personnelle_encadr%C3%A9), ainsi que les fichiers déposés sur le site [SCEI](https://scei-concours.fr).

L'objectif de ce TIPE est de trouver une solution au [problème de la brachistochrone](https://fr.wikipedia.org/wiki/Courbe_brachistochrone) dans le cas où l'expression des frottements est trop complexe pour une résolution analytique. Pour cela, j'ai développé un algorithme à mi-chemin entre les algorithmes génétiques et les algorithmes déterministes, que vous trouverez dans le fichier `toboggan.py`.

## toboggan.py

C'est l'unique fichier de code, il s'utilise directement dans une console :

```console
# Calcule le toboggan optimal pour le dé pendant 60 secondes
python toboggan.py
# Calcule pendant 5 minutes (300 secondes)
python toboggan.py 300
# Exporte la figure au format svg (dans le fichier toboggan.svg)
python toboggan.py 300 svg
```

![Recherche du toboggan optimal pour le dé](https://raw.githubusercontent.com/GauBen/Toboggan/master/images/toboggan-optimal.gif)

Les forces exercées sur le mobile pour le calcul du toboggan optimal se trouvent **ligne 346**. Vous pouvez utiliser n'importe quelles forces dépendantes de l'inclinaison et de la vitesse :

```python
# Aucun frottement (pour la recherche de la brachistochrone) :
def appliquer_pfd(x, y):
    g_sin_theta = 9.81 * y / (y*y + x*x) ** 0.5
    # Renvoie la dérivée de la vitesse v exprimée en fonction d'elle-même
    return lambda v: g_sin_theta

# Frottement solide uniquement :
f = 0.32
def appliquer_pfd(x, y):
    g_sin_theta = 9.81 * y / (y*y + x*x) ** 0.5
    fg_cos_theta = f * 9.81 * x / (y*y + x*x) ** 0.5
    # Renvoie la dérivée de la vitesse v exprimée en fonction d'elle-même
    return lambda v: g_sin_theta - fg_cos_theta
```

Toutes les combinaisons sont possibles. La combinaison par défaut est `dv/dt = gsinθ − 0.3263gcosθ − 0.0026v − 0.4748v²`, ce qui correspond à un dé en plastique classique (6 faces gravées, 18 mm, 7 g).

Voici les animations respectives des deux exemples ci-dessus :

![Recherche de la brachistochrone](https://raw.githubusercontent.com/GauBen/Toboggan/master/images/brachistochrone.gif)

![Recherche du toboggan optimal pour les frottements solides](https://raw.githubusercontent.com/GauBen/Toboggan/master/images/frottement-solide.gif)

## Présentation

Pour créer le pdf de la présentation, vous aurez besoin de :

* [Node.js](https://nodejs.org)
* [Marp](https://github.com/marp-team/marp-cli) (`npm install -g @marp-team/marp-cli`)

Dans une console dans le dossier `presentation/` tapez simplement :

```
marp slides.md -o slides.pdf
```

## Remerciements

* Mes professeurs encadrants pour leurs réponses à mes nombreuses questions
* L'équipe de TIPE du lycée Saint Louis pour l'organisation et le matériel
* [@yhatt](https://github.com/yhatt) pour l'outil [Marp](https://github.com/marp-team/marp)
* [@jaredpetersen](https://github.com/jaredpetersen) pour le site [CodePrinter](https://jaredpetersen.github.io/codeprinter/)
* Les sites [ezGIF](https://ezgif.com/) et [PDF Compressor](https://pdfcompressor.com/)