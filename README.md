# Coding dojo Node.js de l'Elsass JUG, 9 juin 2015

### Etape 0 : Installation de Node.js ![Easy](icons/dojo-difficulty-easy.png)

- Pour les utilisateurs de windows, vous pouvez passer directement par l'installeur classique. En bonus ça serait mieux d'avoir une ligne de commande user friendly, par exemple cygwin.
- Pour les autres, je vous préconise d'installer Node Version Manager qui vous permettra de facilement changer de version de Node.js, mais vous pouvez aussi passer par l'installeur classique.

Pour valider votre installation, vous pouvez lancer les commandes suivantes :

```
node --version
npm --version
npm install -g grunt-cli
npm install -g bower
grunt --version
```

Nous réutiliserons les commandes node, npm, grunt et bower dans les étapes suivantes.

### Etape 1 : Hello world ![Easy](icons/dojo-difficulty-easy.png)

En utilisant votre éditeur préféré (par exemple SublimeText), créez un fichier hello.js qui affiche "hello world" dans la console puis exécutez ce fichier avec la commande node.

Félicitations, vous venez d'exécuter du code Node.js ;-)

### Etape 2 : Votre premier projet ![Easy](icons/dojo-difficulty-easy.png)

Lancez la commande "npm init" et suivez les différentes étapes de création du projet.

Vous venez de créer votre premier projet Node.js. Jetez un coup d'oeil au fichier package.json qui vient d'être généré. Ce fichier permet de décrire votre projet et ses différentes dépendances.

### Etape 3 : Votre premier serveur HTTP ![Intermediate](icons/dojo-difficulty-intermediate.png)

[Express](https://github.com/strongloop/express) est un module qui permet de créer un serveur web en Node.js. C'est une surcouche au module http qui est compris de base dans Node.js.

En une seule commande npm, installez ce module dans votre projet et faites en sorte que le module soit automatiquement référencé dans le fichier package.json.
Jetez à présent un coup d'oeil au contenu de votre projet, un nouveau répertoire vient d'être créé. Jetez y un coup d'oeil.

Créez un fichier server.js à la racine de votre projet et utilisez le module Express. Votre objectif : faire en sorte que votre serveur renvoie une liste d'animaux sur l'url /pets :

```
$ curl http://localhost:3000/pets
[{"name":"Heidi","kind":"Dog","age":3},{"name":"Pluto","kind":"Dog","age":14},{"name":"Heidi","kind":"Dog","age":4}]
```

Pour tester votre serveur, vous pouvez aussi utiliser un client graphique, par exemple le plugin Postman pour Chrome / Firefox.

### Etape 4 : Un peu d'outillage ![Easy](icons/dojo-difficulty-easy.png)

Fatigué de redémarrer votre serveur à chaque modification du code ? Créez un fichier gruntfile.js à la racine de votre projet avec le contenu suivant :

```
module.exports = function(grunt) {
  grunt.initConfig({
    nodemon: {
      dev: {
        script: 'server.js'
      }
    }
  });

  grunt.loadNpmTasks("grunt-nodemon");

  grunt.registerTask("default", ["nodemon:dev"]);
};
```

Installez le packet grunt en local en exécutant dans votre racine de projet:

```
npm install grunt --save
npm install grunt-nodemon --save
```

Désormais, vous pouvez lancer la commande grunt pour démarrer votre serveur et toute modification du code sera prise immédiatement en compte.

Une alternative à grunt est gulp. Grunt ou Gulp permettent de gérer toutes les tâches autour de votre projet : exécution des tests, build, minification, gestion des feuilles de style etc.

### Etape 5 : Requête HTTP Post ![Intermediate](icons/dojo-difficulty-intermediate.png)

Pour l'instant, on a géré une requête de type GET dans Express. Désormais vous allez gérer un POST. 

Faites en sorte de pouvoir ajouter un animal de la manière suivante :

```
$ curl -H "Content-Type: application/json" -X POST http://localhost:3000/pet -d '{"name": "Rockie", "age": "4"}'
```

Pour l'instant on se contentera d'afficher le contenu de la requête dans la console du serveur. La difficulté ici est de parser le body de la requête. Pour faire cela, vous allez devoir installer un autre module...

### Etape 6 : Intégration d'un backend ![Intermediate](icons/dojo-difficulty-intermediate.png)

Firebase est une base de données NoSQL dans le cloud. Les données y sont stockées en JSON. Elle est particulièrement appréciée pour la synchronisation des données en temps réel sur chacun des clients et pour son hébergement de Single Page Application.

Ici, nous allons utiliser la partie stockage des données, comme une alternative à une base de données MongoDB par exemple.

Pour commencer, créez-vous un compte sur Firebase. C'est gratuit et vous ne le regretterez pas!

Ensuite, installez le module [Node.js](https://www.firebase.com/docs/web/quickstart.html) dans le projet.

Utilisez à présent les fonctions d'écriture et de lecture du module Firebase pour créer et lister des animaux, en remplacant le code des requêtes GET /pets et POST /pet.

Testez ensuite vos deux requêtes HTTP et visualisez vos données sur votre compte Firebase.

### Etape 7 : Un peu d'HTML ![Intermediate](icons/dojo-difficulty-intermediate.png)

[Swig](https://github.com/paularmstrong/swig) est un module permettant de faire du templating HTML en Node.js.

A partir de la [documentation](http://paularmstrong.github.io/swig/docs/#express), intégrez Swig à Express. Attention à bien désactiver le cache pour éviter de redémarer votre serveur à chaque fois que vous modifierez votre template HTML.

A présent, créer un fichier index.html dans un répertoire views et affichez y un hello world. Puis faites en sorte que l'url / affiche votre fichier index.html.

Enfin, récupérez dans Firebase la liste des animaux, passez cette liste dans le contexte swig, et affichez les noms des animaux dans la page index.html.

### Etape 8 : MEAN.js ![Intermediate](icons/dojo-difficulty-intermediate.png)

MEAN ou Mongo Express Angular Node.js est une stack très populaire permettant de créer une application web complète en se basant sur des outils qui manipulent du JSON du browser à la base de données.

Le projet [MEAN.js](https://github.com/meanjs/mean) propose de créer un projet complet avec tous les outils nécessaires pour démarrer les développements ainsi qu'une bonne base de code respectant les bonnes pratiques de développement de Node.js et d'Angular, notamment en ce qui concerne la testabilité.

En vous basant sur la [documentation de Mean.js](http://meanjs.org/generator.html), générez un projet MEAN.js. Attention, le projet sera généré directement dans le répertoire courant. Il est donc préférable de lancer les commandes de génération dans un répertoire vide.

Installez à présent une base [MongoDB](https://www.mongodb.org/downloads) en local.

Puis lancez la commande grunt, et rendez vous à l'url http://localhost:3000/#!/signup.

Félicitations, vous avez bootstrappé un projet MEAN.js. Vous pouvez à présent jeter un coup d'oeil au code et essayer de le modifier.

### Etape 9 : Refactoring ![Intermediate](icons/dojo-difficulty-intermediate.png)

Dans le projet créé en Etape 2, vous avez peut-être écrit tout le code directement dans server.js. Inspirez vous de la structure du projet MEAN.js pour créer un découpage propre entre chacune des couches du projet : route / controller / service.

### Etape 10 : Les promesses ![Hard](icons/dojo-difficulty-hard.png)

La gestion de l'ansynchrone en Node.js n'est pas habituelle pour un développeur qui fait peu de Javascript. Les promesses permettent de faciliter cette gestion. Essayer d'utiliser le module Q ou le module bluebird pour comprendre comment ça fonctionne.
