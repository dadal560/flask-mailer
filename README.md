# Flask mailer

Une application web simple pour envoyer des emails via un formulaire de contact, construite avec Flask et Flask-Mail.

## Installation et Configuration

### Prérequis

![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white) ![Flask](https://img.shields.io/badge/Flask-000000?style=for-the-badge&logo=flask&logoColor=white) ![Gmail](https://img.shields.io/badge/Gmail-D14836?style=for-the-badge&logo=gmail&logoColor=white)

- Python 3.7+
- Un compte Gmail avec mot de passe d'application

### Installation

1. **Clonez le dépôt**
```bash
git clone https://github.com/dadal560/flask-mailer.git
cd flask-mailer
```

2. **Créez un environnement virtuel**
```bash
python -m venv venv
source venv/bin/activate  # Sur Windows: venv\Scripts\activate
```

3. **Installez les dépendances**
```bash
pip install -r requirements.txt
```

4. **Configuration des variables d'environnement**

Créez un fichier `.env` à la racine du projet :

```env
FLASK_SECRET_KEY=votre_clé_secrète_très_longue_et_complexe
MAIL_USERNAME=votre.email@gmail.com
MAIL_PASSWORD=votre_mot_de_passe_application
```

### Configuration Gmail

1. Activez l'authentification à deux facteurs sur votre compte Gmail
2. Générez un mot de passe d'application :
   - Allez dans **Compte Google** → **Sécurité**
   - Cliquez sur **Mots de passe des applications**
   - Sélectionnez **Autre** et nommez-le "Flask App"
   - Utilisez le mot de passe généré dans le fichier `.env`

## Utilisation

### Lancement de l'application

```bash
python app.py
```

L'application sera accessible sur `http://127.0.0.1:5000`

### Structure du projet

```
flask-contact-form/
├── app.py              # Application Flask principale
├── templates/
│   └── index.html      # Template du formulaire
├── .env               # Variables d'environnement (à créer)
├── requirements.txt   # Dépendances Python
└── README.md         # Documentation
```

## Fichier requirements.txt

```txt
Flask==3.0.0
Flask-Mail==0.9.1
Flask-WTF==1.2.1
WTForms==3.1.1
python-dotenv==1.0.0
email-validator==2.1.0
gunicorn==21.2.0
```

## Configuration Avancée

### Personnalisation du serveur SMTP

Pour utiliser un autre fournisseur d'email, modifiez les paramètres dans `app.py` :

```python
# Pour Outlook/Hotmail
app.config.update(
    MAIL_SERVER='smtp.live.com',
    MAIL_PORT=587,
    MAIL_USE_TLS=True,
    # ...
)

# Pour Yahoo
app.config.update(
    MAIL_SERVER='smtp.mail.yahoo.com',
    MAIL_PORT=587,
    MAIL_USE_TLS=True,
    # ...
)
```

### Variables d'environnement

| Variable | Description | Exemple |
|----------|-------------|---------|
| `FLASK_SECRET_KEY` | Clé secrète pour Flask | `your-super-secret-key-here` |
| `MAIL_USERNAME` | Email expéditeur | `votre.email@gmail.com` |
| `MAIL_PASSWORD` | Mot de passe d'application | `abcd efgh ijkl mnop` |


## Personnalisation

### Modification du style

Le CSS est intégré dans le fichier `templates/index.html`. Vous pouvez :

- Modifier les couleurs en changeant les valeurs hexadécimales
- Ajuster les tailles et espacements
- Ajouter des animations CSS

### Personnalisation du message

Dans `app.py`, modifiez le contenu du message :

```python
body=f"""
Nouveau message de contact

Expéditeur: {client_email}
Date: {datetime.now().strftime('%d/%m/%Y %H:%M')}

Message:
{client_message}
"""
```

## Dépannage

### Problèmes courants

**Erreur d'authentification Gmail**
- Vérifiez que l'authentification 2FA est activée
- Utilisez un mot de passe d'application, pas votre mot de passe principal

**Erreur "Variables d'environnement manquantes"**
- Vérifiez que le fichier `.env` existe
- Assurez-vous que toutes les variables sont définies

**Page ne se charge pas**
- Vérifiez que le port 5000 n'est pas utilisé
- Essayez de changer le port dans `app.run(port=5001)`

### Logs de débogage

Activez le mode debug pour plus d'informations :

```python
app.run(debug=True)
```

## Déploiement

### Déploiement avec Gunicorn (Recommandé)

**Installation de Gunicorn**
```bash
pip install gunicorn
```

**Fichiers nécessaires**

**wsgi.py** (point d'entrée) :
```python
from app import app

if __name__ == "__main__":
    app.run()
```

**Commandes de déploiement**

Développement local :
```bash
# Test simple
gunicorn --bind 127.0.0.1:8000 --reload app:app

# Avec logs détaillés  
gunicorn --bind 127.0.0.1:8000 --reload --log-level debug app:app
```

Production :
```bash
# Configuration basique
gunicorn --bind 0.0.0.0:8000 --workers 4 app:app

# Configuration optimisée
gunicorn \
  --bind 0.0.0.0:8000 \
  --workers 4 \
  --timeout 60 \
  --keep-alive 2 \
  --max-requests 1000 \
  --access-logfile access.log \
  --error-logfile error.log \
  app:app

# En arrière-plan
gunicorn --bind 0.0.0.0:8000 --workers 4 --daemon --pid gunicorn.pid app:app
```

**Pourquoi Gunicorn ?**
- ✅ Serveur WSGI robuste pour la production
- ✅ Gestion multi-workers (performance)
- ✅ Redémarrage automatique en cas de crash
- ✅ Logs avancés et monitoring
- ✅ Compatible avec tous les hébergeurs

### Déploiement sur VPS (OVH, DigitalOcean, etc.)

**1. Connexion et setup serveur**
```bash
# Connexion SSH
ssh root@votre-ip-serveur

# Mise à jour système
apt update && apt upgrade -y

# Installation dépendances
apt install python3 python3-pip python3-venv nginx git -y
```

**2. Déploiement application**
```bash
# Clone du projet
git clone https://github.com/dadal560/flask-mailer.git
cd flask-mailer

# Environnement virtuel
python3 -m venv venv
source venv/bin/activate

# Installation dépendances
pip install -r requirements.txt

# Configuration variables d'environnement
cp .env.example .env
nano .env  # Éditer avec vos valeurs
```

**3. Configuration systemd**
Créer `/etc/systemd/system/flask-mailer.service` :
```ini
[Unit]
Description=Flask Mailer avec Gunicorn
After=network.target

[Service]
User=root
WorkingDirectory=/root/flask-mailer
Environment=PATH=/root/flask-mailer/venv/bin
EnvironmentFile=/root/flask-mailer/.env
ExecStart=/root/flask-mailer/venv/bin/gunicorn --workers 3 --bind 127.0.0.1:8000 app:app
Restart=always

[Install]
WantedBy=multi-user.target
```

**4. Configuration Nginx**
Créer `/etc/nginx/sites-available/flask-mailer` :
```nginx
server {
    listen 80;
    server_name votre-domaine.com;
    
    location / {
        proxy_pass http://127.0.0.1:8000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

**5. Activation et démarrage**
```bash
# Service systemd
systemctl daemon-reload
systemctl enable flask-mailer
systemctl start flask-mailer
systemctl status flask-mailer

# Nginx
ln -s /etc/nginx/sites-available/flask-mailer /etc/nginx/sites-enabled/
nginx -t
systemctl reload nginx

# Firewall (optionnel)
ufw allow 22
ufw allow 80
ufw allow 443
ufw enable
```

### Plateformes alternatives
- **Railway** - Simple et gratuit
- **Render** - Excellent pour Flask
- **Fly.io** - Performant
- **Vercel** - Pour applications statiques

## Contribution

Les contributions sont les bienvenues ! N'hésitez pas à :

1. Fork le projet
2. Créer une branche pour votre fonctionnalité
3. Commit vos changements
4. Push vers la branche
5. Ouvrir une Pull Request


## Support

Pour toute question ou problème :

- Email : gwen.henry56@gmail.com
- Issues : [GitHub Issues](https://github.com/dadal560/flask-mailer/issues)

---

⭐ **N'oubliez pas de star le projet si il vous a été utile !**
