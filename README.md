# BDD TDD

## Liens utiles
[Résumé de la conférence](https://conferences.elapsetech.com/tests-automatises-maintenir/)

[Références](http://references.elapsetech.com/)

[Conférence ATDD (à double boucle)](https://conferences.elapsetech.com/atdd-double-boucle/)

[Les tests unitaires... Et après ?](https://conferences.elapsetech.com/tests-unitaires-apres/)

## Qu'est-ce qu'un test ?
 - Valider un comportement attendu
 - Guider le développement
 - Tester
 - Valider une hypothèse
 - Techno
 - Affaires

| | Affaires | |
| --- | --- | --- |
Guider le développeur | | Le produit
| | Techno

 ### Quest-ce qu'un test unitaire ?
  - Ne teste pas les fonctionnalités
  - Teste le comportement d'une unité
    - Varie en fonction du paradigme
      - Programmation fonctionnelle: fonction
      - Programmation objet: Classe
        - Pas méthode car état interne qui fluctue, n'est pas isolée du reste des attributs

Une partie de la **dette technique** est expliquée par la complexité exponentielle du logiciel.
Le but de réduire la complexité est de limiter le cout du changement (refactoring).

#### Dette technique
Augmentation du cout du changement.

Les tets doivent permettre de limiter la dette technique. Peur du changement car + coûteux et peur de péter > les tests doivent réduire les risques de péter

```
Le pire ennemi du dévelopeur logiciel, c'est le développeur logiciel (et pas son voisin).
```

Le plus gros avantage des tests unitaires est de mettre une pression sur comment sont codées les classes, pour forcer à isoler / découpler au maximum les composants.

Fort couplage = rigidité du code = moins adaptable.

```
Les tests unitaires sont une source de rétroaction sur le design.
```

La documentation a plus de valeur que le TU. Elle permet d'écrire moins de tests. Le test empêche un bug de se produire, mais va-t-il servir dans le **futur** ?

Dans le futur, les tests risquent de planter, **ils vont identifier le problème.**

Les tests servent aussi à laisser une trace exécutable du comportement que l'on a voulu implémenter. Il sert donc dans un sens à documenter.

#### SOLID+T principles

S = single responsibility

```py
class FacturationClient
 def FacturerClient(client_id):
  client = clientRepo.findById(client_id)
  client.facturer()
  clientRepo.save(client)

class FacturationClientTest
  def siClientExisteQuandFacture_debiter():
    fauxClient = ...(12)
    client.save(fauxClient)

    useCase.facturer(12)
    verify(fauxClient).facturer().Times(1)
```

Principe du AAA: Arrange, Act, Assert

## TDD
Discipline par laquelle on va tenter de faire échouer un test avant de coder la fonctionnalité.

### 3 phases du TDD
1. Écrire un test qui échoue
2. Écrire le **minimum** pour faire passer le test
3. Refactoring

#### Exemple
Tests d'une pile. Premier test: On veut avoir une pile **vide**.

```java
public class MyStack {
  private boolean empty;

  public boolean isEmpty() {
    return this.empty;
  }
}

public class MyStackTest {
  @Test
  public void createStackThatShouldBeEmpty() {
    MyStack maStack = new MyStack();
    assertTrue(myStack.isEmpty());
  }

  @Test
  public void whenPushingStackShouldNotBeEmptyAnymore() {
    MyStack maStack = new MyStack();
    myStack.push(1);
    assertFalse(myStack.isEmpty());
  }
}
```

Une méthode ne devrait pas retourner un état et le modifier en même temps. C'est une bonne pratique (**CQS**) pour l'orienté objet.

Exemple: remove et return, ils ont 2 comportements indépendants.

Il faut limiter le nombre d'assertions dans un test pour connaitre exactement et plus rapidement la cause du problème.

```
Un bon test se lit comme une petite histoire.
Exemple: Il était une fois une pile qui devait être vide. On créa une pile, et elle fut vide.
```

## Behaviour Driven Developpement
**Le BDD n'est pas une technique de test.** 

### Discover workshop
Le BDD requiert **absolument** la présence du client. Présence obligatoire des 3 amigos. Le BDD est un **processus** qui ne fonctionne que si l'on fait de l'analyse *just in time*, et qui fonctionne de manière itérative et incrémentale.

L'objectif du BDD est d'avoir des conversations, comme *quelles sont les règles d'affaires ?*, *sommes-nous sûrs que nous avons compris la même chose ?*.

3 amigos = 3 profils psychologiques: Le premier est la réflexion à la logique d'affaires (analyse d'affaires). Le deuxième est le sens du détail (detail person, UI/UX). Le 3ème est le développeur, le profil technique, celui qui réfléchit à la faisabilité.

### Analyse des spécifications
#### Gerkhin
Écriture sous forme de texte de scénarios (Écrit dans un fichier .feature).

```feature
Scenario: Celui ou j'ai assez d'argent
  Given: Alice avec un compte c1 avec 300$
  And: Alice avec un compte c2 avec 0$
  When: Alice vers 50$ de c1 vers c2
  Then: Le compte c1 a un solde de 250$
  (And/But): Le compte c2 a un solde de 50$
```

Une fois qu'on a la spécification sous une forme connue, on automatise ces spécifications. On peut écrire des "glues", c.à.d des morceaux de code qui analysent le gerkhin (ou autre, avec une regex). La documentation devient alors **vivante**.

### Pyramide des tests

- Large (10%)
  - Tests end-to-end.
  - Devraient servir à trouver des problèmes **d'intégration** (exemple: mauvais découpage des services ou microservices) et non de **logique**.
- Medium (20%)
  - Permettre d'intégrer, de faire des appels à des services, appels http, bases de données, etc. Appels limités. Exemple: Un seul appel http, et pas de retour, pas d'appel à une base de donnée.
  - Tests en bordure de framework. Exemple: appel à une base de donnée mockée pour tester la requête SQL (base de données in-memory).
- Small (70%)
  - Pas de technologies (pas d'appel à un framework, à une librairie, etc.)
  - Permet de tester la logique.
  - Devraient être exécutés en millisecondes.

Pourcentages en terme de quantités. Notion de **fragilité** des tests.

#### Ice cream cone of death: quand la majorité des tests entrent dans la catégorie Large.

Système CRUD ou formulaire: Couplage entre les données et le UI. On veut le minimum de couches entre les 2 pour éviter un maximum de couplage.

Système riche: on veut un maximum de couches entre les données et le UI afin de sécuriser le coeur.

2 architectures différentes, 2 techniques de tests différentes.

## En résumé
```
If it ain't broke, don't fix it.
```

### Bonnes pratiques des tests unitaires:
- Nommage respectant le AAA
- Test court
- 1 seul comportement par test
- 1 seule raison d'échec (valable pour tous les autres tests)
- Rapide
- Pas de technos

CUT pour Class Under Test

Plus on descend dans la pyramide des tests, plus on augmente le retour sur investissement.

Le BDD est un outil pour amener la conversation, puis aux spécifications, puis à l'automatisation.

