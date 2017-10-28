# Configurer PhpStorm sur MacOS

Ce document décrit la procédure de configuration d'un environnement de développement sur **MacOS** afin d'utiliser correctement l'IDE **PhpStorm** ainsi que certains de ses outils (**PHP CodeSniffer,** **PHP Mess Detector**, etc.) pour des fichiers ou projets PHP.

> **Note :**
> La procédure décrite ci-dessous est, au moment où ces lignes sont écrites, valides sur Mac OS X 10.11 (*El Capitan*).
> Elle sera vérifiée et adaptée sur les versions ultérieures de l'OS dans les semaines à venir.

----------
### Installation de PhpStorm
Téléchargez l'IDE PhpStorm depuis l'url suivante : https://www.jetbrains.com/phpstorm/download/#section=mac-version
Installez le logiciel sur votre Mac.

----------
### Installation de PEAR
Dans un terminal, lancez la commande pour savoir si PEAR est déjà installé :
```
// Tester si PEAR est déjà installé
# pear version
```
S'il n'y a aucun résultat à cette commande :
```
// Télécharger le fichier .phar de PEAR
# curl -O  http://pear.php.net/go-pear.phar
# php -d detect_unicode=0 go-pear.phar
```
Appuyez **deux fois** sur la touche “*Entrée*” une fois arrivé sur l’écran de choix d’installation.

Après cela la première commande devrait retourner un résultat, confirmant son installation.
Il ne reste plus qu'à ajouter cette commande au PATH du Mac :
```
// Se placer à la racine de son compte utilisateur
# cd $HOME
// Editer le fichier .bash_profile
# sudo nano .bash_profile
// Ajouter la commande "pear" en saisissant l'instruction suivante au path :
export PATH="/Users/duduc/pear/bin:$PATH"
```
Terminez l'opération en rechargeant la configuration du profil de l'utilisateur par la commande suivante :
```
// Recharger la configuration du profile de l'utilisateur
# source .bash_profile
```
Désormais la commande "pear" est complètement accessible et utilisable.



-------------
### Installation de PHP CodeSniffer
>**Note:**
>Ici nous n'allons **pas installer la version la plus récente** de l'outil PHP CodeSniffer (3.x) mais car il subsiste quelques bugs qui ne sont pas encore corrigés, de ce fait **nous installerons la version 2.9**.

Téléchargez les fichiers .phar des commandes PHP CodeSniffer :
```
// Télécharger les fichiers .phar des commandes phpcs et phpcbf
# curl -OL https://squizlabs.github.io/PHP_CodeSniffer/phpcs.phar
php phpcs.phar -h
# curl -OL https://squizlabs.github.io/PHP_CodeSniffer/phpcbf.phar
php phpcbf.phar -h
```

Installez de PHP CodeSniffer en version 2.9 (sans conflits) :
```
// Installer la version 2.9 de PHP CodeSniffer
# pear install PHP_CodeSniffer-2.9.0
```

Vérifiez l’installation de PHP CodeSniffer 2.9 :
```
// Lister les paquets installés avec PEAR
# pear list
```
Cette commande doit afficher une liste de paquets dont ‘**PHP_CodeSniffer 2.9.0 stable’**.

> **Tip:** Pour désinstaller un paquet PEAR, il suffit d'utiliser la commande suivante avec le nom du paquet en question :
>  *# pear uninstall PHP_CodeSniffer-2.9.0*

#### Installer les règles de codage de Symfony 2
Téléchargez manuellement les règles de codage de Symfony 2 depuis le depot Github suivant : https://github.com/djoos/Symfony-coding-standard/tree/2.x

Créez l’arborescence des répertoires suivants depuis le finder :
```
/usr/share/php/php/CodeSniffer/Standards/
```

Placez le répertoire 'Symfony 2' des règles de codage de Symfony 2 téléchargées précédemment dans le répertoire créé ci-dessus.
Normalement ce répertoire doit contenir : 

 - un répertoire Sniffs/ 
 - un répertoire Tests/ 
 - un fichier ruleset.xml

Ajoutez les règles de codage de Symfony 2 au path de PHP CodeSniffer :
```
// Ajouter les standards Symfony 2 au path de PHP CodeSniffer
# phpcs --config-set installed_paths /usr/share/php/php/CodeSniffer/Standards
```

Vérifiez l’ajout de ces règles :
```
// Vérifier l'ajout des standards Symfony 2
# phpcs -i
```
Cette commande doit retourner quelque chose dans ce genre : “The installed coding standards are MySource, PEAR, PHPCS, PSR1, PSR2, Squiz, Zend & **Symfony 2”**.

> **Note :** 
En exécutant la commande <code># phpcs</code>, il se peut que les warning ci-dessous apparaissent :
<code>Warning: include_once(PHP/CodeSniffer/CLI.php): failed to open stream: No such file or directory in /usr/local/bin/phpcs on line 21</code>
<code>Warning: include_once(): Failed opening 'PHP/CodeSniffer/CLI.php' for inclusion (include_path='.:') in /usr/local/bin/phpcs on line 21</code>
<code>Fatal error: Class 'PHP_CodeSniffer_CLI' not found in /usr/local/bin/phpcs on line 24</code>
Pour les résoudre exécutez les instructions suivantes :
 <code># sudo mkdir -p /Library/Server/Web/Config/php</code>
 <code># sudo touch /Library/Server/Web/Config/php/local.ini</code>
 <code># echo 'include_path = ".:'`pear config-get php_dir`'"' | sudo tee -a /Library/Server/Web/Config/php/local.ini</code>

Testez la commande phpcs de la manière suivante avec un fichier PHP comportant des erreurs évidentes :
```
# phpcs --standard=SYMFONY2 monFichier.php
```
Comparez le résultat obtenu avec celui d'un autre standard de règle de codage :
```
# phpcs --standard=PSR2 monFichier.php
```

-------------
### Installation de PHP Mess Detector
Dans un terminal, ajoutez les channels phpmd et pdepend :
```
// Ajouter les channels phpmd et pdepend
# sudo pear channel-discover pear.phpmd.org
# sudo pear channel-discover pear.pdepend.org
```
On regarde les paquets disponibles pour ces channels :
```
// Rechercher les paquets disponibles pour phpmd et pdepend
# pear remote-list -c phpmd
# pear remote-list -c pdepend
```
Téléchargez PHP Mess Detector et ses dépendances :
```
// Installer PHP Mess Detector et ses dépendances
# sudo pear install pdepend/PHP_Depend
# sudo pear install phpmd/PHP_PMD
```
Vérification que l’installation c’est bien déroulée :
```
// Vérification de l'installation de PHP Mess Detector
# phpmd --version
// Vérification de l'installation de PHP Depend
# pdepend --version
```
>**Note:**
> De base PHP Mess Detector fonctionne avec ses listes de règles de codage, mais on peut éditer nos propres règles dans des fichiers via des générateurs en ligne : http://edorian.github.io/php-coding-standard-generator/#phpmd

-------------
### Installation de Xdebug

-------------
