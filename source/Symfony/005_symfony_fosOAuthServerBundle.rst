Symfony fosOAuthServerBundle
############################

OAuth2 principe
***************

OAuth Roles
===========

`lien`_

.. _lien: https://www.digitalocean.com/community/tutorials/an-introduction-to-oauth-2


OAuth defines four roles:

* Resource Owner
* Client
* Resource Server
* Authorization Server



**Resource Owner: User**

The resource owner is the user who authorizes an application to access their account. 
The application's access to the user's account is limited to the "scope" of the authorization granted (e.g. read or write access).

**Resource/Authorization Server: API**

The resource server hosts the protected user accounts, and the authorization server verifies the identity of the user then issues access tokens to the application.

From an application developer's point of view, a service's API fulfills both the resource and authorization server roles. We will refer to both of these roles combined, as the Service or API role.

**Client: Application**

The client is the application that wants to access the user's account. Before it may do so, it must be authorized by the user, and the authorization must be validated by the API.

**Abstract Protocol Flow**

.. image:: images/abstract_flow.png


1. The application requests authorization to access service resources from the user
#. If the user authorized the request, the application receives an authorization grant
#. The application requests an access token from the authorization server (API) by presenting authentication of its own identity, and the authorization grant
#. If the application identity is authenticated and the authorization grant is valid, the authorization server (API) issues an access token to the application. Authorization is complete.
#. The application requests the resource from the resource server (API) and presents the access token for authentication
#. If the access token is valid, the resource server (API) serves the resource to the application



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

Grant type Authorization code
=============================

Commande à saisir ::

 php bin/console oauth:client:create client1 www.client1.com authorization_code 
 
Reponse ::

 CreateOAuthClientCommand::configureCreateOAuthClientCommandThe client client1 was created with 33_5uiis8fsaa04kgk0848wscg8kwg804wgocso88wk8cscswgksk as public id and 3apu4xhbnuckwoso8sw8k4os80sg8ocokc0k0g80wos8kk40k8 as secret
 Customer 586246a9201fe linked to client client1
 Customer 586246cd083b7 linked to client client1
 

cela me créer plusieurs ligne dans le client, autant de lignes que d'utilisateurs enregistrés dans la table user du module FOSUser.

Table client
+----+----------------------------------------------------+-----------------------------------+----------------------------------------------------+--------------------------------------+---------+
| id | random_id                                          | redirect_uris                     | secret                                             | allowed_grant_types                  | name    |
+----+----------------------------------------------------+-----------------------------------+----------------------------------------------------+--------------------------------------+---------+
| 33 | 5uiis8fsaa04kgk0848wscg8kwg804wgocso88wk8cscswgksk | a:1:{i:0;s:15:"www.client1.com";} | 3apu4xhbnuckwoso8sw8k4os80sg8ocokc0k0g80wos8kk40k8 | a:1:{i:0;s:8:"password";}            | client1 |
+----+----------------------------------------------------+-----------------------------------+----------------------------------------------------+--------------------------------------+---------+

Table auth_code
+----+-----------+-------------+----------------------------------------------------------------------------------------+-----------------+------------+----------+
| id | client_id | customer_id | token                                                                                  | redirect_uri    | expires_at | scope    |
+----+-----------+-------------+----------------------------------------------------------------------------------------+-----------------+------------+----------+
| 36 |        33 | NULL        | Yzk4ZTkwNjhhODk1ZmQ1MmRkOTlmZWM2YzVlMTU5MzI5YjFmZDBkYWRhMTNmZGIzN2M0YjBlZDMwZDQ0ZmU0Mg | www.client1.com | 1483983403 | password |
| 37 |        33 | NULL        | MWJiYmE2MzZlMzE1ZWZhZTFjZjc0OTlmNWFlMmFhNDZhMzdmYWQ3MzNkYjhiYTg4YTU5YjllN2JmYTgwZWFmMg | www.client1.com | 1483983403 | password |
+----+-----------+-------------+----------------------------------------------------------------------------------------+-----------------+------------+----------+


Cela génére un token (code), dans la table auth_code, qui a une durée de validité assez courte ce qui ne permet pas de l'utliser lorsque l'on est dans un traitement manuel. 
j'ai donc ajouté les durées de validité du token comme ci-dessous ::

 //app/config.yml
 fos_oauth_server:
    service:
	    options:
            # Changing tokens and authcode lifetime
            access_token_lifetime: 3600
            refresh_token_lifetime: 1209600
            auth_code_lifetime: 3600


Commande à saisir ::

 https://snowyday-man.c9users.io/web/app_dev.php/oauth/v2/token?client_id=33_5uiis8fsaa04kgk0848wscg8kwg804wgocso88wk8cscswgksk&client_secret=3apu4xhbnuckwoso8sw8k4os80sg8ocokc0k0g80wos8kk40k8&grant_type=authorization_code&redirect_uri=www.client1.com&code=OGEyOGI3ODcxNjkyYjMwOWUxYTcyOGJlYTcwMGM0YWUxOGY3MzgyMDI4YzljZGQzYjExYTg1NGQzYzhkM2Y3MA

ensuite aprés avoir lancé la requète http ci-dessus cela me génére un token dans la table access_token et un autre token dans la table refresh_token



Version avec login/password
===========================

Commande à saisir ::

 php bin/console oauth:client:create client1 www.client1.com password

Reponse ::

 CreateOAuthClientCommand::configureCreateOAuthClientCommandThe client client1 was created with 32_2gks0pya85z4wwg888wskowwow0k4koock4wckww8w0ossk0co as 
 public id and br9en9hy0j4sggcc800c0gwsc4cww8go0o4sos0cogksw4s8c as secret
 Customer 586246a9201fe linked to client client1
 Customer 586246cd083b7 linked to client client1

Commande à saisir ::

 https://snowyday-man.c9users.io/web/app_dev.php/oauth/v2/token?client_id=32_2gks0pya85z4wwg888wskowwow0k4koock4wckww8w0ossk0co&client_secret=br9en9hy0j4sggcc800c0gwsc4cww8go0o4sos0cogksw4s8c&grant_type=password&redirect_uri=www.client1.com&username=manu&password=xevrod2x

Reponse ::

 {"access_token":"ZGMwZmRlM2M5MjQzMzI4MWRhYmU2NjdkMzk2ZjBjZjAwZGZjNzNmZmEyMjc4YzU0M2ZmNjBhYjI0NmE3NDljMw","expires_in":3600,"token_type":"bearer","scope":"authorization_code","refresh_token":"N2ZiOGY2MWRkZTMzYTNiYWE0MDMwNjdmNjBlYzJjZDEyMDk2YWU3MmMwNWMxN2NkZmEwY2E1ZDMzNzNmZDg3Mg"}

Commande à saisir ::
 
 Pour l'instant ce n'est pas la bonne commande je cherche encore
 https://snowyday-man.c9users.io/web/app_dev.php/api/customers#access_token=ZGMwZmRlM2M5MjQzMzI4MWRhYmU2NjdkMzk2ZjBjZjAwZGZjNzNmZmEyMjc4YzU0M2ZmNjBhYjI0NmE3NDljMw&expires_in=3600&token_type=bearer&refresh_token=N2ZiOGY2MWRkZTMzYTNiYWE0MDMwNjdmNjBlYzJjZDEyMDk2YWU3MmMwNWMxN2NkZmEwY2E1ZDMzNzNmZDg3Mg

La même commande mais avec curl ::
 
 curl -i https://snowyday-man.c9users.io/web/app_dev.php/api/customers -H "Authorization: Bearer ZGMwZmRlM2M5MjQzMzI4MWRhYmU2NjdkMzk2ZjBjZjAwZGZjNzNmZmEyMjc4YzU0M2ZmNjBhYjI0NmE3NDljMw"				
 
Reponse ::

 HTTP/1.1 200 OK
 date: Mon, 09 Jan 2017 16:39:34 GMT
 server: Apache/2.4.7 (Ubuntu)
 vary: Authorization
 cache-control: no-cache, private
 allow: GET, POST
 x-debug-token: bd5464
 x-debug-token-link: http://snowyday-man.c9users.io/web/app_dev.php/_profiler/bd5464
 content-length: 731
 keep-alive: timeout=5, max=100
 content-type: application/json
 X-BACKEND: apps-proxy

 CustomersController::cgetAction[
 {"id":"586246a9201fe",
  "username":"manu",
  "username_canonical":"manu",
  "email":"edelobre@yahoo.com",
  "email_canonical":"edelobre@yahoo.com",
  "enabled":true,
  "password":"$2y$13$ENnRUZDAJYP7TQ4peSwXuuOrP5cnbCC3wZRw5xO55iOxudbFJsOp.",
  "roles":[],
  "_links":{"self":{"href":"\/web\/app_dev.php\/api\/customers\/586246a9201fe"},
  "customers":{"href":"\/web\/app_dev.php\/api\/customers"}}},
 {"id":"586246cd083b7",
  "username":"yasmany",
  "username_canonical":"yasmany",
  "email":"yasmanycm@gmail.com",
  "email_canonical":"yasmanycm@gmail.com",
  "enabled":false,
  "password":"ok",
  "roles":[],
  "_links":{"self":{"href":"\/web\/app_dev.php\/api\/customers\/586246cd083b7"},
  "customers":{"href":"\/web\/app_dev.php\/api\/customers"}}}
 
 
 