# Flask Mailer

[![Python Version](https://img.shields.io/badge/python-3.7+-blue.svg)](https://python.org)
[![Flask Version](https://img.shields.io/badge/flask-2.3.3-green.svg)](https://flask.palletsprojects.com/)
[![License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)

Une application web élégante et simple pour envoyer des emails via un formulaire de contact, construite avec Flask et Flask-Mail.

## Fonctionnalités

- Formulaire de contact moderne et responsive
- Envoi d'emails sécurisé via SMTP
- Support de l'authentification Gmail avec mots de passe d'application
- Interface utilisateur soignée avec CSS intégré
- Configuration facile via variables d'environnement

## Installation Rapide

### Prérequis

- Python 3.7 ou supérieur
- Un compte Gmail avec l'authentification à deux facteurs activée

### 1. Cloner le projet

```bash
git clone https://github.com/dadal560/flask-mailer.git
cd flask-mailer
```

### 2. Environnement virtuel

```bash
# Créer l'environnement virtuel
python -m venv venv

# L'activer
# Sur Linux/Mac :
source venv/bin/activate
# Sur Windows :
venv\Scripts\activate
```

### 3. Installer les dépendances

```bash
pip install -r requirements.txt
```

### 4. Configuration

Créez un fichier `.env` à la racine du projet :

```env
FLASK_SECRET_KEY=votre_clé_secrète_très_longue_et_sécurisée_ici
MAIL_USERNAME=votre.email@gmail.com
MAIL_PASSWORD=votre_mot_de_passe_application_gmail
```

### 5. Lancer l'application

```bash
python app.py
```

Votre application est maintenant accessible sur `http://127.0.0.1:5000`

## Configuration Gmail

Pour utiliser Gmail comme serveur SMTP :

1. **Activez l'authentification à deux facteurs** sur votre compte Google
2. **Générez un mot de passe d'application** :
   - Accédez à [myaccount.google.com](https://myaccount.google.com)
   - Allez dans **Sécurité** → **Mots de passe des applications**
   - Sélectionnez **Autre (nom personnalisé)** et tapez "Flask Mailer"
   - Copiez le mot de passe généré (16 caractères) dans votre fichier `.env`

## Structure du Projet

```
flask-mailer/
├── app.py              # Application Flask principale
├── templates/
│   └── index.html      # Template du formulaire de contact
├── .env               # Variables d'environnement (à créer)
├── requirements.txt   # Dépendances Python
├── README.md         # Cette documentation
└── .gitignore        # Fichiers à ignorer par Git
```

## Dépendances

```txt
Flask==2.3.3
Flask-Mail==0.9.1
python-dotenv==1.0.0
```

## Configuration Avancée

### Utiliser d'autres fournisseurs d'email

Modifiez les paramètres SMTP dans `app.py` :

#### Outlook/Hotmail
```python
app.config.update(
    MAIL_SERVER='smtp.live.com',
    MAIL_PORT=587,
    MAIL_USE_TLS=True,
    MAIL_USE_SSL=False,
    MAIL_USERNAME=os.getenv('MAIL_USERNAME'),
    MAIL_PASSWORD=os.getenv('MAIL_PASSWORD')
)
```

#### Yahoo Mail
```python
app.config.update(
    MAIL_SERVER='smtp.mail.yahoo.com',
    MAIL_PORT=587,
    MAIL_USE_TLS=True,
    MAIL_USE_SSL=False,
    MAIL_USERNAME=os.getenv('MAIL_USERNAME'),
    MAIL_PASSWORD=os.getenv('MAIL_PASSWORD')
)
```

### Variables d'environnement

| Variable | Description | Exemple |
|----------|-------------|---------|
| `FLASK_SECRET_KEY` | Clé secrète Flask (générez-en une unique) | `ma-super-clé-secrète-2024` |
| `MAIL_USERNAME` | Adresse email expéditrice | `contact@monsite.com` |
| `MAIL_PASSWORD` | Mot de passe d'application | `abcd efgh ijkl mnop` |

## Personnalisation

### Modifier l'apparence

Le CSS se trouve dans `templates/index.html`. Vous pouvez personnaliser :
- **Couleurs** : Modifiez les valeurs dans les propriétés `background`, `color`, etc.
- **Police** : Changez `font-family` pour utiliser une autre police
- **Layout** : Ajustez les `padding`, `margin`, `width` pour l'espacement

### Personnaliser le contenu des emails

Dans `app.py`, modifiez le template du message :

```python
body = f"""
Nouveau message depuis votre site web

Expéditeur : {client_email}
Date : {datetime.now().strftime('%d/%m/%Y à %H:%M')}
IP : {request.remote_addr}

Message :
{client_message}

---
Envoyé automatiquement par Flask Mailer
"""
```

## Déploiement

### Heroku

1. Créez un `Procfile` :
```
web: python app.py
```

2. Configurez les variables d'environnement :
```bash
heroku config:set FLASK_SECRET_KEY="votre-clé-secrète"
heroku config:set MAIL_USERNAME="votre-email@gmail.com"
heroku config:set MAIL_PASSWORD="votre-mot-de-passe-app"
```

3. Déployez :
```bash
git add .
git commit -m "Initial deployment"
heroku create votre-app-name
git push heroku main
```

### Railway / Render

Ces plateformes supportent également Flask. Consultez leur documentation respective pour les instructions de déploiement.

## Dépannage

### Problèmes courants

**Erreur d'authentification Gmail**
```
Solution : Vérifiez que vous utilisez un mot de passe d'application et non votre mot de passe principal
```

**Variables d'environnement non trouvées**
```
Solution : Vérifiez que le fichier .env existe et contient toutes les variables requises
```

**Port 5000 déjà utilisé**
```python
# Dans app.py, changez le port :
app.run(host='0.0.0.0', port=5001, debug=False)
```

### Mode débogage

Pour diagnostiquer les problèmes, activez le mode debug :

```python
app.run(debug=True)
```

**Important** : Désactivez le mode debug en production !

## Sécurité

- Ne commitez jamais votre fichier `.env`
- Utilisez des clés secrètes uniques et complexes
- Activez HTTPS en production
- Validez et sanitisez les entrées utilisateur

## Contribution

Les contributions sont les bienvenues ! Pour contribuer :

1. **Fork** ce repository
2. Créez votre branche (`git checkout -b feature/nouvelle-fonctionnalité`)
3. Commitez vos changements (`git commit -am 'Ajoute nouvelle fonctionnalité'`)
4. Push vers la branche (`git push origin feature/nouvelle-fonctionnalité`)
5. Ouvrez une Pull Request

## Support

Besoin d'aide ? Plusieurs options :

- **Email** : [gwen.henry56@gmail.com](mailto:gwen.henry56@gmail.com)
- **Issues** : [GitHub Issues](https://github.com/dadal560/flask-mailer/issues)
- **Discussions** : [GitHub Discussions](https://github.com/dadal560/flask-mailer/discussions)

## Licence

Ce projet est sous licence MIT. Voir le fichier `LICENSE` pour plus de détails.

⭐ Si ce projet vous a été utile, n'hésitez pas à lui donner une étoile !
