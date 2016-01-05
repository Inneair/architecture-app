# <a name="test"></a>Stratégie de test

## <a name="dependencies"></a>1. Présentation
Les composants de la couche métier, développés en PHP, font l'objet de tests automatisés. Optionnellement, les
composants Javascript  développés pourront faire l'objet également de tests automatisés, bien que ce ne soit pas un
objectif majeur. Chaque version livrée du produit fera l'objet d'une campagne de recette usine (FAT), afin de vérifier
que l'application répond aux exigences fixées par la maîtrise d'œuvre en début de projet.

Le framework de test Codeception, intégrant PhpUnit, a été retenu pour le développement et l'exécution de tests
automatisés.

## <a name="cases"></a>2. Cas de test

### <a name="unit"></a>2.1. Tests unitaires
__TBC__

### <a name="integration"></a>2.2. Tests d'intégration
Le framework Symfony fournit des composants afin de tester la chaîne complète client-Serveur d'une application. Il est
ainsi possible de simuler des requêtes HTTP d'un client sur un chemin donné, et de contrôler la réponse renvoyée par le
serveur. Ce fonctionnement permet de traverser toutes les couches de l'application, et de contrôler l'intégration de
chaque composant applicatif dans le produit.

### <a name="acceptance"></a>2.3. Tests de recette
__TBC__
