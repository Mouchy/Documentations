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

 Dans les routes on rajoute le paramétre préfix avec comme valeur api. Dans la définition de l'adrresse http utilisé on doit donc maintenant rajouter api.
 
 curl -X GET -H "Accept:application/json" https://snowyday-man.c9users.io/web/app_dev.php/api/customers 
 et on obtient
 {"error":"access_denied","error_description":"OAuth2 authentication required"}
 
Ici le tutoriel de bitgandtter est devenu un brin compliqué pour moi. Je ne suis pas encore sur d'avoir tout saisie.
Pour l'instant je n'ai pas reussi avec le grant_type password/credential mais uniquement avec le grant_type code.

 Commande à saisir ::

 php bin/console oauth:client:create client1 www.client1.com authorization_code 
 
Reponse ::

 The client client1 was created with 15_oyjmcfmv834okk0kwcw4o04gkwg4sc0wwcwwg000kg8c88kwc as public id and 1wvtfgw57ahw4wkw4o4wcgw0wk8kk40kow8s4wkc04go8ks40s as secret
 /oauth/v2/token?client_id=15_oyjmcfmv834okk0kwcw4o04gkwg4sc0wwcwwg000kg8c88kwc&
                client_secret=1wvtfgw57ahw4wkw4o4wcgw0wk8kk40kow8s4wkc04go8ks40s&grant_type=password&redirect_uri=www.client1.com&username=fabien&password=fabien

cela me créer plusieurs ligne dans le client, autant de lignes que d'utilisateurs enregistrés dans la table user du module FOSUser.

A finir
https://snowyday-man.c9users.io/web/app_dev.php/oauth/v2/token?client_id=29_5tokgp06mj4800co88ssc0cgscs0kkk0480wssswsog8k8cks8&client_secret=5r1y9h039ncwc4848kk80kw0gw80o40k4gcwgg00os4www0s44&grant_type=authorization_code&redirect_uri=www.client1.com&code=OTI0YzM5Yjk1ZGMwMzdjOTk2NzMzYzg2YWU5NmE4NTdjOWRmMWMzY2JjYjM5NWFlNDJmMmZhNjRkYTY3NGJhMA


				
 
 
 
 
 
 