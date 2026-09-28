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

**Capture d’écran :**

