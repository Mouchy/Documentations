Symfony fosOAuthServerBundle
############################

fosOAuthServerBundle
********************

Download the Bundle ::

 php composer.phar require friendsofsymfony/oauth-server-bundle


Enable the Bundle ::

 // app/AppKernel.php
 class AppKernel extends Kernel
 {
    public function registerBundles()
    {
        $bundles = array(
            // ...
           new FOS\OAuthServerBundle\FOSOAuthServerBundle(),
        );

        // ...
    }
 }
 
fosOAuthServerBundle utilisation
********************************

J'ai utilisé le site ci-dessous qui est la continuité des autres tutorial mais qui commence à date 
https://bitgandtter.wordpress.com/2015/09/10/symfony-a-restful-app-security-fosoauthserverbundle/

Le code proposé par le site pour le fichier app/config.yml est obsolète il faut remplacer le code ::

 service:
        user_provider: fos_user.user_manager

par ::

	service:
        user_provider: fos_user.user_provider.username

Tout au long des tutoriels j'utilise les annotations pour la partie entity (Base de donnée) et non pas des fichiers de configuration Yaml ce qui change sensiblement les fichiers client.php, AccesToken.php, AuthCode.php, RefreshToken.php

Je me suis aussi appuyé sur ce site pour la comprehension et le code de Oauth2 en croisant le code avec le code du sit précédent j'ai reussi à venir à bout du bundle.
https://causeyourestuck.io/2016/07/19/oauth2-explained-part-2-setting-up-oauth2-with-symfony2-using-fosoauthserverbundle/

Pour tester ::

 curl -X GET -H "Accept:application/json" https://snowyday-man.c9users.io/web/app_dev.php/api/customers 
 et on obtient
 {"error":"access_denied","error_description":"OAuth2 authentication required"}
 