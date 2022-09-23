# ontologie-rda-fr-docker

[![Docker Pulls](https://img.shields.io/docker/pulls/abesesr/ontologie-rda-fr.svg)](https://hub.docker.com/r/abesesr/ontologie-rda-fr/)

Configuration docker 🐳 pour déployer [l'ontologie rda-fr](https://github.com/transition-bibliographique/ontologie-rda-fr) sur le site https://rdafr.fr (travail en cours)


## URLs de ontologie-rda-fr

Les URLs correspondantes aux déploiements en local, dev, test et prod sont les suivantes :

- local : http://127.0.0.1:13080/
- test : http://diplotaxis1-dev.v212.abes.fr:13080/ => mappée sur https://dev.rdafr.fr
- test : http://diplotaxis1-test.v202.abes.fr:13080/ => mappée sur https://test.rdafr.fr
- prod : http://diplotaxis1-prod.v102.abes.fr:13080/ => mappée sur https://rdafr.fr


## Prérequis

Disposer de :
- ``docker``
- ``docker-compose``

## Installation

Déployer la configuration docker dans un répertoire :
```bash
# adaptez /opt/pod/ avec l'emplacement où vous souhaitez déployer l'application
cd /opt/pod/
git clone https://github.com/abes-esr/ontologie-rda-fr-docker.git
```

Configurer l'application depuis l'exemple du [fichier ``.env-dist``](./.env-dist) (ce fichier contient la liste des variables avec des explications et des exemples de valeurs) :
```bash
cd /opt/pod/ontologie-rda-fr-docker/
cp .env-dist .env
# personnaliser alors le contenu du .env
```

Démarrer l'application :
```bash
cd /opt/pod/ontologie-rda-fr-docker/
docker-compose up -d
```

## Démarrage et arrêt

```bash
# pour démarrer l'application
cd /opt/pod/ontologie-rda-fr-docker/
docker-compose up -d
```

Remarque : retirer le ``-d`` pour voir passer les logs dans le terminal et utiliser alors CTRL+C pour stopper l'application

```bash
# pour stopper l'application
cd /opt/pod/ontologie-rda-fr-docker/
docker-compose stop


# pour redémarrer l'application
cd /opt/pod/ontologie-rda-fr-docker/
docker-compose restart
```

## Supervision

```bash
# pour visualiser les logs de l'appli
cd /opt/pod/ontologie-rda-fr-docker/
docker-compose logs -f --tail=100
```

Cela va afficher les 100 dernière lignes de logs générées par l'application et toutes les suivantes jusqu'au CTRL+C qui stoppera l'affichage temps réel des logs.

## Sauvegardes

Il est uniquement nécessaire de sauvegarder le fichier ``.env`` qui contient la configuration de l'application et qui serait utile pour la restaurer.

Il n'est pas nécessaire de sauvegarder autre chose car c'est un site web qui ne dispose pas de base de données et dont le contenu est auto-généré depuis github. Pour restaurer l'application, il suffit donc de relancer l'installation depuis zéro en se basant sur le .env depuis les dernières sauvegardes.

## Architecture

1) L'ontologie RDA-FR est conçue sur le github suivant https://github.com/transition-bibliographique/ontologie-rda-fr : elle contient un fichier README.md et (plus tard) des fichiers OWL
2) Lorsque le fichier [README.md](https://github.com/transition-bibliographique/ontologie-rda-fr/blob/develop/README.md) est modifié, une [github action](https://github.com/transition-bibliographique/ontologie-rda-fr/actions/workflows/build-pub-rdafr.fr-docker-image.yml) détecte la modification et va générer et publier automatiquement une nouvelle version du site web https://rdafr.fr
3) Le serveur où est hébergé https://rdafr.fr utilise ce présent dépôt pour savoir quel conteneur docker déployer. Il va alors détecter qu'une nouvelle version de l'image docker est disponible (à l'aide de l'outil [watchtower](https://containrrr.dev/watchtower/)) et va l'auto-déployer sur https://rdafr.fr

D'un point de vue utiliseur le workflow pour mettre à jour https://rdafr.fr consiste uniquement à modifier le contenu de https://github.com/transition-bibliographique/ontologie-rda-fr et à attendre quelques minute le déploiement automatique sur https://rdafr.fr
