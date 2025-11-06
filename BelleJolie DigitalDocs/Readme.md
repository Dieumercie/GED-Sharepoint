# 🏢 **Projet GED SharePoint — BelleJolie DigitalDocs**

## 📘 **Description**

Ce projet présente la mise en place complète d’une **solution de Gestion Électronique de Documents (GED)** dans **SharePoint Online**, pour une entreprise fictive nommée **BelleJolie**.
L’objectif est de centraliser la gestion documentaire, renforcer la traçabilité, et structurer les échanges entre les départements à travers un **site hub** et plusieurs **sites d’équipe connectés**.

---

## 🎯 **Objectifs du projet**

* Créer une **architecture SharePoint** organisée par département.
* Mettre en œuvre les **métadonnées gérées** pour la classification documentaire.
* Sécuriser les accès via **groupes et rôles**.
* Faciliter la **recherche** et la **navigation par taxonomie**.
* Gérer les **versions** et la **rétention documentaire**.
* Assurer la **traçabilité complète** grâce à l’**audit Microsoft Purview**.


---

## 🧩 **1. Architecture des sites**

| Site              | Type          | URL                                                        | Description                                          |
| ----------------- | ------------- | ---------------------------------------------------------- | ---------------------------------------------------- |
| **Hub principal** | Communication | `https://fashioncorp.sharepoint.com/sites/hub-fashioncorp` | Point d’entrée central regroupant les politiques GED |
| **RH**            | Équipe        | `/sites/RH`                                    | Dossiers du personnel, contrats, documents RH        |
| **Achats**        | Équipe        | `/sites/Achats`                                | Fournisseurs, factures, bons de commande             |
| **Finance**       | Équipe        | `/sites/Finance`                               | Budgets, bilans, reporting                           |
| **Direction**     | Communication | `/sites/Direction_BJ`                             | Documents stratégiques et internes                   |

> 🔗 Chaque site est associé au **hub BelleJolie** pour centraliser la recherche et la navigation.

<img width="1503" height="536" alt="image" src="https://github.com/user-attachments/assets/b044b3d1-ce68-4a54-8ca6-f75b4d40903c" />

---

## 📂 **2. Bibliothèques et listes**

### Bibliothèques principales

| Site      | Bibliothèque           | Contenu                           |
| --------- | ---------------------- | --------------------------------- |
| RH        | Dossiers du personnel  | CV, contrats, attestations        |
| Achats    | Factures fournisseurs  | Factures PDF, bons de commande    |
| Finance   | Documents financiers   | Bilans, journaux comptables       |
| Direction | Documents stratégiques | Plans d’action, rapports internes |

### Listes associées

| Site    | Liste        | Utilité                      |
| ------- | ------------ | ---------------------------- |
| Achats  | Fournisseurs | Référentiel des fournisseurs |
| RH      | Employés     | Référentiel du personnel     |


📸***Liste fournisseurs***

<img width="1377" height="657" alt="image" src="https://github.com/user-attachments/assets/3eb961d6-6f9e-41df-987e-7a9b65060b2b" />


📸***Liste employés***

<img width="1272" height="578" alt="image" src="https://github.com/user-attachments/assets/1818330f-ff7d-4a5b-9530-159153a1d2d8" />

---

## ⚙️ **3. Contrôle des versions**

Dans chaque bibliothèque :

* **Créer une version majeure à chaque modification** ✅
* **Exiger l’extraction avant modification** ✅
* **Conserver au moins 100 versions majeures**

📸 Example - Bibliothèque **Dossiers du personnel**


<img width="1650" height="803" alt="dossier du personnel - versionsconfig" src="https://github.com/user-attachments/assets/7b828e5c-22c1-4574-b634-4dcd6f435942" />

---

## 🏷️ **4. Métadonnées gérées (Taxonomie)**

### Groupe de termes : `BelleJolie Taxonomy`

* **Départements** : RH, Finance, Achats, Direction
* **Type de document** : Contrat, Facture, Rapport, Bon de commande, Note interne
* **Confidentialité** : Public, Interne, Restreint

### Colonnes de site

| Nom              | Type               | TermSet / Source |
| ---------------- | ------------------ | ---------------- |
| Département      | Métadonnées gérées | Département    |
| Type de document | Métadonnées gérées | Type de document |
| Confidentialité  | Métadonnées gérées | Confidentialité  |

> Ces colonnes sont ajoutées dans toutes les bibliothèques pour standardiser la classification.

📸 Example - Ensemble de terme **Département**


<img width="637" height="568" alt="image" src="https://github.com/user-attachments/assets/26040edc-8bad-42a7-855a-16180f94f8ae" />

---

## 🔐 **5. Sécurité et permissions**

### Groupes SharePoint par site (Il en existe par défaut)

* **Propriétaires** → Contrôle total
* **Membres** → Modification et extraction obligatoire
* **Lecteurs** → Lecture seule

### Permissions

* **RH** → Dossiers restreints aux RH
* **Achats** → Dossiers restreints aux commerciaux
* **Finance** → Dossiers stratégiques restreints à la Direction

---

## 🔄 **6. Relations entre listes et bibliothèques**

| Cas     | Relation                             | Champ de liaison | Objectif                                     |
| ------- | ------------------------------------ | ---------------- | -------------------------------------------- |
| Achats  | Fournisseurs ↔ Factures fournisseurs | Nom fournisseur | Associer chaque facture à un fournisseur     |
| RH      | Employés ↔ Dossiers du personnel     | Nom complet      | Associer chaque document RH à un employé     |


> Colonnes de type **Recherche (Lookup)** utilisées pour établir les liens entre listes et bibliothèques.

📸 Example - Relation **Fournisseurs** et **Facture fournisseur**


<img width="1127" height="232" alt="image" src="https://github.com/user-attachments/assets/c255a649-cff1-41be-ac56-23a86ba6e866" />

---

## 👁️ **7. Vues personnalisées**

### Exemple de vues :

* **Documents récents** → Trier par *Date de modification décroissante* et filtrer sur *Date de modification (modifié >= [Aujourd'hui]-15) : Fichiers modifiés les 15 derniers jours*
* **Mes documents** → Filtrer sur *Créé par = [Utilisateur actif] : Documents créés par l'utilisateur connecté*

📸 Example - Vue **Documents récents** sur **Dossiers du personnel**


<img width="997" height="278" alt="image" src="https://github.com/user-attachments/assets/99425a13-8e85-46ba-a552-fe7c24991f01" />

---

## 🔍 **8. Recherche par métadonnées**

### Locale (dans une bibliothèque)

* Activer les **Paramètres de navigation par métadonnées** si pas visible dans les **Paramètres du site** > **Gérer les fonctionnalités du site**
* Ajouter les colonnes : *Type de document*, *Département*

📸 Example - Bibliothèque **Dossiers du personnel**

<img width="846" height="646" alt="image" src="https://github.com/user-attachments/assets/edde3fd0-8c41-44e0-a743-6bbc639c1a2e" />

### Globale (au niveau du site : mappage)

* Aller dans **Paramètres du site > Administration de la collection de sites > Schéma de recherche**
* Créer une nouvelle propriété gérée
* Configurer les **propriétés gérées** pour les colonnes de taxonomie
* Activer les **colonnes indexées** pour un affichage rapide

📸 Example - Site **RH**

<img width="1326" height="526" alt="image" src="https://github.com/user-attachments/assets/2e8c65b8-8049-46e6-bc83-51c352b9ffa0" />

---


## 🕓 **9. Historique des versions**

### Pour un fichier :

Voir l'historique des versions d'un fichier

<img width="962" height="233" alt="image" src="https://github.com/user-attachments/assets/f8c5724b-e484-46aa-ae22-86a91ccb2f2a" />

---

## 🧾 **10. Configuration de l’audit (Microsoft Purview)**


<img width="1738" height="503" alt="image" src="https://github.com/user-attachments/assets/8e73f442-047a-45fa-a1b4-adfe429a06c7" />



---

## 📊 **12. Résumé des livrables**

| Élément          | Description                                  |
| ---------------- | -------------------------------------------- |
| 🧩 Architecture  | 1 site hub + 4 sites d’équipe                |
| 📂 Bibliothèques | 4 principales avec versioning et extraction  |
| 🏷️ Taxonomie    | Groupe de termes + colonnes de site gérées   |
| 🔐 Sécurité      | Groupes + permissions  |
| 🔍 Recherche     | Par taxonomie + recherche hub                |             
| 🧾 Audit         | Activé via Microsoft Purview                 |
| 📄 Versioning    | 100 versions majeures conservées              |
| 📋 Listes        | Fournisseurs, Employés |

---



## 🚀 **Prochaines évolutions**

* Ajouter un **tableau de bord Power BI** 
* Intégrer **Power Automate Cloud** pour les flux de validation documentaire (ex. contrat RH à valider par le manager)


### 📅 Auteur : **Audrey — 2025**





