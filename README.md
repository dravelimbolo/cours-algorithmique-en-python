
# Cours 1 - Algorithmique en Python
### Du débutant à l'expert

> Ceci est un vrai cours, pas un mémo. Lis-le dans l'ordre, sans sauter. Chaque section construit la suivante. À la fin, tu auras compris *pourquoi* les choses fonctionnent comme elles fonctionnent - et pas juste *comment* taper les bonnes touches.

---

## Avant-propos : qu'est-ce que programmer, vraiment ?

Avant la première ligne de code, comprends ceci : un ordinateur est une machine d'une rapidité prodigieuse et d'une bêtise totale. Il exécute des instructions une par une, dans l'ordre exact où tu les écris, sans jamais deviner ce que tu voulais vraiment dire. Si tu lui demandes de diviser par zéro, il essaye. Si tu oublies de lui dire d'arrêter, il continue éternellement. Il n'a aucun bon sens.

Programmer, c'est donc apprendre à **parler à une entité littérale**. Cette compétence ne vient pas de la mémorisation de la syntaxe - la syntaxe se révise en une heure. Elle vient de la capacité à **décomposer un problème en étapes assez petites pour qu'un idiot puisse les suivre**. Tous les bons développeurs ont ça en commun. Tous les bugs viennent du moment où on a oublié que l'ordinateur ne devine pas.

Garde cette image présente en permanence pendant ce cours. Quand un programme ne marche pas, ne demande pas "pourquoi il ne comprend pas ?". Demande "qu'est-ce que je lui ai dit, *exactement* ?". La réponse est presque toujours différente de ce que tu croyais lui avoir dit.

Python est un langage particulièrement adapté pour apprendre, parce qu'il est lisible. Une ligne Python ressemble souvent à de l'anglais. Moins tu écris, moins tu te trompes.

---

## 1. Les variables : ranger des choses pour s'en resservir

### 1.1 L'idée de base

Une variable est un nom donné à une valeur, pour pouvoir s'y référer plus tard. C'est l'équivalent informatique d'une étiquette collée sur une boîte. Tu mets quelque chose dans la boîte, tu colles une étiquette dessus, et désormais quand tu prononces le nom de l'étiquette, c'est comme si tu prononçais le contenu.

```python
age = 25
```

Cette ligne se lit littéralement : "prends la valeur 25, mets-la quelque part en mémoire, et appelle cet endroit `age`". À partir de là, partout où tu écris `age`, Python comprend `25`.

```python
print(age)         # affiche 25
print(age + 5)     # affiche 30
print(age * 2)     # affiche 50
```

Le signe `=` en programmation n'est **pas** le signe égal des mathématiques. En maths, `x = 3` est une affirmation symétrique : x est égal à 3, et 3 est égal à x. En programmation, `age = 25` est une **action** : "range 25 dans la boîte age". C'est unidirectionnel : la valeur de droite est rangée dans la variable de gauche. Tu ne peux pas écrire `25 = age`, ça n'a aucun sens pour Python.

### 1.2 Modifier une variable

Le contenu d'une variable peut changer. C'est même tout l'intérêt.

```python
score = 0
score = score + 10
score = score + 5
print(score)       # affiche 15
```

Lis la deuxième ligne très lentement : "prends ce qu'il y a actuellement dans score, ajoute-lui 10, et range le résultat dans score". L'ancienne valeur est écrasée. C'est ce qui te permet d'accumuler, de compter, de progresser pendant qu'un programme tourne.

Comme cette opération est très fréquente, Python offre un raccourci :

```python
score = 0
score += 10        # équivaut à : score = score + 10
score += 5
print(score)       # 15
```

Idem pour `-=`, `*=`, `/=`. Ce n'est pas plus rapide pour la machine, mais c'est plus rapide à taper et plus lisible.

### 1.3 Les types de données

Une variable a un **type**. Le type dit *quel genre* de chose contient la boîte. Les principaux :

- **`int`** (entier) : `42`, `-7`, `0`
- **`float`** (nombre à virgule) : `3.14`, `-0.5`, `2.0`
- **`str`** (chaîne de caractères) : `"bonjour"`, `'Paris'`
- **`bool`** (booléen) : `True` ou `False`, rien d'autre
- **`list`**, **`dict`**, etc. : on y vient.

Le type compte parce que les opérations possibles dépendent du type. Tu peux additionner deux entiers, mais que se passe-t-il si tu additionnes une chaîne et un entier ?

```python
"3" + 5            # ERREUR ! str + int impossible
```

Python refuse parce qu'il ne sait pas ce que tu veux : ajouter 5 à la chaîne "3" pour obtenir "8" ? Ou ajouter 5 au nombre 3 pour obtenir 8 ? C'est ambigu, donc il arrête plutôt que de deviner. C'est une bonne chose : un langage qui devine fait des bugs silencieux. Python te force à clarifier :

```python
int("3") + 5       # 8   (conversion explicite vers int)
"3" + str(5)       # "35"  (concaténation de chaînes)
```

Au passage, `"3" + "5"` ne donne pas `8` mais `"35"` : pour les chaînes, le `+` veut dire "colle les deux bout à bout". C'est la **concaténation**. Tu rencontreras ça partout.

### 1.4 Nommer correctement (niveau confirmé)

Un mauvais nom de variable est la première source de bugs après les erreurs de syntaxe. Compare :

```python
x = 12.5
t = 60
r = x * t          # qu'est-ce que c'est censé faire ?
```

versus :

```python
prix_horaire = 12.5
duree_minutes = 60
prix_total = prix_horaire * (duree_minutes / 60)
```

Le deuxième programme se documente lui-même. Tu le relis trois mois plus tard, tu comprends instantanément. Le premier, tu es perdu en cinq secondes. Sous chrono pendant un test, des noms clairs t'évitent les bugs idiots où tu inverses deux variables sans t'en rendre compte.

Les règles formelles : un nom de variable commence par une lettre ou `_`, contient lettres/chiffres/underscores, pas d'accents, pas d'espaces, sensible à la casse (`age` et `Age` sont deux variables différentes - évite ce piège).

### 1.5 Niveau expert : les variables ne contiennent pas les valeurs

C'est un détail qu'on découvre quand on commence à déboguer des programmes compliqués. En réalité, une variable Python n'est pas une boîte qui *contient* une valeur, mais une **étiquette** qui *pointe* vers une valeur stockée ailleurs en mémoire. Pour les types simples (`int`, `str`, `bool`, `float`) ça ne change rien à ton intuition. Mais avec les listes ou les dictionnaires, ça compte :

```python
a = [1, 2, 3]
b = a              # b pointe vers la MÊME liste que a
b.append(4)
print(a)           # [1, 2, 3, 4]  ← a a changé aussi !
```

Tu n'as pas copié la liste, tu as collé une deuxième étiquette dessus. Si tu veux une vraie copie indépendante, demande-le explicitement : `b = a.copy()` ou `b = list(a)`. Ce piège a fait pleurer beaucoup de débutants ; mieux vaut le connaître à l'avance.

---

## 2. Les opérateurs : combiner les valeurs

### 2.1 Opérateurs arithmétiques

```python
7 + 3              # 10
7 - 3              # 4
7 * 3              # 21
7 / 3              # 2.333...  (division réelle, donne toujours un float)
7 // 3             # 2         (division entière, jette les décimales)
7 % 3              # 1         (modulo : le reste de la division)
7 ** 3             # 343       (puissance : 7 au cube)
```

Les deux que les débutants oublient le plus souvent sont `//` et `%`. Pourtant, `%` est ton outil principal pour deux questions courantes en algo :

1. **Tester si un nombre est divisible par un autre.** `n % 5 == 0` est vrai si et seulement si n est un multiple de 5.
2. **Tester la parité.** `n % 2 == 0` est vrai pour les nombres pairs, faux pour les impairs.

Apprends ces deux usages par cœur, ils tombent dans 50% des exercices d'algo.

### 2.2 Opérateurs de comparaison

Ils produisent un booléen (`True` ou `False`) :

```python
5 == 5             # True  (deux signes égal : "est égal à")
5 != 3             # True  ("est différent de")
5 < 10             # True
5 <= 5             # True
5 > 10             # False
```

Le piège mortel : `=` versus `==`. Un seul signe = c'est une affectation, l'action de ranger une valeur. Deux signes == c'est une comparaison, la question "est-ce que c'est égal ?". Si tu écris `if age = 18:`, Python te crie dessus. Si tu écris `age == 18` tout seul sur une ligne sans `if`, il ne fait rien d'utile : il calcule un booléen et le jette. C'est une question silencieuse à laquelle personne ne répond.

### 2.3 Opérateurs logiques

Pour combiner plusieurs conditions :

```python
age = 20
solde = 500

age >= 18 and solde > 0       # True  (les DEUX doivent être vrais)
age >= 18 or solde > 0        # True  (AU MOINS UN doit être vrai)
not (age >= 18)               # False (inverse la valeur)
```

Note bien le `or` : dans le langage courant, "ou" peut être exclusif ("fromage *ou* dessert, choisis !"). En programmation, le `or` est toujours **inclusif** : "fromage ou dessert ou les deux".

---

## 3. Les conditions : choisir un chemin

### 3.1 La forme de base

Une condition permet à ton programme de prendre des décisions. Sans condition, tous tes programmes feraient toujours exactement la même chose. Avec elle, ils s'adaptent.

```python
age = 20

if age >= 18:
    print("majeur")
    print("a le droit de voter")
else:
    print("mineur")
```

Trois choses sont essentielles dans cette syntaxe Python :

1. Les **deux-points** `:` à la fin de la ligne `if`. Ils annoncent que ce qui suit est le bloc à exécuter si la condition est vraie. Si tu les oublies, Python proteste.
2. L'**indentation** (le décalage à droite, quatre espaces ou une tabulation, mais sois cohérent). En Python, l'indentation n'est pas décorative : elle dit *quelles lignes appartiennent au if*. Une ligne non indentée est en dehors du `if`, elle s'exécute toujours. Ce point différencie Python de la plupart des autres langages.
3. La structure `else:` (toujours collée à un `if`, avec le même niveau d'indentation que lui) capte tout ce qui ne tombe pas dans le `if`.

### 3.2 Plusieurs cas avec `elif`

Quand tu as plus de deux situations, n'empile pas des `if` séparés. Utilise `elif` (contraction de "else if") :

```python
note = 14

if note >= 16:
    print("très bien")
elif note >= 14:
    print("bien")
elif note >= 12:
    print("assez bien")
elif note >= 10:
    print("passable")
else:
    print("recalé")
```

Python teste les conditions dans l'ordre. Dès qu'une est vraie, il exécute son bloc et **saute tout le reste** de la cascade. C'est pour ça que l'ordre compte : `if note >= 10` placé en premier attraperait *toutes* les bonnes notes. Tu dois aller du plus restrictif au plus large.

### 3.3 Conditions imbriquées et combinées

Tu peux mettre des `if` dans des `if`, mais souvent c'est plus lisible de combiner avec `and`/`or` :

```python
# Imbriqué - fonctionne mais lourd
if age >= 18:
    if solde > 0:
        print("crédit accordé")

# Combiné - équivalent, plus clair
if age >= 18 and solde > 0:
    print("crédit accordé")
```

Choisis l'imbriqué quand chaque niveau a son propre `else` à gérer ; sinon, combine.

### 3.4 Niveau confirmé : l'expression ternaire

Quand tu veux assigner une valeur différente selon une condition, tu peux condenser en une ligne :

```python
# Version normale
if age >= 18:
    statut = "majeur"
else:
    statut = "mineur"

# Version ternaire (équivalente)
statut = "majeur" if age >= 18 else "mineur"
```

À lire littéralement : "majeur **si** age est ≥ 18, **sinon** mineur". Ne l'utilise pas pour des cas complexes (devient illisible), mais pour les choix binaires courts, c'est plus propre.

### 3.5 Niveau expert : la vérité des valeurs non-booléennes

Python est tolérant. Dans un `if`, il accepte autre chose qu'un booléen pur : il regarde si la valeur est "vraisemblable" ou pas. Les valeurs considérées comme **fausses** sont : `False`, `0`, `0.0`, `""` (chaîne vide), `[]` (liste vide), `{}` (dico vide), `None`. Tout le reste est considéré comme vrai.

```python
nom = ""
if nom:
    print(f"Bonjour {nom}")
else:
    print("Veuillez entrer votre nom")
```

`if nom:` est ici un raccourci pour `if nom != "":`. C'est élégant, à condition de connaître la règle. Sinon, c'est mystérieux. Maintenant tu la connais.

---

## 4. Les boucles : répéter sans s'épuiser

### 4.1 Pourquoi des boucles ?

Si tu veux afficher les nombres de 1 à 10, tu pourrais écrire dix lignes :

```python
print(1)
print(2)
print(3)
# ... etc
```

Pour dix, ça passe. Pour mille, c'est ridicule. Pour les éléments d'une liste dont tu ne connais pas la taille à l'avance, c'est impossible. Les boucles existent pour ça : répéter un bloc d'instructions, autant de fois que nécessaire, sans le réécrire.

### 4.2 La boucle `for` : quand on sait sur quoi parcourir

La boucle `for` en Python parcourt une **collection** : une liste, une chaîne, une plage de nombres, ce que tu veux d'itérable.

```python
fruits = ["pomme", "poire", "kiwi"]
for fruit in fruits:
    print(fruit)
```

À chaque tour de boucle, la variable `fruit` prend la valeur de l'élément suivant. Au premier tour `fruit = "pomme"`, au deuxième `fruit = "poire"`, etc. Quand il n'y a plus d'éléments, la boucle s'arrête toute seule.

Si tu veux juste répéter quelque chose un nombre fixé de fois, ou parcourir une suite de nombres, utilise `range` :

```python
for i in range(5):
    print(i)
# affiche 0, 1, 2, 3, 4
```

`range(5)` génère les nombres de 0 à 4 inclus (5 valeurs, en commençant à 0 - toujours à 0 sauf indication contraire). C'est une convention universelle en programmation, et elle te jouera des tours au début. Apprends à dire mentalement : "range(N), c'est N valeurs qui commencent à 0".

Les autres formes utiles :

```python
range(1, 11)       # 1, 2, 3, ..., 10  (le 11 est EXCLU)
range(0, 20, 2)    # 0, 2, 4, 6, ..., 18  (pas de 2)
range(10, 0, -1)   # 10, 9, 8, ..., 1  (à l'envers)
```

La règle générale : `range(début, fin, pas)`. Le début est inclus, la fin est **exclue**. C'est l'asymétrie qui surprend les débutants : pour aller jusqu'à 10 inclus, tu écris `range(1, 11)`. Pourquoi cette convention bizarre ? Parce que `range(0, N)` te donne exactement N valeurs, ce qui est bien plus pratique pour la plupart des calculs. Tu finis par l'aimer.

### 4.3 Le schéma d'accumulation

Voici le **schéma roi** des boucles, à mémoriser absolument :

```python
nombres = [4, 8, 15, 16, 23, 42]
total = 0                         # 1. on prépare un accumulateur vide
for n in nombres:                 # 2. on parcourt
    total = total + n             # 3. on accumule
print(total)                      # 108
```

Trois étapes : initialiser, parcourir, accumuler. Ce schéma résout : faire une somme, compter des éléments selon un critère, trouver un maximum, concaténer des chaînes, et beaucoup d'autres choses. Tu vas le réécrire des centaines de fois. Garde l'image : une boîte vide qu'on remplit en passant devant chaque wagon du train.

Variante pour compter :

```python
notes = [12, 8, 17, 5, 14, 6]
nb_admis = 0
for note in notes:
    if note >= 10:
        nb_admis += 1
print(nb_admis)                   # 3
```

Variante pour trouver le maximum (sans utiliser `max`) :

```python
nombres = [3, 18, 7, 25, 11]
champion = nombres[0]             # on suppose que le premier est le champion
for n in nombres:
    if n > champion:
        champion = n              # un challenger plus fort détrône
print(champion)                   # 25
```

### 4.4 La boucle `while` : tant que la condition tient

Quand tu ne sais pas à l'avance combien de tours tu vas faire, mais tu sais à quelle condition tu dois arrêter, utilise `while` :

```python
mot_de_passe = ""
while mot_de_passe != "secret":
    mot_de_passe = input("Mot de passe : ")
print("Accès autorisé")
```

La boucle tourne tant que la condition est vraie. **À chaque tour**, la condition est re-testée.

Le danger numéro un : la **boucle infinie**. Si la condition ne devient jamais fausse, ton programme tourne pour l'éternité (et bloque ton ordinateur). Exemple piégeux :

```python
n = 10
while n > 0:
    print(n)
    # OUPS, j'ai oublié de modifier n. La condition reste vraie pour toujours.
```

Règle d'or : à chaque fois que tu écris un `while`, demande-toi à voix haute : "qu'est-ce qui va faire que cette condition devienne fausse ?". Si tu ne peux pas répondre, n'écris pas la boucle.

### 4.5 `break` et `continue`

Deux mots-clés pour contrôler finement le déroulement d'une boucle :

- `break` : sortir immédiatement de la boucle, peu importe la condition.
- `continue` : sauter la fin du tour courant et passer directement au suivant.

```python
# Chercher si un mot existe dans une liste, s'arrêter dès qu'on le trouve
mots = ["chat", "chien", "lapin", "souris"]
for mot in mots:
    if mot == "lapin":
        print("trouvé !")
        break                     # inutile de continuer

# Afficher les nombres de 1 à 10 sauf les multiples de 3
for n in range(1, 11):
    if n % 3 == 0:
        continue                  # on saute ce tour
    print(n)
```

### 4.6 Niveau expert : les boucles imbriquées et la complexité

Tu peux mettre une boucle dans une boucle. C'est puissant, mais attention au coût.

```python
# Affiche une table de multiplication 5x5
for i in range(1, 6):
    for j in range(1, 6):
        print(i * j, end="\t")
    print()                       # nouvelle ligne après chaque rangée
```

La boucle externe tourne 5 fois ; à chaque tour, l'interne tourne 5 fois ; donc le bloc le plus profond s'exécute 5 × 5 = 25 fois. C'est l'idée de **complexité quadratique** : pour N éléments en entrée, le coût est de l'ordre de N². Pour N = 10, c'est 100 opérations (négligeable). Pour N = 100 000, c'est 10 milliards (ça met des heures).

Pourquoi ça te concerne pour le test ? Parce que certains exercices ont une solution naïve en double boucle qui marche pour les petits cas mais explose pour les grands. La plateforme de test mesure parfois le temps. Si l'exercice te dit "soyez efficace", c'est le signal qu'il existe une solution sans double boucle (souvent avec un dictionnaire ou un ensemble).

---

## 5. Les listes : ordonner et manipuler des collections

### 5.1 Création et accès

Une liste est une suite ordonnée de valeurs, modifiable.

```python
notes = [12, 8, 17, 5, 14]
fruits = ["pomme", "poire", "kiwi"]
mixte = [1, "deux", 3.0, True]    # Python accepte tout
vide = []
```

L'accès aux éléments se fait par leur **index**, qui commence à 0 :

```python
notes[0]                          # 12  (premier)
notes[1]                          # 8   (deuxième)
notes[4]                          # 14  (cinquième, dernier)
notes[5]                          # ERREUR : il n'y en a que 5, donc indices 0 à 4
notes[-1]                         # 14  (dernier, en comptant à l'envers)
notes[-2]                         # 5   (avant-dernier)
```

La règle des index à partir de 0 est *la* source de bug n°1 chez les débutants. On l'appelle l'erreur **off-by-one** (décalée d'un cran). Tu écris une boucle qui doit aller de 1 à N, tu écris `range(N)`, et tu obtiens 0 à N-1. Ou tu accèdes à `liste[len(liste)]` en pensant atteindre le dernier, alors que l'index du dernier est `len(liste) - 1`. Reste vigilant.

### 5.2 Modifier une liste

Les listes sont **mutables** : tu peux changer leur contenu après création.

```python
notes = [12, 8, 17, 5, 14]
notes[0] = 20                     # remplace le premier élément
notes.append(15)                  # ajoute à la fin
notes.insert(1, 10)               # insère 10 à l'index 1 (pousse les autres)
notes.remove(8)                   # retire la première occurrence de 8
del notes[0]                      # supprime l'élément à l'index 0
notes.pop()                       # retire le dernier et le retourne
print(notes)
```

`append` et `pop` (en fin de liste) sont rapides. `insert` et `remove` (au milieu) sont plus lents parce qu'il faut décaler tous les éléments suivants. Pour un test, ça n'a pas d'importance, mais c'est bon à savoir.

### 5.3 Le slicing (découpage)

Le slicing te permet d'extraire des sous-listes :

```python
notes = [12, 8, 17, 5, 14, 6, 19]

notes[1:4]                        # [8, 17, 5]    indices 1 à 3 (4 exclu)
notes[:3]                         # [12, 8, 17]   du début à l'indice 2
notes[3:]                         # [5, 14, 6, 19]  de l'indice 3 jusqu'à la fin
notes[:]                          # copie complète
notes[::2]                        # [12, 17, 14, 19]  un sur deux
notes[::-1]                       # [19, 6, 14, 5, 17, 8, 12]  à l'envers
```

Même règle d'asymétrie que `range` : début inclus, fin exclue. C'est cohérent dans tout Python : une fois que tu l'as accepté, ça devient un réflexe.

### 5.4 Parcourir une liste : plusieurs façons

Trois manières utiles selon ton besoin :

```python
fruits = ["pomme", "poire", "kiwi"]

# 1. Si tu n'as besoin que des valeurs
for fruit in fruits:
    print(fruit)

# 2. Si tu as besoin aussi de l'index
for i, fruit in enumerate(fruits):
    print(i, fruit)
    # affiche : 0 pomme / 1 poire / 2 kiwi

# 3. Si tu veux parcourir deux listes en parallèle
prix = [1.50, 2.00, 3.20]
for fruit, p in zip(fruits, prix):
    print(fruit, "coûte", p, "€")
```

`enumerate` et `zip` sont des outils que les débutants méconnaissent mais qui rendent le code dix fois plus clair que les versions avec indices manuels.

### 5.5 Méthodes utiles à connaître

```python
notes = [12, 8, 17, 5, 14]

len(notes)                        # 5
sum(notes)                        # 56
max(notes)                        # 17
min(notes)                        # 5
sorted(notes)                     # [5, 8, 12, 14, 17]  (nouvelle liste triée)
notes.sort()                      # trie la liste sur place (la modifie)
notes.reverse()                   # inverse sur place
12 in notes                       # True  (test d'appartenance)
notes.count(8)                    # 1  (combien de fois 8 apparaît)
notes.index(17)                   # position de 17 (lève une erreur si absent)
```

### 5.6 Niveau confirmé : les compréhensions de liste

C'est une des plus belles features de Python. Au lieu d'écrire :

```python
carres = []
for n in range(1, 6):
    carres.append(n * n)
print(carres)                     # [1, 4, 9, 16, 25]
```

Tu peux écrire en une ligne :

```python
carres = [n * n for n in range(1, 6)]
```

Avec un filtre :

```python
pairs = [n for n in range(20) if n % 2 == 0]
# [0, 2, 4, 6, 8, 10, 12, 14, 16, 18]
```

À lire littéralement : "fabrique une liste contenant n*n pour chaque n dans range(1,6)". C'est exactement la même chose que la boucle expansée, mais condensé. Sous chrono, c'est précieux. Ne l'abuse pas pour des cas complexes (ça devient illisible), mais maîtrise-la pour les cas simples.

---

## 6. Les chaînes de caractères

Une chaîne est techniquement une suite ordonnée de caractères, donc elle ressemble à une liste à plein d'égards.

### 6.1 Création

```python
nom = "Paris"
nom2 = 'Lyon'                     # les deux types de guillemets sont valides
phrase = "Il a dit \"bonjour\""   # antislash pour échapper les guillemets
multiligne = """Sur
plusieurs
lignes"""
```

### 6.2 Accès et slicing

Comme pour une liste :

```python
mot = "developpeur"
mot[0]                            # "d"
mot[-1]                           # "r"
mot[0:4]                          # "deve"
len(mot)                          # 11
```

### 6.3 La grande différence : immutabilité

Tu **ne peux pas** modifier une chaîne en place.

```python
mot = "code"
mot[0] = "M"                      # ERREUR
```

Pour "modifier" une chaîne, tu en crées une nouvelle :

```python
mot = "code"
mot = "M" + mot[1:]               # "Mode"
```

C'est important à comprendre, parce que beaucoup d'exercices de manipulation de chaînes consistent en réalité à construire une nouvelle chaîne lettre par lettre.

### 6.4 Méthodes essentielles

```python
phrase = "Bonjour le monde"

phrase.upper()                    # "BONJOUR LE MONDE"
phrase.lower()                    # "bonjour le monde"
phrase.replace("monde", "Paris")  # "Bonjour le Paris"
phrase.split()                    # ["Bonjour", "le", "monde"]  (sépare sur les espaces)
phrase.split(" ", 1)              # ["Bonjour", "le monde"]  (limite à 1 séparation)
"a,b,c".split(",")                # ["a", "b", "c"]
"-".join(["a", "b", "c"])         # "a-b-c"  (l'inverse de split)
phrase.startswith("Bon")          # True
phrase.endswith("monde")          # True
phrase.find("le")                 # 8  (position, -1 si absent)
"  hello  ".strip()               # "hello"  (enlève espaces autour)
"42".isdigit()                    # True
"abc".isalpha()                   # True
```

`split` et `join` forment une paire que tu utiliseras tout le temps : `split` casse une chaîne en liste, `join` recolle une liste en chaîne. Maîtrise-les.

### 6.5 Formatage : les f-strings

Pour insérer des valeurs dans une chaîne, oublie les vieilles méthodes, utilise les **f-strings** (depuis Python 3.6) :

```python
nom = "Léa"
age = 25
print(f"Je m'appelle {nom} et j'ai {age} ans")
# "Je m'appelle Léa et j'ai 25 ans"
```

Le `f` devant la chaîne active l'interprétation : tout ce qui est entre `{}` est évalué comme du Python et inséré. C'est concis, lisible, et tu peux mettre des expressions :

```python
print(f"Dans 5 ans, j'aurai {age + 5} ans")
```

---

## 7. Les fonctions : factoriser et nommer

### 7.1 Pourquoi des fonctions ?

Quand tu te surprends à copier-coller le même bloc de code à plusieurs endroits, c'est le signal absolu pour créer une fonction. Une fonction te permet de :

1. **Réutiliser** un bloc de code sans le réécrire
2. **Nommer** une intention, ce qui rend ton programme plus lisible
3. **Tester isolément** un comportement
4. **Le modifier en un seul endroit** si tu veux changer la logique

### 7.2 Définition

```python
def saluer(nom):
    print(f"Bonjour {nom}")

saluer("Léa")                     # "Bonjour Léa"
saluer("Tom")                     # "Bonjour Tom"
```

Décortiquons :

- `def` : mot-clé qui dit "je définis une fonction"
- `saluer` : le nom que tu choisis (mêmes règles que les variables)
- `(nom)` : les **paramètres** (les entrées de la machine). Ici, un seul.
- `:` : annonce le bloc
- Le corps est indenté

À la définition, **rien ne s'exécute**. Tu décris juste ce que la fonction *fera* quand on l'appellera. C'est à l'appel (`saluer("Léa")`) que ça tourne.

### 7.3 `return` versus `print`

C'est probablement la confusion la plus fréquente chez les débutants, et elle coûte cher en test.

`print` affiche quelque chose sur l'écran. Point. Ça n'est utile qu'à l'utilisateur humain qui regarde la console.

`return` rend une valeur que **le reste de ton programme peut récupérer et réutiliser**.

```python
def double_p(n):
    print(n * 2)                  # affiche, mais ne rend rien

def double_r(n):
    return n * 2                  # rend la valeur, sans l'afficher

double_p(5)                       # affiche 10
double_r(5)                       # rend 10, mais ne l'affiche pas

x = double_p(5)                   # x vaut None (rien n'a été rendu)
y = double_r(5)                   # y vaut 10
print(y * 3)                      # 30
```

Sous chrono, si tu écris `print` au lieu de `return`, le grader automatique de la plateforme reçoit `None` au lieu de ta vraie réponse, et compte l'exercice comme faux. C'est un piège classique. **Pour un test automatisé, la fonction doit `return`.**

### 7.4 Paramètres : positionnels, par défaut, nommés

```python
def saluer(nom, salutation="Bonjour"):
    return f"{salutation} {nom}"

saluer("Léa")                     # "Bonjour Léa"  (utilise la valeur par défaut)
saluer("Léa", "Salut")            # "Salut Léa"
saluer(nom="Léa", salutation="Hi")  # explicite, ordre indifférent
```

Les paramètres avec valeur par défaut doivent toujours venir **après** les paramètres sans défaut. Sinon Python ne sait pas quoi faire d'un appel ambigu.

### 7.5 Plusieurs valeurs de retour

Une fonction Python peut retourner plusieurs valeurs en les séparant par des virgules. En réalité, elle retourne un **tuple** (une mini-liste figée), mais tu peux le déballer directement :

```python
def min_max(liste):
    return min(liste), max(liste)

petit, grand = min_max([3, 18, 7, 25, 11])
print(petit, grand)               # 3 25
```

### 7.6 La portée des variables

Une variable créée à l'intérieur d'une fonction n'existe que dans cette fonction. C'est sa **portée locale**.

```python
def calculer():
    resultat = 42
    return resultat

calculer()
print(resultat)                   # ERREUR : resultat n'existe pas ici
```

Inversement, une fonction peut **lire** une variable de l'extérieur, mais ne peut pas la modifier sans cérémonie spéciale :

```python
total = 0

def ajouter_un():
    total = total + 1             # ERREUR : Python pense que total est local

# Pour modifier la globale (à éviter en pratique) :
def ajouter_un_bis():
    global total
    total = total + 1
```

En pratique, **évite les variables globales**. Une bonne fonction reçoit ce dont elle a besoin en paramètres et retourne ce qu'elle produit. C'est plus prévisible, plus testable, et c'est ce qu'on attend de toi dans un test.

### 7.7 Niveau expert : la récursion

Une fonction peut s'appeler elle-même. Ça s'appelle la **récursion**, et c'est une façon élégante de résoudre les problèmes qui se définissent en fonction d'eux-mêmes.

Exemple classique : la factorielle. Mathématiquement, n! = n × (n-1)!, et 0! = 1. Tu peux traduire ça mot pour mot :

```python
def factorielle(n):
    if n == 0:
        return 1                  # cas de base : on arrête la récursion
    return n * factorielle(n - 1)

factorielle(5)                    # 120
```

Le secret d'une récursion qui marche : **un cas de base** où la fonction ne s'appelle plus, et **une décroissance** qui rapproche les appels suivants du cas de base. Sans cas de base, tu fais une boucle infinie d'appels et Python lève une erreur de "récursion trop profonde".

Pour le test, la récursion est rarement obligatoire (tout problème récursif peut s'écrire avec une boucle), mais elle est parfois plus claire pour les structures arborescentes. Comprends le principe, ne le force pas.

---

## 8. Les dictionnaires : associer une clé à une valeur

Quand tu veux stocker des paires "étiquette → valeur" plutôt qu'une suite numérotée, utilise un dictionnaire.

```python
personne = {
    "nom": "Dupont",
    "age": 30,
    "ville": "Paris"
}

personne["nom"]                   # "Dupont"
personne["age"] = 31              # modification
personne["email"] = "x@y.fr"      # ajout
del personne["ville"]             # suppression
"nom" in personne                 # True (test d'existence d'une clé)
```

Les dictionnaires sont **non ordonnés conceptuellement** (Python les ordonne depuis 3.7, mais ne t'appuie pas dessus pour ta logique) ; on y accède par clé, pas par position.

### 8.1 Parcourir un dictionnaire

```python
for cle in personne:
    print(cle, personne[cle])

# Plus idiomatique :
for cle, valeur in personne.items():
    print(cle, valeur)
```

### 8.2 Le pattern du compteur

Un usage très fréquent : compter les occurrences. Exemple, compter combien de fois chaque lettre apparaît dans un mot.

```python
mot = "anticonstitutionnellement"
compte = {}
for lettre in mot:
    if lettre in compte:
        compte[lettre] += 1
    else:
        compte[lettre] = 1
print(compte)
```

Encore plus court avec `.get(cle, defaut)` qui retourne `defaut` si la clé n'existe pas :

```python
compte = {}
for lettre in mot:
    compte[lettre] = compte.get(lettre, 0) + 1
```

Ce pattern revient sans cesse. Apprends-le.

---

## 9. Vue d'expert sur la complexité

C'est conceptuel mais important pour le test.

Quand on parle de complexité, on parle du **nombre d'opérations en fonction de la taille de l'entrée**. Si N est la taille de l'entrée :

- **O(1)** : constant. Accéder à `liste[5]` prend le même temps quelle que soit la taille de la liste.
- **O(N)** : linéaire. Parcourir une liste de N éléments.
- **O(N²)** : quadratique. Une double boucle sur la même liste de N éléments.
- **O(log N)** : logarithmique. Très efficace (recherche dichotomique).

Si un exercice fonctionne pour des petites données mais est trop lent pour des grandes, c'est presque toujours qu'on est en O(N²) alors qu'une solution O(N) existe. La technique la plus fréquente pour passer de N² à N est d'**utiliser un dictionnaire ou un ensemble** pour mémoriser ce qu'on a déjà vu.

Exemple : "deux nombres de la liste font-ils la somme cible ?"

Version naïve, O(N²) :

```python
def somme_deux(liste, cible):
    for i in range(len(liste)):
        for j in range(i + 1, len(liste)):
            if liste[i] + liste[j] == cible:
                return True
    return False
```

Version efficace, O(N) :

```python
def somme_deux_rapide(liste, cible):
    vus = set()
    for n in liste:
        if cible - n in vus:      # le complément a déjà été vu
            return True
        vus.add(n)
    return False
```

L'idée : au lieu de comparer chaque paire (N²), on garde la trace de tout ce qu'on a vu, et pour chaque nouveau nombre on demande "est-ce que son complément (cible - n) a déjà été rencontré ?". Le test d'appartenance dans un `set` est en O(1), donc le total est en O(N). C'est l'idée à connaître.

---

## Pour finir : ce qu'il faut retenir

Trois principes, à graver :

1. **L'ordinateur ne devine pas.** Tous tes bugs viendront du moment où tu as supposé qu'il comprendrait quelque chose que tu ne lui as pas explicitement dit.

2. **Les bons noms valent mieux que les bons commentaires.** Une variable bien nommée explique son rôle sans que tu aies à le commenter.

3. **Le schéma "préparer / parcourir / accumuler" résout la majorité des exercices d'algo.** Tu prépares une boîte (un total, un compteur, un maximum, une liste résultat). Tu parcours les données. Tu mets à jour la boîte à chaque tour.

La maîtrise vient de la pratique. Maintenant que tu as les fondations, dans le prochain cours on enchaîne sur SQL : parler aux bases de données. Puis le web. Puis la POO et les frameworks.


## Public visé

* Débutants
* Étudiants
* Développeurs juniors
* Personnes en reconversion

## Auteur

Dravel Imbolo

Développeur Full Stack · Web · Mobile · Agile

[![Portfolio](https://img.shields.io/badge/-dravelimbolo.com-111111?style=for-the-badge&logo=safari&logoColor=white)](https://dravelimbolo.com)
[![LinkedIn](https://img.shields.io/badge/-LinkedIn-111111?style=for-the-badge&logo=linkedin&logoColor=white)](https://linkedin.com/in/dravel-imbolo)
[![GitHub](https://img.shields.io/badge/-GitHub-111111?style=for-the-badge&logo=github&logoColor=white)](https://github.com/dravelimbolo)
[![Email](https://img.shields.io/badge/-contact@dravelimbolo.com-111111?style=for-the-badge&logo=gmail&logoColor=white)](mailto:contact@dravelimbolo.com)

## Licence

MIT License
