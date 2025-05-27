# Projet GCP (ETL Data Pipeline with Cloud Data Fusion)

Dans ce projet, nous verrons comment importer des données dans un bucket Google Cloud Storage (GCS), puis les transformer à l’aide de l’outil visuel Wrangler dans une instance Cloud Data Fusion (étape de transformation). Ensuite, nous créerons un pipeline pour charger les données transformées dans BigQuery (étape de chargement). Enfin, nous verrons comment connecter Looker Studio (anciennement Google Data Studio) à BigQuery afin de réaliser des visualisations à partir des données nettoyées et structurées.

<img width="601" alt="Airflow01" src="https://github.com/user-attachments/assets/9cc5e917-a9c1-4bfe-a569-b984bf1f0601" />

Nous commençons par accéder à la console Google Cloud Platform (GCP). Pour ce projet, j'utilise l'essai gratuit, comme cela a été le cas pour le projet **MSBI SSIS (ETL)** que je vous invite à consulter dans la section *Projets* de mon profil.

### Création de l’instance Cloud Data Fusion

Je commence par créer une instance **Cloud Data Fusion**, qui nous permettra d’orchestrer notre pipeline ETL à l’aide de son interface visuelle.

`--------data_fusion0--------`

Pendant les 20 minutes nécessaires à la création de l’instance, je crée un **bucket GCS (Google Cloud Storage)**, qui servira de source de données. Pour cela, je recherche “Cloud Storage” dans la barre de navigation GCP.

`--------bucket1--------`

Une fois le bucket créé, j’y importe mon fichier CSV nommé **employee\_data.csv**, qui contient les données brutes à traiter.

`--------bucket2--------`

---

### Lancement de l’environnement Wrangler

Lorsque l’instance Cloud Data Fusion est prête, je clique sur **"Afficher l’instance"**. Après avoir accepté les conditions d’utilisation liées au compte, j’accède à **Wrangler**, l’outil visuel de transformation de données.

`--------wrangler3--------`

Dans Wrangler, je retrouve mon bucket et le fichier importé.

* Vue du dossier contenant les données :
  `--------wrangler4--------`

* Vue des données du fichier CSV :
  `--------wrangler5--------`

---

### 🛠️ Préparation et transformation des données (Wrangler)

Dans cet espace visuel, j’effectue les étapes de **préparation des données** :

* 🔤 **Création d'une colonne Full\_name** : en concaténant les colonnes `first_name` et `last_name`
* 🔒 **Masquage des données sensibles** : par exemple, en **masquant les salaires** avec une transformation de type “mask”
* 🔐 **Encodage des mots de passe** : par **hachage MD5** pour les sécuriser

Aperçu des transformations appliquées dans Wrangler :

`--------wrangler6--------`

---

### ⚙️ Création et déploiement du pipeline ETL

Une fois les transformations terminées, je clique en haut à droite sur **“Create a pipeline”**, puis je choisis **“Batch pipeline”**. Cela m’ouvre l’interface de création du pipeline.

`--------pipeline7--------`

Dans cette interface :

1. Je crée une destination (sink) **BigQuery**
2. J’indique le **nom du dataset** et **l’identifiant du projet**
3. Je donne un **nom au pipeline**
4. Puis je clique sur **“Deploy”** pour le déployer

`--------pipeline8--------`

Une fois déployé, je suis redirigé vers l’écran d’exécution où je peux lancer le pipeline.

`--------pipeline9--------`

---

### ⚠️ Avant d'exécuter le pipeline, penser à :

* **Activer l’API Cloud Resource Manager**
* **Attribuer les bonnes permissions IAM** à l’instance :

  * Rôle **Dataproc Editor**
  * Rôle **Storage Admin (ou Storage Object Admin)**
* **Passer les buckets en mode “autorisations uniformes”** dans GCS

Une fois le pipeline exécuté avec succès (*Status: Succeeded*), je peux revenir dans BigQuery, actualiser la page et vérifier que mon dataset et ma table ont bien été créés.

`--------bigquery_table10--------`

---

### 📊 Visualisation des données avec Looker Studio

Enfin, je connecte **BigQuery à Looker Studio** (anciennement Google Data Studio) pour créer des tableaux de bord à partir des données transformées.

`--------looker_studio11--------`

Exemple de visualisation générée à partir du champ `full_name` :

`--------looker_studio12--------`
