# TP2
## Proposition de mise à jour du système de sestion de bibliothèque
### présenté par :
### Patrick Leclair, Dinh Hoa Luu, Tristan Major-Labelle, Abdel Jalil Zouhair 


## Solution Proposée

### Modifications au Front-end (Vue.js)
1. Interface de Recherche de livres : Ajouter une interface utilisateur permettant aux usagers de rechercher des livres par son ISBN et/ou son titre ou son auteur ou son année.
2. Interface d'ajout de livre : Ajouter une interface utilisateur permettant aux bibliothécaires d'entrer les détails du livre, y compris l'ISBN (si disponible) ou le titre.
3.	Validation de l'entrée : Intégrer une validation pour vérifier la présence soit de l'ISBN soit du titre (pour les livres sans ISBN).
   
### Modifications au Back-end (Express.js)
1.	Validation de la recherche : Ajouter une logique pour retourner les résultats de la recherche soit par ISBN ou titre  ou auteur ou année.
2. Ajout de livre : Implémenter la fonctionnalité pour insérer les détails du livre dans MongoDB.
3. Validation de l'ISBN : Ajouter une logique pour valider l'ISBN et vérifier s'il existe déjà dans la base de données MongoDB.
4.	Gestion des livres sans ISBN : Si l'ISBN n'est pas disponible, utiliser le titre comme identifiant.


### Base de Données (MongoDB)
1.	Modification de la structure de données : Adapter le schéma de la base de données pour gérer les livres y compris ceux sans ISBN.

## Diagramme de cas d'utilisation
![](UseCaseTP2.drawio.png)
## Diagramme de séquence
```mermaid
sequenceDiagram
    participant Usager as "👤 Usager"
     participant Bibliothécaire as "👤 Bibliothécaire"
    participant Système as "💻 Système"
    participant MongoDB as "🏢 MongoDB"

    Usager->> + Système: Rechercher un livre
        Système ->> + Usager : demander ISBN ou titre ou auteur ou année
        Usager ->> - Système : Entrer ISBN ou titre ou auteur ou année
        Système-->> +MongoDB: Rechercher le livre par ISBN ou titre ou auteur ou année
        MongoDB-->>-Système: Renvoyer les résultats de la recherche

        alt : si ISBN ou Titre ou auteur ou année est valide:
            Système->>Usager: Afficher les résultats de livre
        else : Sinon : 
            Système ->> - Usager : Afficher livre inexistant
        end

    Bibliothécaire->> + Système: Ajouter un livre (créer un nouveau livre)
    Système->>Bibliothécaire: Demander ISBN ou titre
       
    Bibliothécaire->>Système: Entrer ISBN   
    Système-->>Système: Valider ISBN (si fourni)
    alt : Si ISBN non valide
     Système ->> Bibliothécaire : retourner msg d'erreur<br>(ISBN non valide)<br>demander à nouveau ISBN
     
    else : Si ISBN valide
    Système-->>MongoDB: Vérifier si ISBN existe
    MongoDB-->>Système: Renvoyer le statut de l'ISBN
        alt : Si ISBN existe
            Système->>Bibliothécaire: ISBN existe déjà, informer le bibliothécaire
        else : Si ISBN n'existe pas
            Système ->> Bibliothécaire: demander titre (obligatoire)
            Bibliothécaire->>Système: Entrer Titre 
            Système ->> Bibliothécaire: demander détails du livre
            Bibliothécaire->>Système: Ajouter les détails du livre (auteur, année, etc.)
        end
       
       

    Système-->>MongoDB: Sauvegarder les informations du livre
    Système->> - Bibliothécaire: Livre ajouté avec succès
    end
    

```
