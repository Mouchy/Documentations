symfony database
################

Ajout de l’utilisateur manu comme super utilisateur ::

 GRANT ALL PRIVILEGES ON *.* TO manu@localhost
     IDENTIFIED BY 'xevrod2x' WITH GRANT OPTION;

Changer le nom de la base, le user name et le password
Confiuration accés base ::

 app/config/parameters.yml
 port mysql :3309 à controler 
 parameters:
    database_host: 127.0.0.1
    database_port: null
    database_name: snowyday
    database_user: manu
    database_password: xevrod2x

Créer la base soit dans mysql soit avec symphony
On créer ensuite l'entity qui va nous générer la table ::

 php bin/console doctrine:generate:entity

 The entity shortcut name: SDASnowydayrestBundle:Location
 Configure format:annotation

Generation des scripts sql de création de la table pour controler que tout est ok ::
 
 php bin/console doctrine:schema:update --dump-sql

Création de table dans la base ::

 php bin/console doctrine:schema:update --force