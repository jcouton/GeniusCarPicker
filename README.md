# GeniusCarPicker

## Présentation

### Architecture

L'application est composée de trois modules : 

![archi](https://raw.githubusercontent.com/menaren/GeniusCarPickerDoc/master/Presentation%20projet/diagram/archi.PNG?token=AAKQDGJ252B5ORHTDURUAB25TWGDY "Architecture")


### Pré-requis

L'hébergement de l'application Genius Car Picker doit se faire sur un serveur Linux x64 (idéalement centOS).

## Installation

### Docker

Pour des raisons de simplicité, l'application n'est livrée que sous la forme d'images Docker.

Il faut dans un premier temps installer docker sur la machine hôte. 
Pour cela, le plus simple reste de suivre la documentation officielle : https://docs.docker.com/install/

### Application

Commencez par télécharger ce dépôt git via un `git clone`, ou via le bouton télécharger en haut à droite. 

Il est aussi possible d'utiliser `wget` ou `cUrl` : 

```bash
mkdir genius-car-picker && cd genius-car-picker
wget https://raw.githubusercontent.com/menaren/GeniusCarPicker/master/docker-compose.yml
# ou
# curl https://raw.githubusercontent.com/menaren/GeniusCarPicker/master/docker-compose.yml -o docker-compose.yml
```

Une fois le fichier téléchargé, il faut configurer quelques valeurs (voir Configuration)

Pour lancer l'application un simple `docker-compose up -d` suffit.

## Configuration

### Configuration de la base de données

```yaml
db:
  # Basé sur l'image postgresql
  image: postgres
  # Redémarre en cas d'erreur
  restart: always
  environment:
    # Spécification du nom d'utilisateur pour la base de données
    POSTGRES_USER: gcp
    # Le mot de passe de la base de données
    POSTGRES_PASSWORD: myS3cr3tPaww$0rd
```

### Configuration du back-office

```yaml
back:
    image: menaren/genius-car-picker-back:latest
    restart: always
    container_name: genius-car-picker-back
    ports:
      # l'api sera visible sur le port 7000 de la machine hôte
      - 7000:8081
    environment:
      # nom d'utilisateur de la base de données (doit correspondre au paramétrage 'db')
      DB_USERNAME: gcp
      # Mot de passe de la base de données (doit correspondre au paramétrage 'db')
      DB_PASSWORD: myS3cr3tPaww$0rd
```

### Configuration du front-office

```yaml
front:
    image: menaren/genius-car-picker-front:latest
    restart: always
    container_name: genius-car-picker-front
    ports:
      # l'application sera visible sur le port 7001 de la machine hôte
      - 7001:80
```
