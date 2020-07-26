# docker-symfony4  

----------------  

## Description :  

Stack Docker pour Symfony4   
Version : 1.0.0    
Date : 2020-07-26   
Responsable : HI   
Requirements : Symfony 4, Docker and Docker-compose  
Compatibilité : Symfony CLI version v4.17.2 minimum, mysql 5.7, PHP 7  

----------------  

## Mise en oeuvre :  

1/ Clonez le répertoire et placez votre projet Symfony à la racine de celui-ci (si le projet n'existe pas encore il est possible de le créer par la suite, cf: Pour Installer Symfony étape 6/ ).   

2/ Modifiez si besoin les valeurs des variables d'environement du fichier .env avec vos propres valeurs ainsi que le .gitignore.   

3/ Ouvrir un terminal à la racine du projet (où se situe le fichier docker-compose.yml) puis tapez la ligne de commande suivante : ```$ docker-compose up -d```  

4/ Si vous avez déjà un projet existant et que vous l'avez simplement ajouté à la racine de ce répertoire, vous pouvez directement vous rendre à l'étape suivante. Si vous souhaitez créer un nouveau projet, tapez ```$ symfony new [project-name] --full``` toujours à la racine de votre répertoire.  

5/ Modifiez les valeurs de la base de données dans le .env de votre projet Symfony (example : DATABASE_URL=mysql://symfony:symfony@172.19.0.3:3306/symfony?serverVersion=5.7) avec vos propres données (celles rentrées dans le .env global et l'adresse et le port de votre container mysql)  
Pour connaître l'adresse IP du container mysql, tapez ```$ docker container inspect [mysql-nom_de_votre_projet] | grep IPAddress```   
    example de la réponse : "IPAddress": "",  
                        "IPAddress": "172.18.0.2",  

Pour connaître le port du container mysql, tapez ```$ docker container inspect [mysql-nom_de_votre_projet] | grep HostPort```   
    example de la réponse : "HostPort": "3306"  
                        "HostPort": "3306"  

6/ Ouvrir un navigateur et entrez l'url de votre projet dans la barre de recherche : http://localhost:8002/ remplacez 8002 par la valeur de APP_PORT déclarée dans le .env pour accéder à votre projet.  
Pour accéder à PhpMyAdmin, tapez http://localhost:8082/ [remplacez 8082 par la valeur de PHP_MYADMIN_PORT déclarée dans le .env] et renseignez le login avec la valeur de MYSQL_USER et MYSQL_PASSWORD déclarées dans le .env global.  


#### Pour Installer Symfony  

1/ Vérifier que PHP est en version 7.2.5 minimum, sinon le mettre à jour  
2/ Installer composer via https://getcomposer.org/download. Pour les systèmes Linux ou Mac, saisissez en plus dans la console :   
```$ sudo mv composer.phar /usr/local/bin/composer```  
3/ Fermer et rouvrir le terminal et vérifier que Composer est installé : ```$ composer --version```  
4/ Installer le CLI Symfony https://symfony.com/download. Pour Linux et Mac : suivez bien les instructions de terminal dans la section « Install it globally »  
5/ Fermer la console puis relancer et saisir ```$ symfony version``` pour vérifier que Symfony est bien installé  
6/ Ouvrir le terminal dans un dossier de votre choix dans lequel se trouvera les projets Symfony puis créer le projet : ```$ symfony new [project-name] --full```  
7/ Aller DANS le dossier du projet nouvellement créé et saisir : ```$ symfony server:start``` pour vérifier que tout est bien installé, puis passez à l'étape 5 de la mise en oeuvre.  

----------------  
  
## Utilisation :  

La base de données est déjà créée lors du build des containers, il faut simplement ajouter les tables et les colonnes voulues.  

Pour taper les commandes Symfony, se placer à la racine du répertoire et taper la commande suivante : ```$ docker exec -it php-fpm-[nom_de_votre_projet] sh```   

Une fois en sh, on peut utiliser les commandes Symfony pour construire notre application :  
Example :  

* Créer une instance  
    - ```$ php bin/console make:entity EntityName```  

* Créer une migration :   
    // Exécuter les précédentes migrations éventuelles  
    - ```$ php bin/console doctrine:migrations:migrate``` ou ```$ php bin/console d:m:m```  
    // Créer la nouvelle migration 
    - ```$ php bin/console make:migration```   
    // Exécute la nouvelle migration  
    - ```$ php bin/console doctrine:migrations:migrate``` ou ```$ php bin/console d:m:m```  

* Créer une base de données  
    - ```$ php bin/console d:d:c```  

* Créer un form  
    - ```$ php bin/console make:form```  

* Créer un crud  
    - ```$ php bin/console make:crud```  

----------------  

## Rappel des principales commandes Docker :  

### Noms des containers docker  
 mysql-[nom_de_votre_projet]     
 php-fpm-[nom_de_votre_projet]  
 nginx-[nom_de_votre_projet]  
 phpmyadmin-[nom_de_votre_projet]  

* pour stopper les containers :  
    - ```$ docker-compose stop```  

* pour les relancer si déjà buildés :  
    - ```$ docker-compose start```   

* pour lister les containers qui tournent :  
    - ```$ docker-compose ps```  

* pour lister tous les containers :  
    - ```$ docker container ls -la```  

* pour inspecter les containers :  
    - ```$ docker container inspect [nom_du_container]```  

* pour exécuter des commandes arbitraires dans les services.  
    - ```$ docker-compose exec [nom_du_container] sh``` ou ```$ docker exec -it [nom_du_container] sh```  


* pour supprimer les containers et les volumes de données (Arrête les container et supprime les container, réseaux, volumes et images créés par up) :   
    - ```$ docker-compose down --rmi all --remove-orphans -v```  

    * Par défaut, les seules choses supprimées sont :  
        - Container pour les services définis dans le fichier docker-compose.yml  
        - Réseaux définis dans la section network du fichier docker-compose.yml  
        - Le réseau par défaut, s'il est utilisé  

Les réseaux et volumes définis comme externes ne sont jamais supprimés.  

* pour tout supprimer du système une fois les containers stoppés (**faire très attention**, **supprime** du système **TOUS** les container, réseaux, images, volumes inutilisés)  
    - ```$ docker system prune```  

    * Cela supprimera :  
        - tous les container arrêtés  
        - tous les réseaux non utilisés par au moins un conteneur  
        - toutes les images en suspend  
        - tout le cache de construction  

    - ```$ docker system prune -a --volumes```  

    * Cela supprimera :  
        - tous les container arrêtés  
        - tous les réseaux non utilisés par au moins un conteneur  
        - tous les volumes non utilisés par au moins un conteneur  
        - toutes les images sans au moins un conteneur qui leur est associé  
        - tout le cache de construction  


----------------