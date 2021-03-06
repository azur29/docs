Normes de codes
###############

Les développeurs de Cake vont utiliser les normes de code suivantes.

Il est recommandé que les autres personnes qui développent des IngredientsCake suivent les mêmes normes.

Ajout de nouvelles fonctionnalités
==================================

Aucune nouvelle fonctionnalité ne devrait être ajouté, sans avoir fait ces propres
tests - qui doivent être validés avant de les committer au dépôt.

Indentation
===========

Un onglet sera utilisé pour l'indentation.

Alors, l'indentation devrait ressembler à cela::

    <?php
    // niveau de base
        // niveau 1
            // niveau 2
        // niveau 1
    // niveau de base

Ou::

    <?php
    $booleanVariable = true;
    $stringVariable = "moose";
    if ($booleanVariable) {
        echo "Boolean value is true";
        if ($stringVariable == "moose") {
            echo "We have encountered a moose";
        }
    }

Structures de contrôle
======================

Les structures de controle sont par exemple "``if``", "``for``", "``foreach``",
"``while``", "``switch``" etc. En-dessous, une exemple avec "``if``"::

    <?php 
    if ((expr_1) || (expr_2)) { 
        // action_1;
    } elseif (!(expr_3) && (expr_4)) {
        // action_2; 
    } else {
        // default_action; 
    } 

*  Dans les structures de contrôle, il devrait y avoir 1 (un) espace avant la
   première parenthèse et 1 (un) espace entre les dernières parenthèses et 
   l'accolade ouvrante.    
*  Toujours utiliser des accolades dans les structures de contrôle,
   même si elles ne sont pas nécessaires. Elles augmentent la lisibilité
   du code, et elles vous donnent moins d'erreurs logiques.

*  L'ouverture des accolades doit être placé sur la même ligne que la structure de
   contrôle. La fermeture des accolades doit être placé sur de nouvelles
   lignes, et ils devraient avoir le même niveau d'indentation que la structure de
   contrôle. La déclaration incluse dans les accolades doit commencer sur une
   nouvelle ligne, et le code qu'il contient doit gagner un nouveau niveau 
   d'indentation::

    <?php 
    // wrong = no brackets, badly placed statement
    if (expr) statement; 

    // wrong = no brackets
    if (expr) 
        statement; 

    // good
    if (expr) {
        statement;
    }

Opérateurs ternaires
--------------------

Les opérateurs ternaires sont permis quand l'opération entière rentre sur une ligne.
Les opérateurs ternaires plus longues doivent être séparées en expression ``if else``.
Les opérateurs ternaires ne doivent pas être imbriqués. Des parenthèses optionnelles
peuvent être utilisées autour de la condition vérifiée de l'opération pour clarifier::

    <?php
    //Bien, simple et lisible
    $variable = isset($options['variable']) ? $options['variable'] : true;

    //Imbriquations des ternaires est mauvaise
    $variable = isset($options['variable']) ? isset($options['othervar']) ? true : false : false;

Appels des fonctions
====================

Les fonctions doivent être appelées sans espace entre le nom de la fonction et 
la parenthèse ouvrante. Il doit y avoir un espace entre chaque paramètre d'un appel
de fonction::

    <?php 
    $var = foo($bar, $bar2, $bar3); 

Comme vous pouvez le voir, il devrait y avoir un espace des deux côtés des signes égal (=).

Définition des méthodes
=======================

Exemple d'un définition de fonction::

    <?php 
    function someFunction($arg1, $arg2 = '') {
        if (expr) {
            statement;
        }
        return $var;
    }

Les paramètres avec une valeur par défaut, devrait être placée en dernier dans la défintion
de la fonction. Essayez de faire en sorte que vos fonctions retournent quelque chose, 
au moins true ou false = ainsi cela peut déterminer si l'appel de la fonction est un succès::

    <?php 
    function connection($dns, $persistent = false) {
        if (is_array($dns)) {
            $dnsInfo = $dns;
        } else {
            $dnsInfo = BD::parseDNS($dns);
        }

        if (!($dnsInfo) || !($dnsInfo['phpType'])) {
            return $this->addError();
        }
        return true;
    }

Il y a des espaces des deux côtés du signe égal.

Commenter le code
=================

Tous les commentaires doivent être écrits en anglais,
et doivent clairement décrire le block de code commenté.

Les commentaires doivent inclure les tags `phpDocumentor suivants <http://phpdoc.org>`_:

*  `@access <http://manual.phpdoc.org/HTMLframesConverter/phpdoc.de/phpDocumentor/tutorial_tags.access.pkg.html>`_
*  `@author <http://manual.phpdoc.org/HTMLframesConverter/phpdoc.de/phpDocumentor/tutorial_tags.author.pkg.html>`_
*  `@copyright <http://manual.phpdoc.org/HTMLframesConverter/phpdoc.de/phpDocumentor/tutorial_tags.copyright.pkg.html>`_
*  `@deprecated <http://manual.phpdoc.org/HTMLframesConverter/phpdoc.de/phpDocumentor/tutorial_tags.deprecated.pkg.html>`_
*  `@example <http://manual.phpdoc.org/HTMLframesConverter/phpdoc.de/phpDocumentor/tutorial_tags.example.pkg.html>`_
*  `@ignore <http://manual.phpdoc.org/HTMLframesConverter/phpdoc.de/phpDocumentor/tutorial_tags.ignore.pkg.html>`_
*  `@internal <http://manual.phpdoc.org/HTMLframesConverter/phpdoc.de/phpDocumentor/tutorial_tags.internal.pkg.html>`_
*  `@link <http://manual.phpdoc.org/HTMLframesConverter/phpdoc.de/phpDocumentor/tutorial_tags.link.pkg.html>`_
*  `@see <http://manual.phpdoc.org/HTMLframesConverter/phpdoc.de/phpDocumentor/tutorial_tags.see.pkg.html>`_
*  `@since <http://manual.phpdoc.org/HTMLframesConverter/phpdoc.de/phpDocumentor/tutorial_tags.since.pkg.html>`_
*  `@tutorial <http://manual.phpdoc.org/HTMLframesConverter/phpdoc.de/phpDocumentor/tutorial_tags.tutorial.pkg.html>`_
*  `@version <http://manual.phpdoc.org/HTMLframesConverter/phpdoc.de/phpDocumentor/tutorial_tags.version.pkg.html>`_

Les tags de PhpDoc sont un peu du même style que les tags de JavaDoc dans Java. Les Tags 
sont seulement traités si ils sont la première chose dans la ligne DocBlock, par exemple::

    <?php
    /**
     * Exemple de Tag.
     * @author this tag is parsed, but this @version is ignored
     * @version 1.0 this tag is also parsed
     */

::

    <?php 
    /**
     * Exemple de tags inline phpDoc.
     *
     * Cette fonction travaille dur avec foo() pour gouverner le monde.
     */
    function bar() {
    }
     
    /**
     * Foo function
     */
    function foo() {
    }

Les blocks de commentaires, avec une exception du premier block dans le fichier, doivent
toujours être précédés par un retour à la ligne.

Inclure les fichiers
====================

Quand on inclut les fichiers avec des classes ou librairies, utilisez seulement
et toujours la fonction `require\_once <http://php.net/require_once>`_.

Les tags PHP
============

Toujours utiliser les tags longs (``<?php ?>``) plutôt que les tags courts (<? ?>).

Convention de nommage
=====================

Fonctions
---------

Ecrivez toutes les fonctions en camelBack::

    <?php
    function NomDeFonctionLong() {
    }

Classes
-------

Les noms de classe doivent être écrites en CamelCase, par exemple::

    <?php
    class ClasseExemple {
    }

Variables
---------

Les noms de variable doivent être aussi descriptives que possibles, mais
aussi courtes que possibles. Les variables normales doivent démarrer 
avec une lettre minuscule, et doivent être écrites en camelBack en cas
de mots multiples. Les variables contenant des objets doivent démarrer 
avec une majuscule, et d'une certaine manière associées à la classe dont
elles proviennent. Exemple::

    <?php
    $user = 'John';
    $users = array('John', 'Hans', 'Arne');

    $Dispatcher = new Dispatcher();

Visibilité des membres
----------------------

Utilisez les mots-clés private et protected de PHP5 pour les méthodes et variables.
De plus les noms des méthodes et variables protégées commencent avec un underscore
simple ("\_"). Exemple::

    <?php
    class A {
        protected $_iAmAProtectedVariable;

        protected function _iAmAProtectedMethod() {
           /*...*/
        }
    }

Les noms de méthode et variables privées commencent avec un underscore double ("\_\_"). Exemple::

    <?php
    class A {
        private $__iAmAPrivateVariable;

        private function __iAmAPrivateMethod() {
            /*...*/
        }
    }

Chaînage des méthodes
---------------------

Le chaînage des méthodes doit avoir des méthodes multiples répartis dans des lignes
distinctes, et indenté avec une tabulation::

    <?php
    $email->from('foo@example.com')
        ->to('bar@example.com')
        ->subject('A great message')
        ->send();

Exemple d'adresses
------------------

Pour tous les exemples d'URL et d'adresse email, utilisez "example.com", "example.org"
and "example.net", par exemple:

*  Email: someone@example.com
*  WWW: `http://www.example.com <http://www.example.com>`_
*  FTP: `ftp://ftp.example.com <ftp://ftp.example.com>`_

Le nom de domaine ``example.com`` est réservé à cela (see :rfc:`2606`) et est recommandée
pour l'utilisation dans la documentation ou comme exemples.

Fichiers
--------

Les noms de fichier qui ne contiennent pas de classes, doivent être écrits en minuscules et soulignés,
par exemple:
::

    nom_de_fichier_long.php

Types de variables
------------------

Les types de variable pour l'utilisation dans DocBlocks:

Type
    Description
mixed
    Une variable avec un type indéfini (ou multiple).
integer
    Variable de type Integer (Tout nombre).
float
    Type Float (nombres à virgule).
boolean
    Type Logique (true or false).
string
    Type String (toutes les valeurs en "" ou ' ').
array
    Type Tableau.
object
    Type Objet.
resource
    Type Ressource (retourné par exemple par mysql\_connect()).
    Rappelez vous que quand vous spécifiez un type en mixed, vous devez indiquer
    si il est inconnu, ou les types possibles.

Constantes
---------

Les constantes doivent être définies en majuscules:

::

    <?php
    define('CONSTANT', 1);

Si un nom de constante a plusieurs mots, ils doivent être séparés par un caractère
underscore, par exemple:

::

    <?php
    define('LONG_NAMED_CONSTANT', 2);


.. meta::
    :title lang=fr: Normes de code
    :keywords lang=fr: accolades,niveau d'indentation,erreurs logiques,structures de contrôle,structure de contôle,expr,normes de code,parenthèses,foreach,Lecture possible,moose,nouvelles fonctionnalités,dépôt,developpeurs