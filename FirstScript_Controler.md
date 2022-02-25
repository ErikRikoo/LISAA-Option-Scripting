[< Accueil](README.md)

# Notre premier Script: Un contrôleur
**Commençons par le mouvement !**

Prenons notre problême et cassons le en de plus petits problèmes, on souhaite:
- A chaque image/frame
    - Regarder quel est le déplacement demandé par le joueur
    - Calculer quelque chose en fonction de ce déplacement, la vitesse et... le deta time bien sûr.
    - Déplacer le personnage du mouvement correspondant.

Mantenant que nous avons détailler le problème, nous pouvons le traduire en C#.

## Input du joueur
Pour l'input du Joueur, cela ressemble beaucoup à Bolt. La plupart des noeuds
de Bolt se retrouve en C#. Donc pour récupérer les inputs,
on peut utiliser `Input.GetAxis`. Vous pourrez retrouver la documentation
[ici](https://docs.unity3d.com/ScriptReference/Input.GetAxis.html).

**Remarque:** Il est parfaitement possible d'utiliser directement le résultat.
En effet, `Input.GetAxis` retourne un _float_ et donc utilisable dans un Vector par exemple.
Mais pour gagner en lisibilité nous allons utiliser une variable locale.

## Variables locales
Les variables locales sont un petit peu comme les variables de type _Graph_ dans Bolt.
Elles vont nous permettre de stocker une valeur, le résultat d'un calcul et la réutiliser plus tard.
### Création des variables locales:
```csharp
  int monEntier; // Ici on déclare un entier (int) qui s'appelle monEntier;
  
  int monDeuxiemeEntier = 42; // On peut donner une valeur au moment de la déclaration

  int monEntier = 2; // Ici, on redéclare une variable, c'est interdit et vous aurez une erreur
```
Vous pouvez noter ici que pour créer une variables locales, vous avez plusieurs élements:
- Le type, comme par exemple:
    - int qui est un entier (0, 1, -54, etc.)
    - float qui est un nombre réel, un nombre à virgule (0.15, 65.45, etc.)
    - bool qui vaut soit _false_ soit _true_.
    - etc.
- Le nom de la variable, ce qui permet de l'identifier et l'utiliser
- **Optionnellement**, il est possible de l'initialiser en faisant `= ...`.
    - La valeur à assigner peut être une constante (5),
      ou le résultat d'un calcul(`5 * uneAutreVariable`).
- Vous noterez à la fin la présence d'un point virgule qui est **très important**, sans
  vous aurez une erreur. Il est nécessaire pour indiquer la fin de l'instruction.


### Utilisation d'une variable après la déclaration:
```csharp
monEntier = 47; // Ici, on change la valeur de 'monEntier' pour 47

Debug.Log(monDeuxiemeEntier); // Ici on affiche monDeuxièmeEntier dans la console
```
Comme vous pouvez le voir il est possible de changer la valeur d'une variable,
ce qui serait équivalent à un set variable dans Bolt. Mais vous pouvez aussi l'utiliser,
ce qui est similaire au Get Variable de Bolt.

**Attention:** Il est important de faire attention au nom des variables, tout n'est pas
possible. Voici quelques régles:
- Pas d'accent
- La variable doit commencer par une lettre
- Elle peut contenir des chiffres
- Pas d'espace
    - Pour noter un espace, nous utilisons en général le lowerCamelCase pour les variables locales.
      Les mots son écrits en minuscules sauf la lettre qui est normalement précédé par un espace.
      Par exemple:
        - `mon entier` devient `monEntier`.

<details>
 <summary> Utilisation de Debug.Log </summary>
Vous pouvez noter la présence de Debug.Log dans la section de code précédente. Cette fonction
permet d'afficher dans la console ce qu'il y a entre parenthèse, ce qui peut être utile pour débugger.
Vous pouvez:
- Afficher la valeur d'une variable
- Afficher un message: `Debug.Log("Je suis le message);"`
- Afficher un message plus une variable: `Debug.Log("Mon entier vaut : " + monEntier);`
</details>

<details>
 <summary> Remarques </summary>
- Le code ci-dessus doit se trouver dans le corps d'une fonction pour fonctionner, c'est pour ne pas alourdir la lecture.
- Quand on utilise `\\ Du texte ...`, c'est ce qu'on appelle un commentaire. Ce texte
sera ignoré par le compilateur. Cela peut être utile pour rajouter de la sémantique,
mais cela ne devrait jamais remplacer un nom de fonction ou de variable claire.
</details>

## Création du Vecteur déplacement
Pour créer un vecteur en 3 dimensions, c'est très simple:
```
Vector3 deplacement = new Vector3(0, 5, 0);
```
Comme vous pouvez le voir ici nous:
- Déclarons une variable locale de type Vector3 (un vecteur en 3 dimensions)
- Son nom est _deplacement_
- On lui assigne une valeur qui est un vecteur en 3 dimensions avec 0 en x, 5 en y, 0 en z.
  Il est totalement possible d'utiliser des variables ici.
    - Noter la présence du new qui est nécessaire, il indique que l'on crée un Vector3.

Ensuite, il est possible d'appliquer les opération habituelles:
```
new Vector3(0, 1, 0) * 5 // Nous donne le résultat de la multiplication
new Vector3(0, 1, 0) + new Vector3(0, 1, 0) // Nous donne le résultat de l'addition
etc.
```
Vous avez en opérations:
- `+` permet d'additionner deux vecteurs entre eux
- `-` va soustraire deux vecteurs
- `*` va multiplier le vecteur avec un float
- `/` va faire la division entre le vecteur et un float

**Attention:** Si vous souhaitez utiliser un nombre à virgule, il vous faudra le suffixé avec f.
Comme ceci: `0.5f`.


## Déplacement du joueur
Pour déplacer un objet, vous pouvez comme dans Bolt utiliser le Translate.
Vous pouvez essayer d'utiliser Translate directement comme dans Bolt et vous aurez une
erreur. C'est normal, Bolt vous simplifiait la vie et récupérer le Transform pour vous.
Comment faire? Le transform est accessible depuis le MonoBehaviour et vous savez quoi? Notre script en est un.
On peut donc faire:
```
transform.Translate(monVector3);
```
Vous pouvez noter ici que:
- On récupère le Transform grâce à la variable `transform`.
- Puis que nous lui disons de se déplacer grâce à la fonction Translate.

## Rendons la vitesse configurable
Si vous souhaiter modifier la vitesse actuellement, vous pouvez multiplier le déplacement
par une constante. Ceci n'est pas très pratique car vous devez retourner dans le script
pour faire une modification, ce qui est en plus pas très _designer-friendly_.

Afin de parer cela nous allons utiliser les variables membres.
### Variables membres
Nous avons vu il y a peu les variables locales qu'il fallait déclarer dans le corps d'une fonction.
Mais il est en faite tout à fait possible de déclarer une variable dans la classe directement.
Ceci peut avoir plusieurs utilités:
- Stocker quelque chose qu'on veut réutiliser plus tard
    - Une vitesse aléatoire par exemple
- Sauvegarder quelque chose entre plusieurs frames
    - Pour lisser un déplacement par exemple
- Afficher quelque chose dans l'Inspector

Pour déclarer une variable membre, on fait comme suit:
```csharp
public class MonScript {
  ...
  private int unEntierAGarder; // Une variable que l'on va réutiliser 
  
  [SerializeField]
  private float maVitesseAAfficher; // Ici une variable qui sera afficher dans l'inspector
  ...
```
Vous pouvez noter plusieurs choses:
- On déclare nos variables dans la classe et pas dans une fonction, elles appartiennent
  donc à notre objet.
- Nous ajoutons le mot clé **private** en plus qui est la visibilité. Ici, personne
  à part notre script ne pourra accéder à notre variable.
- Pour qu'une variable apparaisse dans l'Inspector, nous la préfixons de `[SerializeField]`.

Ces variables peuvent ensuite être modifié, lues etc. comme une variable locale.

<details>
 <summary> Pourquoi utiliser _private_? </summary>
Ici, nous utilisons _private_ afin de rendre notre variable "inaccessible", 
ce qui nous force notamment à rajouter `[SerializeField]` pour l'afficher.
Or, il existe une autre accessibilité: **public** qui nous permet d'afficher directement 
la variable dans l'Inspector et tout le monde peut la modifier.

Si on n'utilise pas _public_, c'est tout simplement pour respecter l'encapsulation.
C'est une bonne pratique de la programmation objet, ou chaque classe est comme
une boîte noire. On peut lui dire d'éxécuter des actions mais on ne sais pas comment
on le fait.

L'autre raison est qu'on ajoute une sémantique au code pour d'autres développeurs:
"Il ne faut pas modifier cette variable".
</details>


Vous pouvez maintenant rajouter une variable membre pour rendre la vitesse configurable.

### Pour aller plus loin
Ici, vous pouvez modifier le contrôleur afin de faire en sorte que:
- L'axe vertical déplace le joueur vers l'avant ou l'arrière
- L'axe horizontal fasse tourner le joueur
    - Vous pourrez vous aider des méthodes présentes sur le Transform

---
**Passons au saut!**
Maintenant que notre personnage peut se déplacer, il est temps de lui permettre
de sauter comme tout bon contrôleur qui se respecte.

Encore une fois détaillons le problème, nous souhaitons:
- A chaque image
    - Vérifier que le joueur a appuyé sur une touche et si c'est le cas:
        - Calculer la force (une direction et une intensité)
        - Appliquer la force sur l'objet

## Vérifions que le joueur a appuyé sur une touche
Afin de vérifier ça nous pouvons utiliser le `Input.GetKeyDown` dont
vous pouvez retrouver la documentation [ici](https://docs.unity3d.com/ScriptReference/Input.GetKeyDown.html).
Nous utiliserons la seconde, celle qui prend un _Keycode_. Pour la barre espace, ça
sera tout simplement `Keycode.Space`.

Cette fonction va nous retourner un booléen i.e. vrai ou faux.

## Exécution de code si la touche est pressée
Pour éxécuter le code en fonction d'un booléen, nous allons utiliser le `if else`.
Celui est l'équivalent du node Branch dans Bolt. Il s'utilise comme suit:
```
if(booleen) {
  // Code à éxécuter si booleen est vrai
} else {
  // Code à éxécuter si booleen est faux
}
```
La structure est très simple et se rapproche beaucoup du Branch. Vous avez:
- Le `if(booleen)` qui permet de tester le booléen
- Le code à éxécuter si notre booléen est vrai (dans notre exemple le saut)
- Le mot clé `else` qui permet de dire que nous allons éxécuter quelque chose si c'est faux
- En dessous, vous avez le code que vous souhaitez éxécuter entre les accolades

**Remarque:** Si vous désirez éxécuter que le code si c'est vrai, vous pouvez omettre le else et les accolades en dessous.

## Calcul de la force
Afin de donner une force vers le haut, il nous faut la calculer. Encore une fois,
nous pouvons avoir une variable qui nous permettra de donner l'intensité (même méthode
que pour la vitesse). Mais il nous faut le vecteur qui va vers le haut, nommé **up**.
Pour cela, il existe deux méthodes:
- Utiliser `Vector3.up` qui vaut (0, 1, 0)
- Utiliser `transform.up`

La différence entre les deux est que le premier est le _up_ du monde, alors que
le second est le _up_ de l'objet, il prend donc en compte sa rotation.

Enfin, pour changer l'intensité de la force, il suffit de multiplier par la variable
précédemment évoquée.

## Donner l'impulsion à notre joueur
Afin de donner l'impulsion à notre joueur, nous pouvons utiliser le AddForce.
Nous allons avoir le même problème que le Translate du Transform ici. Bolt nous simplifiais
grandement la vie en le récupérant pour nous.

Si vous vous souvenez, nous avons pu récupérer très facilement le Transform en faisant `transform`.
Si nous avons pu le faire, c'est car il y a forcément un Transform sur un objet de la scène,
donc Unity l'a rendu accessible. Mais ce n'est pas le cas du RigidBody.

Quand vous souhaitez récupérer le Component sur un objet, il faut utiliser la fonction
`GetComponent`. Celle-ci va regarder la liste des component et chercher le Component
demandé. S'il existe, il nous le retournera.

Elle s'utilise comme suit (ici pour un Rigidbody):
```
Rigidbody rb = GetComponent<Rigidbody>();
```
Ici, nous:
- Déclarons une variable de type Rigidbody (le component souhaité) de nom rb
- Appelons la fonction GetComponent qui ne prend aucun paramètre, il n'y a donc rien
  entre les parenthèses.
    - Par contre, il y `<RigidBody>`, cela nous permet d'indiquer quel type de Component nous
      voulons.

Une fois notre Rigidbody récupéré, nous pouvons tout simplement appeler la fonction [AddForce](https://docs.unity3d.com/ScriptReference/Rigidbody.AddForce.html).

## Interdire le saut multiples au joueur
Si vous testez en l'état, vous verrez que notre joueur peut sauter à l'infini.
Ce qui n'est pas très intéressant. Nous allons maintenant bloquer le saut,
si nous avons déjà sauté.

Commencez par rajouter une variable membre nous disant si nous sommes en l'air ou non.

### Détecter la collision avec le sol
Afin de détecter la collision avec le sol nous allons pouvoir utiliser la fonction
OnCollisionEnter qui s'apparente à celui utilisé dans Bolt. Afin de dire à Unity
que nous souhaitons écouter cet événement, il faut le rajouter dans notre classe
comme le Update. Pour cela, rajoutez:
```
private void OnCollisionEnter(Collision other) {
  // Le code à éxécuter
}
```
Vous pouvez voir qu'ici nous avons un paramètre: `Collision other`. Celui-ci nous permet
d'accéder à toutes les informations sur l'objet qui vient de rentrer en collision avec nous.
Cela nous permet par exemple, de vérifier qu'il a le **tag** Ground. Pour cela,
le Collision fournit la méthode CompareTag (oui comme dans Bolt).

Faites donc en sorte de modifier la variable que vous avez créé pour indiquer que nous avons fini d'être en l'air.


### Empêcher le saut dans le Update
Nous allons maintenant modifier notre Update pour permettre le saut que si nous sommes sur le sol.
Nous allons modifier notre conditionnelle et regarder:
- Est-ce que j'ai appuyé sur la barre espace
- ET est-ce que je suis sur le sol

Pour faire un ET en C#, vous pouvez tout simplement faire `booleen1 && booleen2`.
Maintenant, notre saut n'est permis que si nous sommes sur le sol.

Faites bien attention à dire que vous n'êtes plus sur le sol au moment de sauter.

<details>
 <summary> Opérateurs booléens </summary>
Ici nous avons utiliser le ET, mais il existe d'autres opérateurs booléens, en voici la liste:
- `&&` permet de faire un ET entre les deux booléens i.e. ça retourne vrai si les deux sont vrais
- `||` permet de faire un OU inclusif qui retourne vrai si l'un des deux l'est
- `!` faire la négation d'un booléen
- `^` fait un OU exclusif i.e. seulement l'un des deux doit être vrai
</details>

## Améliorons le code
Actuellement, notre code est fonctionnelle mais nous pouvons en améliorer sa qualité.
Nous allons donc faire une phase de refactor (ou réusinage en français). Cela
consiste à modifier le code pour améliorer sa qualité tout en gardant les même fonctionnalités.

Ici par qualité, j'entends des performance et de la lisibilité.

### Stockage du Rigidbody dans le Awake
Si vous souvenez, nous récupérons notre RigidBody dans le Update, ce qui risque de s'éxécuter assez
souvent. Or le GetComponent est assez gourmand. Je vous rappelle qu'il doit parcourir
tous les Component de votre objet et vous retourner le bon s'il est présent.

Afin de gagner en efficacité, nous allons le faire une seule fois dans l'événement
qui s'appelle Awake (celui-ci est comme le Start, sauf qu'il s'éxécute avant), puis le
stocker dans une variable membre.

### Utilisons les fonctions
Si vous regarder votre Update, actuellement vous avez à la fois le code du mouvement
et du saut. Vous avez peut-être déjà sauté un espace entre les deux, et peut-être
rajouter un commentaire. Nous allons aller plus loin en séparant ses codes dans des
fonctions différentes.

Pour déclarer une fonction, on fait comme ceci:
```
private void NomDeLaFonction() {
  // Code de la fonction
}

// Vous pouvez ensuite l'appeler dans une fonction comme ceci:
NomDeLaFonction();
```
La déclaration d'une fonction (membre) contient:
- L'accessibilité, encore une fois, on le met en `private` car il n'y a que nous
  qui devons y accéder.
- Le type de retour qui vaut void ici i.e. on ne retourne rien (nous verrons ça en détail)
  plus tard.
- Le nom de la fonction
- La liste de paramètres entre parenthèse qui est vide
- Le corps de la fonction entre accolades

Nous allons donc ici créer deux fonctions avec le code correspondant:
- Movement ou Deplacement pour les déplacement
- Jump ou Saut pour le saut

Nous pourrons ensuite les appeler dans le Update et nous voilà avec un code
plus clair. En un coup d'oeil, on comprend qu'on se déplace et qu'on saute
dans le Update.


### Pour aller plus loin
1. Rendre configurable la touche

Vous pouvez rendre configurable la touche assez facilement en créant une variable
membre, modifiable depuis l'inspector. Je rappelle que le type est Keycode.

2. Permettre le double saut (ou plus)

Vous ferez en sorte de permettre au joueur de sauter plusieurs fois, ça devra être
configurable dans l'Inspector. Vous pourrez pour cela compter le nombre de saut effectué.

3. Faire en sorte "d'attérir" sur autre chose que le sol

Actuellement, nous ne pouvons attérir que sur des objets ayant le tag "Ground".
Ce n'est pas pratique car si nous attérissons sur un objet qui n'est pas le sol,
nous ne pourrons pas resauter.

En réalité, nous pouvons utiliser une autre technique pour savoir si on attérit sur "le sol".
On peut tout simplement vérifier qu'on attérit sur une surface plane. Pour cela, vous
pouvez:
- Accéder aux points de contacts (grâce à la variable `contacts`) sur le collider.
- Et de vérifier que l'un deux a sa normale orienté vers le haut. Pour cela, vous
  pouvez utiliser la fonction Any sur la liste de points de contacts.

[< Accueil](README.md)