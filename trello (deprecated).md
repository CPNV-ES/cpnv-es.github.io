# Organisation des projets sur Trello

Trello est un outil de gestion de projet collaboratif en ligne permettant à ses utilisateurs d’organiser leur projet selon la méthode Kanban. Cet outil est disponible sur le web et en version mobile dans les différents app stores.

Ce document présente les bonnes pratiques utilisées dans les classes de techniciens "Développement d'application".

Pour une utilisation optimale de Trello avec Github une intégration du [Power-up Github est conseillée](http://help.trello.com/article/1065-using-the-github-power-up).

Un _board_ est créé pour chaque projet. L'organisation des listes à l'intérieur d'un _board_ est la suivante:

  * **Projet info**: partie administrative du projet, doit contenir les cartes suivantes:
    * **Titre**: Titre et description du projet
    * **Team**: description des noms de l'équipe et du chef de projet (ATTENTION: merci de garder l'historique)
    * **Journal de Bord**: historique (sous la forme d'une _Activity_) "haut niveau" de la gestion du projet (il ne s'agit pas là du journal de bord individuel des membres de l'équipe)
    * **Documentation**: liens vers les différents répertoires contenant la documentation (y comp. les conventions de codage mises en place).
    * ... autres cartes que l'équipe de projet estimera nécessaire.
  * **Customer requests**: Espace réservé au client pour formuler et prioritiser ses demandes auprès de l'équipe de développement
  * **Product backlog**: Ensemble de cartes (ou _user stories_) décomposant les requêtes du client. Un requête du client peut être décomposée en plusieurs cartes dans le **product backlog**
  * **Sprint backlog**: Ensemble des cartes/fonctionnalités qui seront développés
  * **In progress**: Liste des cartes en cours de développement. Chaque carte est liée à la branche correspondante.
  * **Review**: Liste des cartes à _Review_
  * **Tests**: Liste des cartes à tester. Les critères d'acceptances sont définis dans la carte.
  * **Done**: Fonctionnalités implémentées et testées. A la fin du _Sprint_ cette liste sera archivée avec la date de la démonstration.

## Process

  * Le "client" introduit ses requêtes sous la forme d'une carte dans la liste **Customer requests**. Cet ajout peut se faire à n'importe quel moment.
  * Lors d'une réunion avec le client (équivalent du _Backlog grooming_ ou _levée de périmètre_) l'équipe de développement traduit les requêtes du client en _user stories_. A cette étape une décomposition d'une requête du client en plusieurs _user stories_ est possible.
  * Une _User story_ respecte le format *<Identifiant> <Titre> (effort); description sous le format "En tant que <rôle>, je veux <quelque chose> afin de <atteindre un but>"; critères d'acceptation (sous la forme d'une checklist)*.
  * Lors de la réunion avec le client l'équipe définit quelles seront les cartes à implémenter et donc à transférer du _Product Backlog_ au _Sprint Backlog_. A cette occasion l'équipe définit la durée du _sprint_ (idéalement deux semaines mais peut-être variable selon la charge de l'équipe) et donc la date de la prochaine démonstration avec le client.
  * Pendant le _sprint_ une carte est transférée de la liste _Sprint Backlog_ à la liste _In progress_. Sur Git une nouvelle branche est créee, et la carte est alors liée à la branche grâce au Power-Up Github.
  * Quand la fonctionnalité est implémentée, la carte est transférée dans _Review_ pour qu'un autre membre de l'équipe puisse revoir le code. En aucun cas le reviewer ne modifie le code mais communique avec la personne qui a codé la fonctionnalité. Le codeur intègre les remarques et les communique dans le _commit_
  * La _Review_ terminée, la carte est passée dans _Tests_ pour être confrontée aux différents critères d'acceptances. Lorsque les tests ont été effectués et validés le code est mergé avec la branche _develop_ et la carte est liée au _merge_ en question.

## Ressources utiles

  * [Agile coach: Kanban](https://fr.atlassian.com/agile/kanban)
  * [3 Differences Between Scrum and Kanban You Need to Know](https://www.cprime.com/2015/02/3-differences-between-scrum-and-kanban-you-need-to-know/)
  * [Trello et la méthode Kanban](https://www.supinfo.com/articles/single/2245-trello-methode-kanban)
  * [How Do YOU Kanban? A Simple Guide to Using Kanban with Trello](https://blog.hubstaff.com/kanban-with-trello/)
  * [Scrum avec Trello](https://blog.hadrien.eu/2014/01/31/scrum-avec-trello/)
