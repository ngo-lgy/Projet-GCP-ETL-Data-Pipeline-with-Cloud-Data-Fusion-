# Projet GCP (ETL Data Pipeline with Cloud Data Fusion)

Dans ce projet, nous mettons en place un pipeline de données complet, de l'importation de fichiers dans Google Cloud Storage (GCS) à leur transformation via Cloud Data Fusion (Wrangler), jusqu'au chargement dans BigQuery. Enfin, nous connectons Looker Studio (anciennement Google Data Studio) à BigQuery pour créer des visualisations sur les données chargées.

<img width="600" alt="pipeline00" src="https://github.com/user-attachments/assets/e19aab41-fe5a-4f28-abdd-f262d1960e47" />


Nous commençons par accéder à la console Google Cloud Platform (GCP). Pour ce projet, j'utilise l'essai gratuit, comme cela a été le cas pour le projet **MSBI SSIS (ETL)** que je vous invite à consulter dans la section *Projets* de mon profil.

### Création de l’instance Cloud Data Fusion

Je commence par créer une instance **Cloud Data Fusion**, qui nous permettra d’orchestrer notre pipeline ETL à l’aide de son interface visuelle.

<img width="959" alt="data_fusion0" src="https://github.com/user-attachments/assets/73dea6b8-aabf-4ffc-b898-76dbfd2b5b14" />

Pendant les 20 minutes nécessaires à la création de l’instance, je crée un **bucket GCS (Google Cloud Storage)**, qui servira de source de données. Pour cela, je recherche “Cloud Storage” dans la barre de navigation GCP.

<img width="958" alt="bucket1" src="https://github.com/user-attachments/assets/62f6ecfa-c3f7-40e6-9792-d20fca004809" />

Une fois le bucket créé, j’y importe mon fichier CSV nommé **employee\_data.csv**, qui contient les données brutes à traiter.

<img width="958" alt="bucket2" src="https://github.com/user-attachments/assets/19efdf51-03ac-4c55-93c3-39a227bffb76" />

---

### Lancement de l’environnement Wrangler

Lorsque l’instance Cloud Data Fusion est prête, je clique sur **"Afficher l’instance"**. Après avoir accepté les conditions d’utilisation liées au compte, j’accède à **Wrangler**, l’outil visuel de transformation de données.

<img width="956" alt="warngler3" src="https://github.com/user-attachments/assets/651d3ab3-e0f5-4f52-a33b-5fa04a4f8817" />

Dans Wrangler, je retrouve mon bucket et le fichier importé.

* Vue du dossier contenant les données :
 
<img width="959" alt="wrangler4" src="https://github.com/user-attachments/assets/fc78b93b-ea3c-4c06-9d05-d54499f1cdd0" />

* Vue des données du fichier CSV :
  
<img width="959" alt="wrangler5" src="https://github.com/user-attachments/assets/960983bb-b617-413c-9bcf-5ee523daad58" />

---

### 🛠️ Préparation et transformation des données (Wrangler)

Dans cet espace visuel, j’effectue les étapes de **préparation des données** :

* 🔤 **Création d'une colonne Full\_name** : en concaténant les colonnes first_name et`last_name
* 🔒 **Masquage des données sensibles** : par exemple, en **masquant les salaires** avec une transformation de type “mask”
* 🔐 **Encodage des mots de passe** : par **hachage MD5** pour les sécuriser

Aperçu des transformations appliquées dans Wrangler :


<img width="718" alt="wrangler6" src="https://github.com/user-attachments/assets/a1b0c25f-1c92-4080-838b-b9d9e496f097" />

---

### ⚙️ Création et déploiement du pipeline ETL

Une fois les transformations terminées, je clique en haut à droite sur **“Create a pipeline”**, puis je choisis **“Batch pipeline”**. Cela m’ouvre l’interface de création du pipeline.

<img width="958" alt="pipeline7" src="https://github.com/user-attachments/assets/76d1ac28-f439-4195-a3de-4f967ccb9f11" />

Dans cette interface :

1. Je crée une destination (sink) **BigQuery**
2. J’indique le **nom du dataset** et **l’identifiant du projet**
3. Je donne un **nom au pipeline**
4. Puis je clique sur **“Deploy”** pour le déployer

<img width="957" alt="pipeline8" src="https://github.com/user-attachments/assets/b3563120-8fe6-40c1-b9f5-f15442cdb642" />

Une fois déployé, je suis redirigé vers l’écran d’exécution où je peux lancer le pipeline.

<img width="956" alt="pipeline9" src="https://github.com/user-attachments/assets/cf10d873-7bbf-4a6d-bf71-b9d8f5901d56" />

---

### ⚠️ Avant d'exécuter le pipeline, penser à :

* **Activer l’API Cloud Resource Manager**
* **Attribuer les bonnes permissions IAM** à l’instance :

  * Rôle **Dataproc Editor**
  * Rôle **Storage Admin (ou Storage Object Admin)**
* **Passer les buckets en mode “autorisations uniformes”** dans GCS

Une fois le pipeline exécuté avec succès (*Status: Succeeded*), je peux revenir dans BigQuery, actualiser la page et vérifier que mon dataset et ma table ont bien été créés.

<img width="957" alt="bigquery_table10" src="https://github.com/user-attachments/assets/df088f62-fcaa-4422-ae28-e207453a9cc7" />

---

### 📊 Visualisation des données avec Looker Studio

Enfin, je connecte **BigQuery à Looker Studio** (anciennement Google Data Studio) pour créer des tableaux de bord à partir des données transformées.

<img width="959" alt="looker_studio11" src="https://github.com/user-attachments/assets/e7512fc9-72aa-4d75-aa6d-9ef324460917" />

Exemple de visualisation générée à partir du champ full_name :

<img width="455" alt="looker_studio12" src="https://github.com/user-attachments/assets/e80e7969-7722-4c42-b0c0-0ec1eb4e6a82" />

<img width="959" alt="looker_studio13" src="https://github.com/user-attachments/assets/57b6fb6f-d6e3-4bba-a8e5-39cbba199d52" />
