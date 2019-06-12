# Déployement de projet Laravel

Objectif: Déployer un projet **Laravel** chez [swisscenter](https://www.swisscenter.com/)

Auteur: Pascal Hurni <phi@cpnv.ch>

Modifications:

 - mai 2018
 - décembre 2017

## Marche à suivre

Dans tous les points suivants, lorsque vous lisez `swisscenter_username` comme étant une chaine de caractère, substituez votre vrai nom d'utilisateur
chez swisscenter, il en va de même pour `projectname` mettez le vrai nom de votre projet.

**ATTENTION**: Il ne faut pas faire la substitution sur les termes commençant par des `:` (deux-points), comme `:swisscenter_username`

### Prérequis

A faire une seule fois sur la/les machines de développement.

 1. Installer ruby

 Attention: sur OSX Mojave (et plus vraisemblalement) il faut downgrader la gem `rbnacl` à la version 4.0.2 
    
 2. Installer capistrano
    `gem install capistrano capistrano-laravel`

### Configurer capistrano pour le projet laravel

 1. Dans le répertoire du projet laravel:
    `cap install`
    
 2. Modifier les fichiers fraichement créés
    
    1.  Modifier `Capfile` en ajoutant après le commentaire `# Include tasks from other gems included in your Gemfile`:
        ```ruby
        require "capistrano/laravel"
        ```
        
    2.  Remplacer `config/deploy/production.rb` par le contenu suivant en adaptant les informations:
        ```ruby
        set :swisscenter_username, "swisscenter_username"
        set :swisscenter_servername, "projectname.mycpnv.ch"
        
        set :repo_url, "git@github.com:CPNV-ES/projectname.git"

        require_relative 'laravel_swisscenter'
        ```
        
    3.  Créer le fichier `config/deploy/laravel_swisscenter.rb` avec ce contenu (similaire pour tous les projets):
        ```ruby
        set :application, fetch(:swisscenter_servername)

        set :deploy_to, "/home/#{fetch(:swisscenter_username)}/#{fetch(:application)}"

        set :use_sudo, false
        set :laravel_set_acl_paths, false
        set :laravel_upload_dotenv_file_on_deploy, false
        set :composer_install_flags, '--no-dev --prefer-dist --no-interaction --optimize-autoloader'

        SSHKit.config.command_map[:composer] = "php -d allow_url_fopen=true #{shared_path.join('composer')}"

        server fetch(:swisscenter_servername), user: fetch(:swisscenter_username), roles: %w{app db web}, ssh_options: {
          keys: ["./config/#{fetch(:swisscenter_username)}_rsa"],
          forward_agent: false,
          auth_methods: %w(publickey)
        }

        before 'composer:run', 'set_php_version'
        after  'composer:run', 'copy_dotenv'
        after  'composer:run', 'laravel:migrate'
        
        # Determine the PHP version chosen in the swisscenter control panel
        task :set_php_version do
          on roles(:all) do
            execute "ls /home/#{fetch(:swisscenter_username)}/.data/#{fetch(:swisscenter_servername)}_php* 2>/dev/null | sed -E 's/.+(php[[:digit:]]+)$/\\1/' >/tmp/.php-cli-version"
          end
        end

        # Copy .env in the current release
        task :copy_dotenv do
          on roles(:all) do
            execute :cp, "#{shared_path}/.env #{release_path}/.env" 
          end
        end

        # Until the `capistrano/laravel` gem is updated, manually remove the `laravel:optimize` task which exists no more from Laravel >=5.5
        Rake::Task['laravel:optimize'].clear_actions rescue nil
        ```
    
 3. Si vous avez une application Laravel >= 5.4, il faut l'adapter pour la version de mysql chez swisscenter

    Modifiez le fichier `app/Providers/AppServiceProvider.php` pour qu'il ressemble à ceci:  
    (Les changements sont montrés par le commentaire `<== Mandatory for production use`)
    
    ```php
    <?php

    namespace App\Providers;

    use Illuminate\Support\ServiceProvider;
    use Illuminate\Support\Facades\Schema;  // <== Mandatory for production use

    class AppServiceProvider extends ServiceProvider
    {
        /**
         * Bootstrap any application services.
         *
         * @return void
         */
        public function boot()
        {
            Schema::defaultStringLength(191);  // <== Mandatory for production use
        }
        
        /**
         * Register any application services.
         *
         * @return void
         */
        public function register()
        {
            //
        }
    }
    ```
    
 4. Créer une paire de clés SSH pour le lien _machine de dév_ -> _swisscenter_

    Depuis le répertoire du projet, lancez cette commande en substituant `swisscenter_username` par le nom du compte sur swisscenter:

    ```bash
    ssh-keygen -t rsa -b 4096 -C "swisscenter_username@projectname.mycpnv.ch" -f config/swisscenter_username_rsa -N '' -m PEM
    ```
    
    **ATTENTION**: Ces clés ne doivent pas être publiées, il faut donc **éviter** de les mettre dans votre repository.
    Pour ceci ajoutez la ligne suivante dans votre `.gitignore`:
    
    ```
    /config/*_rsa*
    ```
    
    Vous pouvez par contre les partager entre les membres du projet.

### Config du serveur chez swisscenter

 1. Depuis la [console](https://apanel.swisscenter.com/login) chez swisscenter:
    
    **Définir la clé SSH pour l'accès distant.**
    
    Cliquez sur l'icône _SSH_ puis copiez le contenu du fichier `config/swisscenter_username_rsa.pub` dans le champs _Clé publique SSH_.
    N'oubliez pas d'activer l'accès en cliquant sur _Actif_ dans le champs _Statut de l'accès SSH_.
    
 2. Connectez-vous en SSH chez swisscenter:
    
    ```bash
    ssh -i config/myproject_rsa swisscenter_username@projectname.mycpnv.ch
    ```
    
    1.  Créer la paire de clés pour la communication entre le serveur swisscenter et github.
        
        Exécutez la commande suivante:
        ```bash
        ssh-keygen -t rsa -b 4096 -C "deploy@github" -f ~/.ssh/id_rsa -N ''
        ```

    2.  Installer `composer`
    
        ```bash
        cd projectname.mycpnv.ch
        mkdir shared
        cd shared
        wget -O composer https://getcomposer.org/composer.phar
        chmod +x composer
        ```

    3.  Définir le fichier `.env` pour la production
        
        Le plus simple est de copier-coller le contenu de votre `.env` de développement et de l'adapter avec les _credentials_ de swisscenter,
        notamment pour la connexion à la base de donnée.
        
        Editez le fichier `.env` qui **DOIT** se trouver dans votre répertoire `shared`:
        
        ```bash
        cd projectname.mycpnv.ch/shared
        nano .env
        ```
        
        Sauvez le avec `CTRL-X`

### Config sur github

 1. Dans votre repository, cliquez sur _Settings_
 
 2. Dans le menu de gauche, cliquez sur _Deploy keys_
 
 3. Cliquez sur le bouton _Add deploy key_
 
 4. Choisissez un titre (ex: `projectname sur swisscenter`), puis dans le champs _Key_ mettez le contenu du fichier `~/.ssh/id_rsa.pub` du serveur swisscenter.

## Premier déploiement

Il faut effectuer un premier déploiement afin de pouvoir faire un dernier réglage chez swisscenter.

 1. Déployez en tappant la commande suivante depuis votre répertoire de projet sur la machine de développement:

        cap production deploy

 2. Depuis la [console](https://apanel.swisscenter.com/login) chez swisscenter:
    
    **Ajuster le répertoire de base du serveur web.**
    
    Cliquez sur l'icône _Configuration Apache_, dans le champs _Répertoire de base du domaine_ choisissez `/current/public`.
    

## C'est prêt

A chaque fois que vous voulez déployer:

1. Faites `cap production deploy` depuis votre répertoire de projet sur la machine de développement):

2. Depuis la [console](https://apanel.swisscenter.com/login) chez swisscenter, redéfinissez le répertoire de base du serveur web `/current/public`. Cela est illogique car `current` est un lien, mais si vous ne le faites pas, votre site continuera à être servi à partir de la release précédente !

Pour revenir à la version précédente:

    cap production deploy:rollback

Pour lister toutes les tâches de capistrano:

    cap -T

**ENJOY!**

