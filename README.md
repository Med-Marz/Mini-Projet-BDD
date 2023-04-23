# Mini-Projet BD
### Mohamed Marzougui LGLSI2-TD2-TP4
## Domaine d'application : Bibliothèque
La base de données permettra de gérer les informations d'une bibliothèque, 
y compris les informations sur les livres, les auteurs, les emprunts, les lecteurs et les employés de la bibliothèque.

## Le Modèle Conceptuel de Données (MCD):
- le schéma relationnel correspondant au MCD:
* Livre (ID_Livre, Titre, ID_Auteur, Date_Publication)
   * ID_Livre : clé primaire de la table Livre
   * ID_Auteur : clé étrangère référençant la table Auteur
* Auteur (ID_Auteur, Nom_Auteur, Date_Naissance)
   * ID_Auteur : clé primaire de la table Auteur
   * Lecteur (ID_Lecteur, Nom_Lecteur, Date_Naissance)
   * ID_Lecteur : clé primaire de la table Lecteur
* Exemplaire (ID_Exemplaire, ID_Livre, Disponible)
   * ID_Exemplaire : clé primaire de la table Exemplaire
   * ID_Livre : clé étrangère référençant la table Livre
* Emprunt (ID_Emprunt, ID_Lecteur, ID_Exemplaire, Date_Emprunt, Date_Retour)
   * ID_Emprunt : clé primaire de la table Emprunt
   * ID_Lecteur : clé étrangère référençant la table Lecteur
   * ID_Exemplaire : clé étrangère référençant la table Exemplaire

## Le Modèle Logique de Données (MLD) :
- Table Livre :
    ID_Livre (clé primaire)
    Titre
    ID_Auteur (clé étrangère)
    Date_Publication

- Table Auteur :
    ID_Auteur (clé primaire)
    Nom_Auteur
    Date_Naissance

- Table Lecteur :
    ID_Lecteur (clé primaire)
    Nom_Lecteur
    Date_Naissance

- Table Exemplaire :
    ID_Exemplaire (clé primaire)
    ID_Livre (clé étrangère)
    Disponible

- Table Emprunt :
    ID_Emprunt (clé primaire)
    ID_Lecteur (clé étrangère)
    ID_Exemplaire (clé étrangère)
    Date_Emprunt
    Date_Retour
------------
## Requêtes de création des tables:

```sql
CREATE TABLE Auteur (
  ID_Auteur NUMBER PRIMARY KEY,
  Nom_Auteur VARCHAR2(50),
  Date_Naissance DATE
);

CREATE TABLE Livre (
  ID_Livre NUMBER PRIMARY KEY,
  Titre VARCHAR2(100),
  Annee_Publication NUMBER,
  ID_Auteur NUMBER,
  FOREIGN KEY (ID_Auteur) REFERENCES Auteur(ID_Auteur)
);

CREATE TABLE Lecteur (
  ID_Lecteur NUMBER PRIMARY KEY,
  Nom_Lecteur VARCHAR2(50),
  Date_Naissance Date
);

CREATE TABLE Exemplaire (
  ID_Exemplaire NUMBER PRIMARY KEY,
  ID_Livre NUMBER,
  FOREIGN KEY (ID_Livre) REFERENCES Livre(ID_Livre),
  Statut_Exemplaire VARCHAR2(50)
);

CREATE TABLE Emprunt (
  ID_Emprunt NUMBER PRIMARY KEY,
  ID_Lecteur NUMBER,
  FOREIGN KEY (ID_Lecteur) REFERENCES Lecteur(ID_Lecteur),
  ID_Livre NUMBER,
  FOREIGN KEY (ID_Livre) REFERENCES Livre(ID_Livre),
  Date_Emprunt DATE
);
```
----------
## Créer un utilisateur "Bibliothécaire" avec un mot de passe :
```sql
CREATE USER Bibliothecaire IDENTIFIED BY MonMotDePasse1;
```
## Créer un rôle "Bibliothécaire" :
```sql
CREATE ROLE Bibliothecaire;
```
## Accorder le rôle "Bibliothécaire" à l'utilisateur "Bibliothécaire" :
```sql
GRANT Bibliothecaire TO Bibliothecaire;
```
## Attribution des privilèges à l'utilisateur "Bibliothécaire" :
```sql
GRANT SELECT, INSERT, UPDATE, DELETE ON Auteur TO Bibliothécaire;
GRANT SELECT, INSERT, UPDATE, DELETE ON Livre TO Bibliothécaire;
GRANT SELECT, INSERT, UPDATE, DELETE ON Exemplaire TO Bibliothécaire;
GRANT SELECT, INSERT, UPDATE, DELETE ON Emprunt TO Bibliothécaire;
GRANT SELECT, INSERT, UPDATE, DELETE ON Lecteur TO Bibliothécaire;
```
## Créer un utilisateur "Lecteur" avec un mot de passe :
```sql
CREATE USER Lecteur IDENTIFIED BY MonMotDePasse2;
```
## Créer un rôle "Lecteur" :
```sql
CREATE ROLE Lecteur;
```
## Accorder le rôle "Lecteur" à l'utilisateur "Lecteur" :
```sql
GRANT Lecteur TO Lecteur;
```
## Attribution des privilèges à l'utilisateur "Lecteur" :
```sql
GRANT SELECT ON Auteur TO Lecteur;
GRANT SELECT ON Livre TO Lecteur;
GRANT SELECT ON Exemplaire TO Lecteur;
GRANT SELECT ON Emprunt TO Lecteur;
GRANT SELECT ON Lecteur TO Lecteur;
```
----------
## Mettre à jour le titre d'un livre :
```sql
UPDATE Livre
SET Titre = 'Nouveau titre'
WHERE ID_Livre = 123;
```
Cette requête met à jour le titre du livre ayant l'identifiant "123" dans la table "Livre".

## Mettre à jour le nombre d'exemplaires disponibles d'un livre :
```sql
UPDATE Livre
SET Nb_Exemplaires_Disponibles = Nb_Exemplaires_Disponibles - 1
WHERE ID_Livre = 456;
```
Cette requête diminue le nombre d'exemplaires disponibles pour le livre ayant l'identifiant "456" dans la table "Livre" 
lorsqu'un exemplaire est emprunté.

## Mettre à jour la date de retour d'un emprunt : 
```sql
UPDATE Emprunt
SET Date_Retour = SYSDATE
WHERE ID_Emprunt = 789;
```
Cette requête met à jour la date de retour d'un emprunt ayant l'identifiant "789" dans la table "Emprunt" 
lorsqu'un lecteur retourne un exemplaire emprunté. 
La fonction SYSDATE permet de mettre à jour la date de retour avec la date et l'heure actuelles du système.
----------
## Insérer un nouveau livre:
```sql
INSERT INTO Livre (ID_Livre, Titre, ID_Auteur, Annee_Publication)
VALUES (1, 'Le Petit Prince', 1, 1943);
```
## Insérer un nouvel auteur:
```sql
INSERT INTO Auteur (ID_Auteur, Nom_Auteur, Date_Naissance)
VALUES (1, 'Friedrich Nietzsche', TO_DATE('15-10-1844', 'DD-MM-YYYY'));
```
## Insérer un nouveau lecteur:
```sql
INSERT INTO Lecteur (ID_Lecteur, Nom_Lecteur, Date_Naissance)
VALUES (1, 'Ahmed', TO_DATE('01-01-1990', 'DD-MM-YYYY'));
```
----------
## Sélectionner tous les auteurs dont le nom commence par "Friedrich" :
```sql
SELECT * FROM Auteur WHERE Nom_Auteur LIKE 'Friedrich%';
```
## Sélectionner tous les livres publiés après 2010 :
```sql
SELECT * FROM Livre WHERE Annee_Publication > 2010;
```
## Sélectionner tous les exemplaires qui sont disponibles (non empruntés) :
```sql
SELECT * FROM Exemplaire WHERE Statut_Exemplaire = 'Disponible';
```

----------
```sql
SELECT Titre, Auteur
FROM Livre
ORDER BY Titre ASC;
```
Cette requête sélectionne les colonnes Titre et Auteur de la table Livre et les trie par ordre alphabétique croissant du titre (en utilisant l'option ASC).
-----------
```sql
SELECT ID_Lecteur, COUNT(*) AS Nb_Emprunts
FROM Emprunt
GROUP BY ID_Lecteur;
```
Cette requête va afficher l'identifiant de chaque lecteur ainsi que le nombre d'emprunts qu'il a effectués. 
GROUP BY permet de regrouper les emprunts par lecteur, et la fonction COUNT(*) permet de compter le nombre d'emprunts pour chaque groupe. 
-----------
```sql
SELECT ID_Lecteur, COUNT(*) AS Nb_Emprunts
FROM Emprunt
GROUP BY ID_Lecteur
HAVING COUNT(*) > 5;
```
Cette requête va afficher le nombre d'emprunts pour chaque lecteur ayant effectué plus de 5 emprunts. 
La clause HAVING va filtrer les résultats de la requête GROUP BY pour ne retourner que les résultats où le nombre d'emprunts est supérieur à 5.
