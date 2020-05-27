# Recettes locales

Comme on peut le lire dans le Scrum Guide, scrum (mais plus généralement les méthodes agiles) est simple à comprendre mais difficile à maîtriser.

Cela provient du fait que la simplicité des règles laisse de la liberté aux membres de l'équipe et que cela prend du temps pour que chacun comprenne comment jouir de sa liberté d'une manière qui soit profitable à tout l'équipe.

Ce document décrit donc de manière un peu plus précise comment nous faisons certaines choses dans notre filière.

## Sandbox, product backlog et sprint backlog

En plus du Product Owner, tout membre de l'équipe peut en tout temps proposer des idées sous forme de [user | technical] story qu'il dépose dans un espace identifié comme "sandbox" (bac à sable)

Le Product Owner doit trier les stories de la sandbox soit en les éliminant, soit en les acceptant dans l'espace identifié comme 'product backlog'. Il veille à maintenir dans le product backlog un nombre de stories tel que l'équipe de développement aie du choix, mais pas trop (!).

Lors du sprint planning, l'équipe ne place dans le sprint backlog que

- Des stories du product backlog
- Des tâches purement internes à l'équipe de développement (bug fix, refactorisation, documentation, ...)

L'équipe veille à l'équilibre entre ces deux types d'activités: garder suffisamment de stories pour que le sprint apporte de la valeur visible au produit, mais sans repousser les tâches internes qui devront bien se faire un jour.

## Analyse

La description de toute [user | technical] story contient:

1. Une description du contexte actuel dans lequel la story intervient
2. Une description du problème (ou du manque) auquel on est confronté
3. Une description de la solution proposée (en termes non-techniques)

Les activités de développement sur une story ne démarrent pas sans que le Product Owner soit d'accord avec l'analyse

## Definition of Done

La définition de 'travail terminé' se fait au moyen d'une liste de tests.

Chaque test doit être énoncé de manière SMAR (comme dans SMART, mais sans le T qui est donné par le sprint).

La liste des tests ne se limite pas forcément à l'aspect logiciel. On y mettra fréquemment un test du genre 'La branche de feature a été mergée dans develop'

Les activités de développement sur une story ne démarrent pas sans que le Product Owner soit d'accord avec la DoD

## Plan d'intervention

Avant de se lancer dans l'écriture du code, le développeur découpe le travail à faire en tâches bien définies. L'ensemble de ces tâches décrit les moyens techniques que le développeur entend utiliser pour réaliser la solution énoncée durant l'analyse.

Les activités de codage ne démarrent pas sans avoir effectué une revue du plan d'intervention avec (au moins) un pair.

