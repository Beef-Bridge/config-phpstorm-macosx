# Configurer PhpStorm sur MacOS

Ce document décrit la procédure de configuration d'un environnement sur **MacOS** afin d'utiliser correctement l'IDE **PhpStorm** et certains de ses outils (**PHP CodeSniffer,** **PHP Mess Detector**, etc.).

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
// Ajouter la commande "pear" au path en saisissant l'instruction suivante :
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
Télécharger PHP Mess Detector et ses dépendances :
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
