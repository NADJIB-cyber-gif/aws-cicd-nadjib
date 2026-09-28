# Projet AWS — Microservices et CI/CD

**Étudiant :** Nadjib Benzerrouk

## Objectif du projet

Transformer une application monolithique en deux microservices :
- un service client pour consulter les fournisseurs ;
- un service employé pour gérer les fournisseurs.

Mettre en place un pipeline pour automatiser leur déploiement sur AWS.

## Rapport de progression

Ce rapport sera complété progressivement. Pour chaque tâche, je présenterai :
- les actions réalisées ;
- les captures d’écran ;
- les résultats obtenus ;
- les difficultés rencontrées et les solutions.

## Phase 1 — Architecture et estimation des coûts

À compléter avec le schéma d’architecture et l’estimation des coûts.


## Phase 2 — Analyse de l’application monolithique

### Tâche 2.1 — Vérifier la disponibilité de l’application

**Objectif :** vérifier que l’application web est accessible depuis Internet.

**Actions réalisées :**
- Sélection de l’instance MonolithicAppServer dans Amazon EC2.
- Copie de son adresse IPv4 publique.
- Ouverture de cette adresse dans le navigateur avec HTTP.

**Résultat :** la page « Monolithic Coffee suppliers » s’affiche correctement.
<img width="1826" height="1076" alt="image" src="https://github.com/user-attachments/assets/685a860e-bf28-433d-b7dd-12c7aff38add" />

### Tâche 2.2 — Tester l’application web monolithique

**Objectif :** vérifier l’ajout et la modification d’un fournisseur.

**Actions réalisées :**
- Consultation de la liste des fournisseurs sur `/suppliers`.
- Ajout du fournisseur fictif « Cafe Test Nadjib » sur `/supplier-add`.
- Modification de sa ville de Montreal à Laval.
- Utilisation de coordonnées fictives pour les tests.

**Résultat :** le fournisseur apparaît dans la liste et la modification de la ville est enregistrée.

**Capture d’écran :**
<img width="1905" height="1072" alt="image" src="https://github.com/user-attachments/assets/b317235c-9180-4ec6-9342-4262e77c3339" />

### Tâche 2.3 — Analyser le fonctionnement de l’application

**Objectif :** comprendre comment l’application s’exécute et où ses données sont stockées.

**Actions réalisées :**
- Connexion au serveur MonolithicAppServer avec EC2 Instance Connect.
- Vérification du port 80 avec `sudo lsof -i :80`.
- Identification du processus avec `ps -ef | head -1; ps -ef | grep node`.
- Consultation des fichiers dans `~/resources/codebase_partner`.
- Vérification de l’accès à RDS avec `nmap -Pn` : le port 3306/TCP est ouvert.
- Connexion à MySQL et exécution des commandes suivantes :
  - `SHOW DATABASES;`
  - `USE COFFEE;`
  - `SHOW TABLES;`
  - `SELECT * FROM suppliers;`

**Résultats :**
- L’application est exécutée sur EC2 par Node.js avec la commande `node index.js`.
- Le processus utilise le compte root, porte le PID 470 et écoute sur le port 80.
- Les données sont stockées dans RDS, dans la table `suppliers` de la base `COFFEE`.
- Le fournisseur « Cafe Test Nadjib » et sa ville « laval » sont présents dans la table.

**Difficulté rencontrée :** la connexion MySQL a d’abord retourné « Access denied ». Une nouvelle saisie des identifiants a permis la connexion.

**Captures d’écran :**
<img width="1851" height="986" alt="image" src="https://github.com/user-attachments/assets/12479856-16a8-4619-b087-607353d934ae" />

### Tâche 3.1 — Créer l’environnement de développement Cloud9

**Objectif :** préparer un espace de travail pour modifier le code et tester les microservices.

**Configuration :**
- Nom : MicroservicesIDE
- Nouvelle instance EC2 : t3.small
- Système : Amazon Linux 2023
- Connexion : SSH
- Réseau : LabVPC
- Sous-réseau : Public Subnet 1
- Mise en veille après 30 minutes d’inactivité

**Résultat :** l’environnement Cloud9 est créé et l’éditeur est ouvert.

**Capture d’écran :**
<img width="1865" height="1031" alt="image" src="https://github.com/user-attachments/assets/82ea6038-55f9-4f8c-ba29-73ebbe82e90b" />

### Tâche 3.2 — Copier le code dans Cloud9

**Objectif :** récupérer le code de l’application monolithique dans l’environnement de développement.

**Actions réalisées :**
- Importation de la clé SSH dans Cloud9.
- Protection de la clé avec `chmod 400`.
- Création du dossier `~/environment/temp`.
- Copie du code depuis MonolithicAppServer avec `scp`.
- Vérification des fichiers avec `ls ~/environment/temp`.

**Résultat :** le code de l’application est disponible dans le dossier `temp` de Cloud9.

**Capture d’écran :**
<img width="1895" height="1091" alt="image" src="https://github.com/user-attachments/assets/83bbcc0f-f27f-4805-9c3c-8319286acce3" />

### Tâche 3.3 — Préparer les dossiers des microservices

**Objectif :** préparer deux copies du code pour les futurs services client et employé.

**Actions réalisées :**
- Création des dossiers `microservices/customer` et `microservices/employee`.
- Copie du code de l’application dans chaque dossier.
- Vérification avec `diff` : les deux copies sont identiques au code original.
- Suppression du dossier temporaire `temp`.

**Résultat :** les deux dossiers contiennent le code de départ. Leurs fonctionnalités seront adaptées pendant la phase 4.

**Difficulté rencontrée :** une première vérification a révélé des fichiers manquants. La copie a été refaite, puis vérifiée avec succès.

**Capture d’écran :**
<img width="1887" height="1032" alt="image" src="https://github.com/user-attachments/assets/3237faa5-50ef-402c-b3a2-54bd59670971" />

### Tâche 3.4 — Enregistrer le code dans CodeCommit

**Objectif :** conserver les versions du code dans un dépôt Git distant.

**Actions réalisées :**
- Création du dépôt AWS CodeCommit `microservices`.
- Initialisation de Git dans le dossier `microservices`.
- Création de la branche `dev` et configuration de l’auteur des commits.
- Enregistrement de la première version avec `git add` et `git commit`.
- Connexion au dépôt distant et envoi du code avec `git push`.
- Vérification des dossiers dans la console CodeCommit.

**Résultat :** les dossiers `customer` et `employee` sont présents dans le dépôt `microservices`, sur la branche `dev`.

**Capture d’écran :**
<img width="1867" height="1005" alt="image" src="https://github.com/user-attachments/assets/85b87ba6-d163-45de-ae0a-3f5931fb9b74" />


