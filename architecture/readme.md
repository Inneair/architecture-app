# <a name="architecture"></a>Architecture

## <a name="dependencies"></a>1. Dépendances
La gestion des dépendances est mise en oeuvre par l'utilisation de [Composer][composer].
Les dépendances du projet sont définies dans le fichier `composer.json`, situé à la racine du projet. Ce
fichier est placé sur GIT.

Dans la suite du document, `<project>` correspond au répertoire racine du projet sur le poste de développement.

### 1.1. Gestionnaire
La liste ci-dessous présente le rôle des propriétés de configuration présentes dans le fichier `composer.json` :

`name` : Identifiant de l'application pour le gestionnaire de dépendances et le référentiel de dépendances Packagist.
Ce nom respecte la syntaxe `<vendor_name>/<project_name>`.

`version` : Version de l'application au format `X.Y.Z`.

`type` : type de package.

`description` : courte description de l'application, sur une ligne.

`keywords` : mots-clefs associés à l'application.

`homepage` : site web de l'application.

`license` proprietary Type de licence pour l'utilisation de l'application.

`autoload.psr-0` : chemins permettant de trouver le code source de l'application pour la création automatique d'un
fichier d'auto-chargement PHP `vendor/autoload.php`. Ce fichier est créé lors de l'installation du projet.

`require` : liste des dépendances. Chaque dépendance est décrite en utilisant le format `"<name>": "<version>"`.

`require.php` : version PHP nécessaire pour exécuter l'application.

`require.ext-<name>` : version d'une extension PHP nécessaire pour exécuter l'application.

`require.lib-<name>` : version d'une librairie PHP nécessaire pour exécuter l'application.

`minimum-stability` : exigence de stabilité des dépendances élues par le gestionnaire.

### 1.2. Installation
- Extraire une copie de travail du référentiel GIT.
- Ouvrir un invite de commandes, se placer à la racine de la copie de travail, et exécuter la commande suivante pour
installer les dépendances :

        composer install

### 1.3. Mise à jour
Dans les faits, une bonne pratique est de mettre à jour les dépendances dès qu'une nouvelle version de l'application
doit être réalisée, lorsque des changements mineurs de version ont été réalisés dans celles-ci. Les changements opérés
dans les dépendances pouvant entraîner des régressions, cette mise à jour doit être réalisée avec prudence. Elle ne peut
être reportée dans le référentiel GIT que si l'ensemble des tests automatisés sont exécutés avec succès.

La procédure indiquée ci-après repose sur l'hypothèse qu'il existe déjà une copie de travail GIT du projet sur le poste
de développement.

#### 1.3.1. Mise à jour majeure
Une mise à jour majeure de dépendances est une mise à jour non anticipée dans le fichier `composer.json`. En général, ce
cas d'utilisation est rencontré lorsque l'équipe projet souhaite migrer une dépendance vers une version ayant fortement
évoluée (changement de numéro de version principal ou majeur).
- Modifier le fichier `composer.json`, et modifier les numéros de version des dépendances.
- Poursuivre avec la procédure décrite dans le paragraphe suivant. Si les tests automatisés sont exécutés avec succès,
reporter les modifications des fichiers `composer.json` et `composer.lock` dans le référentiel GIT.

#### 1.3.2. Mise à jour mineure
Une mise à jour mineure de dépendances est une mise à jour anticipée dans le fichier `composer.json`. mais qui n'a pas
encore été reportée dans le fichier `composer.lock`.
- Ouvrir un invite de commandes, se placer à la racine du projet, et exécuter la commande suivante pour mettre à jour
les dépendances dans la copie de travail :

        composer update

- Vérifier le bon fonctionnement du projet après mise à jour des dépendances en réexécutant les tests automatisés.
- Si les tests automatisés sont exécutés sans erreur, reporter les modifications du fichier `composer.lock` dans le
référentiel GIT.

## <a name="fwk"></a>2. Framework

### 2.1. Programmation orientée objet
L'utilisation de la programmation orientée objet est obligatoire. Le recours à la programmation procédurale est quasi
inexistante sur des applications de cette envergure, car elle ne permet pas de répondre de façon efficace aux objectifs
de l'entreprise et des clients. L'approche objet quant à elle fournit les avantages suivants, en plus des 3 points
fondamentaux que sont l'encapsulation, l'héritage, et le polymorphisme :
- représentation au plus juste du monde réel, par l'abstraction des types de données.
- modularité accrue.
- masquage des détails d'implémentation des classes.
- définition d'une interface claire sur chaque classe.
- réutilisation des classes dans d'autres applications.
- maintenabilité, fiabilisation, et flexibilité accrues.

L'ensemble de ces concepts permet :
- d'obtenir des gains de temps importants lors de la maintenance évolutive et corrective des programmes.
- d'effectuer des estimations de coûts de développement plus précises.
- d'harmoniser le code plus facilement dans tous les projets informatique d'une entreprise.
- de faciliter la mise en place de tests automatisés.
- de faciliter la réalisation de tests de charge, de benchmarks.
- de faciliter l'industrialisation de la production, par la mise en place d'une plate-forme d'intégration continue, la
mise en place d'outils de contrôle de la qualité, la mise en place d'outils de bug tracking, et leurs interactions.
- de faciliter la traçabilité des exigences et des règles métier dans le code.
- d'améliorer la lisibilité du code, en utilisant les termes du monde réel dans les noms de classes, d'objets, ...
- de bénéficier au mieux des assistants disponibles dans les IDEs.
- et bien d'autres gains encores.

### 2.2. Couches logicielles
L'application respectera les standards de l'architecture 3-tiers. Le code source est architecturé en couches :
- La couche de présentation : prend en charge les requêtes que l'utilisateur envoie depuis son navigateur, le contrôle
des données soumises, l'appel à des services métier le cas échéant, la mise en forme des résultats, la génération et
l'envoi des ressources demandées.
- La couche de la logique métier : contient la mise en œuvre des règles de gestion et des services que l'application
peut rendre.
- La couche d'accès aux données : fournit les services permettant d'accéder aux données du système. Ces données peuvent
être persistées dans des entrepôts de nature différente (base de données, système de fichier, etc). L'application
utilisera une couche interne ORM et/ou ODM, afin de réaliser les mises à jour vers les entrepôts de type base de
données.

### 2.3. Programmation orientée aspect
L'AOP est utilisée pour la gestion des transactions, et la journalisation technique automatique. Ce paradigme de
programmation permet de ne pas surcharger le code des services métier avec des instructions transversales qui ne
correspondent pas à l'implémentation de règles métier.

La gestion des transactions et la journalisation technique automatique sont implémentées dans des modules spécifiques,
les services métiers ne font plus d'appels directs à ces modules. Un greffon et un point de jonction permettent de
brancher ces opérations, le cas échéant, en amont et/ou en aval de chaque service.

L'utilisation de l'AOP apporte de nombreux avantages :
- Amélioration de la qualité du code : le code produit dans les services métiers est plus lisible, sa qualité en est
donc meilleure.
- Maintenabilité accrue : les modules techniques tels que ceux énoncés précédemment sont alors plus faciles à maintenir,
car séparés du reste du code métier.
- etc : se reporter à une documentation en ligne pour des compléments d'informations.

L'utilisation de PHP comme langage de programmation de la couche métier, fait que le tissage se fera de façon dynamique,
à l'exécution.

### 2.4. Framework applicatif
On retrouve beaucoup de fonctionnalités identiques dans de nombreux projets de développement, qui ont déjà été
implémentées et éprouvées, dans différents contextes. L'utilisation d'un framework du marché permettra de répondre
efficacement aux problématiques récurrentes qui seront abordées pendant le développement du produit, en intégrant des
composants tiers fiables et performants, développés avec des exigences de qualité fortes, et offrant un maximum de
flexibilité. Ces composants permettront de traiter plus facilement des sujets suivants :
- Initialisation de l'application
- Optimisation du chargement des objets
- Journalisation
- Accès aux données
- Authentification et autorisations
- Gestion des erreurs
- Et bien d'autres ...

### 2.5. Design patterns
Le code source doit s'appuyer sur des design patterns standards du monde informatique, afin de modéliser et implémenter
l'état et le comportement des objets dans l'application, et éviter de redévelopper les composants et les comportements
que l'on retrouve habituellement dans toute application. A ce titre, les design patterns ci-dessous seront utilisés
chaque fois que la situation le justifie :
- Catégorie "Instanciation" :
  - Singleton
  - Prototype
  - Factory
  - Builder
  - Object pool
- Catégorie "Structurel" :
  - Facade
  - Proxy
- Catégorie "Architecture" :
  - Front Controller
  - Service locator
  - Model View Controller
  - Data Transfer Object
- Catégorie "Comportemental" :
  - Iterator
  - Mediator
  - Visitor
  
Il existe de nombreux autres design patterns. Leur utilisation est elle aussi fortement préconisée en fonction de la
situation.

## <a name="components"></a>3. Composants Innéair
Les fonctionnalités de l'application sont regroupées dans différentes librairies et/ou bundles (terminologie issue de
Symfony).

### 3.1. Librairie Synapps
Cette librairie, hébergée sur GitHub, contient un ensemble de classes utilitaires générales, pour le développement
d'applications. Elle peut aussi contenir d'autres ressources, tels que des fichiers XML, des feuilles de style, des
scripts Javascript, ... qui apportent une aide générale pour le développement.
Cette librairie a vocation à rester totalement standalone, elle n'a aucune dépendance sur des librairies tierces, et
peut être utilisée sans pré-requis.

### 3.2. Bundle SynappsBundle
Ce bundle permet d'intégrer la librairie Inneair\Synapps dans le framework Symfony, et fournit des classes utilitaires
additionnelles pour le développement d'applications web avec ce framework (gestion des transactions, service et
contrôleur abstrait, repository d'accès aux données, sérialisation, ...).
Ce bundle possède donc plusieurs dépendances sur des composants tiers : Symfony, Doctrine, JMS Serializer, JMS AOP,
FOS REST, FOS User, ...

### 3.3. Librairie SynappsUI

__TBC__

### 3.4. Bundle OrigamiBundle
Ce bundle contient les classes propres à l'application Origami : modèles métier, services métier et 
d'accès aux données, contrôleurs web, ...

## <a name="code-layout"></a>4. Structure du code source
Le code source de l'application est basé essentiellement sur la structure initiale d'un projet Symfony 3. La liste
ci-dessous présente les principaux répertoires et leur contenu :

`app` : classes du noyau de Symfony.

`app/config` : Fichiers de configuration globaux de l'application.

`bin` : Utilitaires en ligne de commande.

`codeception.yml` : Configuration Codeception pour l'exécution des tests automatisés.

`composer.json` : Paramètres du gestionnaire de dépendances.

`composer.lock` : Paramètres du gestionnaire de dépendances sur la configuration en cours de test.

`data` : Répertoire de travail d'Origami, contenant les données qui ne sont pas stockées dans la base de données.

`data/test` : Répertoire contenant les données de test.

`data/views` : Répertoire contenant les vues HTML importées.

`README.md` : Notes de livraison : spécifications, installation, historique des versions.

`sonar-project.properties` : Configuration SonarQube pour l'analyse qualitative de l'application.

`src` : Code source d'Origami.

`var` : Bootstraps de chargement de l'application.

`var/cache` : Caches des environnements.

`var/logs` : Journaux de l'application.

`vendor` : Code source des dépendances (librairies, bundles Symfony).

`web` : Répertoire contenant les ressources statiques et le front controller.

`web/app.php` : Front controller de l'application (production, intégration, ...).

`web/app_dev.php` : Front controller de l'application (développement).

`web/config.php` : Script de configuration post-installation.

[composer]: <http://getcomposer.org> (Composer)
