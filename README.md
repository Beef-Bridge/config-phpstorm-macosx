# Configurer PhpStorm sur MacOS

Ce document décrit la procédure de configuration d'un environnement de développement sur **MacOS** afin d'utiliser correctement l'IDE **PhpStorm** ainsi que certains de ses outils (**PHP CodeSniffer,** **PHP Mess Detector**, etc.) pour projets PHP basés sur le framework Syomfony.

> **Note :**
> La procédure décrite ci-dessous est, au moment où ces lignes sont écrites, valides sur Mac OS X 10.11 (*El Capitan*).
> Elle sera vérifiée et adaptée sur les versions ultérieures de l'OS dans les semaines à venir.

----------
## Installation de PhpStorm et des divers outils
### Installation de PhpStorm
Téléchargez l'IDE PhpStorm depuis l'url suivante : https://www.jetbrains.com/phpstorm/download/#section=mac-version
Installez le logiciel sur votre Mac.

### Installation de PEAR
Dans un terminal, lancez la commande suivante pour savoir si PEAR est déjà installé :
```
// Tester si PEAR est déjà installé
# pear version
```
Si la réponse est <code>pear: command not found</code> c'est que ce n'est pas le cas, alors lancez les instructions ci-dessous :
```
// Télécharger le fichier .phar de PEAR
# curl -O  http://pear.php.net/go-pear.phar
# php -d detect_unicode=0 go-pear.phar
```
Appuyez **deux fois** sur la touche “*Entrée*” une fois arrivé sur l’écran de choix d’installation.

Après ça il faut ajouter la commande <code># pear</code> au PATH du Mac : 
```
// Se placer à la racine de son compte utilisateur
# cd $HOME
// Editer le fichier .bash_profile
# sudo nano .bash_profile
// Ajouter la commande "pear" au PATH en saisissant l'instruction suivante :
export PATH="/Users/duduc/pear/bin:$PATH"
```
Terminez la modification par <code>ctrl + X</code> pour quitter en enregistrant la modification du fichier.

Il ne reste plus qu'à recharger la configuration du profil de l'utilisateur avec la commande suivante :
```
// Recharger la configuration du profile de l'utilisateur
# source .bash_profile
```
Puis à re-tester la première commande qui dorénavant doit retourner quelque chose dans ce genre là <code>PEAR version 1.10.5</code> confirmant ainsi sa parfaite installation.

Désormais la commande <code># pear</code> est complètement accessible et utilisable.

### Installation de PHP CodeSniffer
>**Note :**
>Ici nous n'allons **pas installer la version la plus récente** de l'outil PHP CodeSniffer (3.x) car il subsiste quelques bugs qui ne sont pas encore corrigés, de ce fait **nous installerons la version 2.9**.

Téléchargez les fichiers .phar des commandes utiles à PHP CodeSniffer :
```
// Télécharger les fichiers .phar des commandes phpcs et phpcbf
# curl -OL https://squizlabs.github.io/PHP_CodeSniffer/phpcs.phar
# php phpcs.phar -h
# curl -OL https://squizlabs.github.io/PHP_CodeSniffer/phpcbf.phar
# php phpcbf.phar -h
```

Installez PHP CodeSniffer en version 2.9 :
```
// Installer la version 2.9 de PHP CodeSniffer
# pear install PHP_CodeSniffer-2.9.0
```

Vérifiez que PEAR a récupéré et installé le bon paquet :
```
// Lister les paquets installés avec PEAR
# pear list
```
Cette commande doit afficher une liste de paquets contenant <code>PHP_CodeSniffer 2.9.0 stable</code> confirmant ainsi son installation.

> **Tip :**
> Pour désinstaller un paquet PEAR, il suffit d'utiliser la commande suivante avec le nom du paquet en question :
>  <code># pear uninstall PHP_CodeSniffer-2.9.0</code>

Assurez-vous que la commande <code># phpcs</code> fonctionne correctement :
```
// Vérifiez le fonctionnement de la commande "phpcs"
# phpcs
```
> **Note :** 
En exécutant la commande <code># phpcs</code>, il se peut que les warning ci-dessous apparaissent empêchant ainsi son fonctionnement :
<code>Warning: include_once(PHP/CodeSniffer/CLI.php): failed to open stream: No such file or directory in /usr/local/bin/phpcs on line 21</code>
<code>Warning: include_once(): Failed opening 'PHP/CodeSniffer/CLI.php' for inclusion (include_path='.:') in /usr/local/bin/phpcs on line 21</code>
<code>Fatal error: Class 'PHP_CodeSniffer_CLI' not found in /usr/local/bin/phpcs on line 24</code>
<br>Pour les résoudre sur **MacOS 10.11** exécutez les instructions suivantes :
 <code># sudo mkdir -p /Library/Server/Web/Config/php</code>
 <code># sudo touch /Library/Server/Web/Config/php/local.ini</code>
 <code># echo 'include_path = ".:'`pear config-get php_dir`'"' | sudo tee -a /Library/Server/Web/Config/php/local.ini</code>
 <br>Pour les résoudre sur **MacOS 10.13** exécutez l'instruction suivante :
 <code># sudo touch /etc/php.ini</code>
 <code># sudo nano /etc/php.ini</code>
 <code>inlude_path = "/Users/duduc/pear/share/pear/"</code>
 Terminez la modification par <code>ctrl + X</code> pour quitter en enregistrant la modification du fichier.

Désormais la commande <code># phpcs -i</code> liste les standards de codage pris en compte dans PHP CodeSniffer : <code>The installed coding standards are PEAR, PHPCS, Zend, PSR2, MySource, Squiz and PSR1</code>.

#### Installer les règles de codage de Symfony 2
Téléchargez manuellement les règles de codage de Symfony 2 depuis le depot Github suivant : https://github.com/djoos/Symfony-coding-standard/tree/2.x

Créez l’arborescence des répertoires nécéssaire depuis le finder :
```
/usr/share/php/php/CodeSniffer/Standards/
```

Placez le répertoire <code>Symfony2</code>, présent dans le fichier .zip téléchargé précédemment, dans le répertoire créé ci-dessus.
Vérifiez que ce répertoire contient les éléments listés ci-dessous : 

 - un répertoire <code>Sniffs/</code>
 - un répertoire <code>Tests/</code>
 - un fichier <code>ruleset.xml</code>

Ajoutez les règles de codage de Symfony 2 au PATH de PHP CodeSniffer :
```
// Ajouter les standards Symfony 2 au PATH de PHP CodeSniffer
# sudo phpcs --config-set installed_paths /usr/share/php/php/CodeSniffer/Standards
```

Vérifiez l’ajout de ces règles :
```
// Vérifier l'ajout des standards Symfony 2
# phpcs -i
```
Maintenant cette commande retourne la même liste que précédemment mais en incluant nos règles de codage Symfony 2 : <code>The installed coding standards are MySource, PEAR, PHPCS, PSR1, PSR2, Squiz, Zend & **Symfony2**</code> confirmant ainsi son installation.

Testez la commande <code># phpcs</code> de la manière suivante avec un fichier PHP comportant des erreurs évidentes :
```
# phpcs --standard=SYMFONY2 monFichier.php
```
Comparez le résultat obtenu avec celui d'un autre standard de règle de codage :
```
# phpcs --standard=PSR2 monFichier.php
```

### Installation de PHP Mess Detector
Dans un terminal, ajoutez les channels nécessaires à phpmd et à pdepend :
```
// Ajouter les channels phpmd et pdepend
# sudo pear channel-discover pear.phpmd.org
# sudo pear channel-discover pear.pdepend.org
```
On regarde les paquets disponibles pour chacun de ces channels :
```
// Rechercher les paquets disponibles pour phpmd et pdepend
# pear remote-list -c phpmd
# pear remote-list -c pdepend
```
Installez PHP Mess Detector et ses dépendances :
```
// Installer PHP Mess Detector et ses dépendances
# sudo pear install pdepend/PHP_Depend
# sudo pear install phpmd/PHP_PMD
```
Vérification que tout a bien été installé correctement :
```
// Vérification de l'installation de PHP Mess Detector
# phpmd --version
// Vérification de l'installation de PHP Depend
# pdepend --version
```
>**Note :**
> De base PHP Mess Detector fonctionne avec ses listes de règles de codage, mais on peut éditer nos propres règles dans des fichiers via des générateurs en ligne : http://edorian.github.io/php-coding-standard-generator/#phpmd

### Installation de Xdebug
>**Note :**
>Installez préalablement l'application Xcode sur votre Mac depuis l'App Store.
