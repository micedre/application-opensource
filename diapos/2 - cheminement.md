# Cheminement

----

## Création d'un projet avec connexion à une BDD

- création d'une API Java avec Spring Boot
- connexion à une BDD Postgre en local
- possibilité de surcharger les properties au démarrage de l'application pour utiliser une BDD au CEI

----

## Ajout d'une couche d'authentification

- utilisation de Keycloak pour gérer l'authentification et les autorisations
- possibilité d'utiliser un Keycloak en local et de surcharger les properties au démarrage de l'application pour utiliser le Keycloak de l'Insee

----

## Utilisation d'un service propriétaire
   
utilisation du service propriétaire de gestion des contacts
   - pas de possibilité de l'utiliser à l'extérieur de l'Insee
   - création d'une interface et de deux implémentations
       - une par défaut qui simule l'utilisation d'un service externe avec la gestion en mémoire d'une liste de contacts
       - une autre qui utilise le service propriétaire

----

## Bilan d'étape

- avec un projet organisé de cette façon, il est possible d'utiliser l'application en externe mais ça reste un peu compliqué car il faut démarrer un Postgre et un Keycloak sur son poste pour que l'appli fonctionne
- le code n'est pas totalement propre car l'appel à des services internes y est présent

----

## Réorganisation du code

découpage en 2 projets :
- *api-opensource* avec tout le code sauf le code spécifique Insee
- *api-insee* avec *api-opensource* comme dépendance + le code spécifique Insee

----

## Simplification du démarrage du projet opensource

- remplacement de PostgreSQL par base H2
    - création en mémoire au démarrage de l'API
    - code sans adhérence à une BDD particulière
- utilisation de Spring Security en mode BASIC à la place de Keycloak pour l'authentification et les autorisations

----

## Bilan final

|                     | Open Source            | Insee                       |
| :---:               | :---:                  | :---:                       |
| Port d'écoute       | 8080                   | 8082                        |
| Base de données     | H2 en mémoire          | PostgreSQL à l'Insee        |
| Sécurité            | Spring Security BASIC  | Keycloak BEARER             |
| UtilisateurService  | UtilisateurServiceImpl | UtilisateurServiceImplInsee |

----

## Déploiement

- déploiement d'*api-opensource* en tant que bibliothèque Java sur le nexus de l'Insee (privé) ou sur Maven Central (public) avec la commande **mvn deploy**
- création du livrable **api-insee** qui va aller chercher la bibliothèque **api-opensource** sur le dépôt distant