# 🚀 Projet : Solution de Gestion des Demandes d'Achat (DA) sur SharePoint Online

Ce projet est une solution complète d'automatisation des processus de Demande d'Achat (DA), développée en utilisant les fonctionnalités avancées de **SharePoint Online** et la **Microsoft Power Platform**.

Il illustre la mise en œuvre de la gouvernance des données, la personnalisation de l'expérience utilisateur et l'automatisation des workflows d'approbation.

---

## 🎯 Objectifs de la Solution

L'objectif principal de cette solution est de digitaliser et d'automatiser le processus de demande et d'approbation d'achats, en appliquant des règles de gestion spécifiques :

1.  **Validation Automatique :** Toute DA d'un montant inférieur à 500 € est automatiquement approuvée.
2.  **Approbation Manuelle :** Toute DA d'un montant supérieur ou égal à 500 € déclenche un workflow d'approbation vers le manager désigné.
3.  **Qualité des Données :** Assurer la validité et la cohérence des données saisies (ex: montant > 0).

---

## 🛠️ Composants Techniques

| Composant | Rôle | Concepts Avancés Appliqués |
| :--- | :--- | :--- |
| **SharePoint Listes** | Stockage structuré des demandes d'achat. | Colonnes calculées, Indexation pour performance, Validation de colonne. |
| **Power Apps** | Personnalisation de l'interface de saisie des DA. | Formulaire intégré, masquage de champs, gestion des `OnSave` et `SubmitForm`. |
| **Power Automate** | Automatisation du workflow d'approbation. | Déclencheur SharePoint, condition Si/Alors (IF/ELSE), Actions d'Approbation (`Start and wait for an approval`), mise à jour conditionnelle des éléments. |
| **Gouvernance** | Sécurité et performance de l'environnement. | Gestion des permissions (rupture d'héritage), création d'index de colonne. |

---

## ⚙️ Mise en Œuvre et Fonctionnalités Clés

### 1. Modélisation de la Liste SharePoint

| Colonne | Type | Rôle et Validation |
| :--- | :--- | :--- |
| `MontantDA` | Nombre (Décimales) | **Validation :** Requiert un montant strictement supérieur à zéro (`=[MontantDA]>0`). |
| `Demandeur` | Personne ou Groupe | Permet l'identification unique dans l'annuaire (Azure AD) - Crucial pour l'automatisation. |
| `StatutApprobation` | Choix | Suivi du statut (`Soumise`, `En attente`, `Approuvée`, `Rejetée`). |
| **`Appro_Requise`** | **Calculée** | Détermine la logique du flux : `=SI([MontantDA]>=500;"OUI";"NON")`. |

### 2. Flux Power Automate (Détail de la Logique)

Le flux s'exécute **`Lorsqu'un élément est créé`** dans la liste `Demandes d'Achat`.

Voici le diagramme logique du flux conditionnel :



1.  **Mise à jour Initiale :** Le statut est mis à `Soumise`.
2.  **Condition 1 (Critère de Montant) :**
    * **Vérification :** `[Appro_Requise]` est égal à `"OUI"`.
3.  **Si VRAI (Appro. Requise) :**
    * Action : `Démarrer et attendre une approbation`.
    * Condition 2 : Vérification du **`Résultat`** de l'approbation (`Approuver` ou `Rejeter`).
    * Mise à jour du statut final (`Approuvée` ou `Rejetée`).
4.  **Si FAUX (Approbation non requise) :**
    * Mise à jour immédiate du statut à **`Approuvée`**.

### 3. Gestion de la Gouvernance

* **Permissions :** L'héritage a été rompu sur la liste pour donner aux utilisateurs de base un niveau de permission **`Contribution`** uniquement (pour créer des éléments), mais pas le contrôle total.
* **Performance :** Un **Index de colonne** a été créé sur la colonne `StatutApprobation` pour assurer des requêtes rapides (notamment pour les vues filtrées et le flux Power Automate) même si la liste dépasse le seuil de vue de 5000 éléments.

---

## 💡 Leçons Apprises (Points Clés du Projet)

* **Distinction Formule :** Maîtrise de la différence entre la **formule de colonne calculée** (qui retourne une valeur) et la **formule de validation** (qui retourne VRAI/FAUX).
* **Intégration Power Apps :** Gestion des propriétés d'intégration (`OnSave`) pour connecter correctement le formulaire à l'action d'enregistrement SharePoint, sans ajouter de bouton de soumission manuel.
* **Types de Données :** L'importance du type de colonne **"Personne ou Groupe"** pour des workflows d'approbation robustes et la connexion avec les profils d'utilisateurs.

---

📸 ##Captures

* **Liste des demandes :** 

<img width="1151" height="382" alt="image" src="https://github.com/user-attachments/assets/cae4947b-5c88-48d5-9cd9-5ccb7b487186" />

* **Formulaire de demande :**
<img width="491" height="602" alt="image" src="https://github.com/user-attachments/assets/31dd63c5-8ab1-467e-beb7-beaed76ef56d" />

<img width="537" height="587" alt="image" src="https://github.com/user-attachments/assets/c540a85e-dfeb-46c7-8b60-aceab750861c" />

* * **Flux Power Automate :**
<img width="1112" height="803" alt="image" src="https://github.com/user-attachments/assets/705b8ace-eb81-4626-8ee9-042439d18ea5" />
<img width="777" height="793" alt="image" src="https://github.com/user-attachments/assets/fa088c6f-081c-4b1a-91e4-1fdd5834fcb3" />
<img width="616" height="345" alt="image" src="https://github.com/user-attachments/assets/40c0fba3-f55d-43cb-a801-d35fe68151dd" />
<img width="692" height="762" alt="image" src="https://github.com/user-attachments/assets/05c082c9-d8d1-4d17-aea9-a9375392bc28" />

---

## ➡️ Prochaines Évolutions Possibles

* **Notifications Teams :** Intégrer des notifications de statut de DA dans un canal Teams via Power Automate.
* **Rapport Power BI :** Créer un tableau de bord Power BI pour visualiser le temps d'approbation moyen et le volume des DA par service.
* **Gestion des Pièces Jointes :** Mettre en place la vérification et la copie des pièces jointes de la DA vers une autre bibliothèque sécurisée.


---






### 📅 Auteur : **Audrey — 2025**
