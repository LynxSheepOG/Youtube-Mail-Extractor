# YouTube Mail Extractor

Un outil d'extraction automatique d'adresses e-mail depuis les descriptions de vidéos YouTube, spécialement conçu pour identifier les créateurs de contenu musical (producteurs de beats, artistes, etc.).

## 📋 Description

Ce projet utilise l'API YouTube pour :
- Rechercher des vidéos basées sur des mots-clés spécifiques (ex: "Luther type beat")
- Filtrer les chaînes selon des critères de qualité (nombre d'abonnés, activité récente)
- Extraire automatiquement les adresses e-mail des descriptions de vidéos
- Stocker les données dans Google Sheets pour éviter les doublons
- Générer une base de données de contacts pour le marketing musical

## 🛠️ Prérequis

### APIs et Services
- **API YouTube Data v3** : Clé API requise
- **Google Sheets API** : Compte de service configuré
- **Google Cloud Project** : Projet avec les APIs activées

### Dépendances Python
```bash
pip install google-api-python-client pandas gspread google-auth python-dotenv
```

## ⚙️ Configuration

### 1. Configuration YouTube API
1. Créez un projet sur [Google Cloud Console](https://console.cloud.google.com/)
2. Activez l'API YouTube Data v3
3. Créez une clé API
4. Créez un fichier `.env` avec :
```
API_KEY=votre_cle_api_youtube
```

### 2. Configuration Google Sheets
1. Créez un compte de service sur Google Cloud
2. Téléchargez le fichier JSON des credentials
3. Renommez-le `credential.json` et placez-le dans le dossier du projet
4. Créez un Google Sheets avec une feuille nommée "Mail adresses"
5. Partagez le sheet avec l'email du compte de service

### 3. Structure du Google Sheets
La feuille "Mail adresses" doit contenir les colonnes :
- Colonne A : Nom de la chaîne
- Colonne B : ID de la chaîne
- Colonne C : Adresse e-mail

## 🚀 Utilisation

### Exemple d'utilisation basique
```python
# Rechercher des vidéos et extraire les emails
videos_id = fetch_videos_id("Luther type beat", max_results=100)
mail_dict = getmail_json(videos_id)

# Exporter vers Google Sheets
result = export_to_excel(mail_dict)
print(result)
```

### Fonctions principales

#### `search_youtube(query, max_results)`
- Recherche des vidéos YouTube par mot-clé
- **Coût** : 100 unités de quota par recherche
- **Paramètres** : 
  - `query` : Terme de recherche
  - `max_results` : Nombre maximum de résultats

#### `fetch_videos_id(query, max_results)`
- Filtre les chaînes selon les critères :
  - Plus de 1000 abonnés
  - Vidéo publiée dans les 2,5 dernières semaines
  - Exclut les chaînes déjà présentes
- **Retourne** : String d'IDs de vidéos

#### `getmail_json(video_ids)`
- Extrait les adresses e-mail des descriptions
- Utilise des expressions régulières pour identifier les emails
- Exclut automatiquement les chaînes VEVO
- **Retourne** : Liste de dictionnaires avec les données extraites

#### `export_to_excel(mail_dict)`
- Exporte les données vers Google Sheets
- Évite les doublons en vérifiant les IDs de chaînes
- **Retourne** : Rapport du nombre d'ajouts

## 📊 Limitations et Coûts

### Quota API YouTube
- **Quota quotidien** : 10 000 unités par jour
- **Coût par recherche** : 100 unités
- **Coût par extraction vidéo** : 1 unité
- **Coût par info chaîne** : 100 unités

### Limites techniques
- Maximum 50 vidéos par requête d'extraction
- Filtrage automatique des chaînes VEVO
- Extraction limitée aux emails présents dans les descriptions

## 📁 Structure du projet

```
Youtube-Mail-Extractor/
├── Shykelz Youtube channel.ipynb  # Notebook principal
├── credential.json                # Credentials Google Sheets
├── .env                          # Variables d'environnement
├── .gitignore                    # Fichiers ignorés par Git
└── README.md                     # Documentation
```

## 🔧 Personnalisation

### Modifier les critères de filtrage
Dans la fonction `fetch_videos_id()`, vous pouvez ajuster :
- Le nombre minimum d'abonnés (actuellement 1000)
- La période d'activité récente (actuellement 2,5 semaines)
- L'ordre de tri des résultats

### Ajouter de nouveaux patterns d'email
Modifiez la regex dans `getmail_json()` :
```python
email_pattern = r'\b[\w.-]+@[a-zA-Z-]+\.[a-zA-Z.]{2,}\b'
```

## ⚠️ Considérations légales

- Respectez les conditions d'utilisation de YouTube
- Conformez-vous au RGPD pour l'utilisation des données personnelles
- Utilisez les données extraites de manière éthique
- Considérez l'opt-in avant tout contact commercial

## 🤝 Contribution

Pour contribuer au projet :
1. Forkez le repository
2. Créez une branche pour votre fonctionnalité
3. Commitez vos changements
4. Proposez une pull request

## 📝 License

Ce projet est destiné à un usage personnel et éducatif. Veuillez respecter les conditions d'utilisation des APIs utilisées. 