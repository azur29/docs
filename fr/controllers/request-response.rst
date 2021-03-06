Les Objets Request et Response
##############################

Les objets request et response sont nouveaux depuis CakePHP 2.0. Dans les versions
précédentes, ces objets étaient représentés à travers des array, et les méthodes liées
étaient répandues à travers :php:class:`RequestHandlerComponent`, :php:class:`Router`, 
:php:class:`Dispatcher` and :php:class:`Controller`. Il n'y avait pas d'objet authorisé
qui reprenait les informations de la requête. Pour CakePHP 2.0,
:php:class:`CakeRequest` et :php:class:`CakeResponse` sont utilisés à ce sujet.

.. index:: $this->request
.. _cake-request:

CakeRequest
###########

:php:class:`CakeRequest` est l'objet requête utilisé par défaut dans CakePHP.  Il centralise
un certain nombre de fonctionnalités pour interroger et intéragir avec les données demandées.
Pour chaque requête, une CakeRequest est créée et passée en référence aux différentes couches
de l'application que la requête de données utilise. Par défaut ``CakeRequest`` est assignée
à ``$this->request``, et est disponible dans les Contrôleurs, Vues et Helpers. Vous pouvez
aussi y accéder dans les Composants en utlisant la référence du contrôleur. Certaines des tâches
incluses que ``CakeRequest`` permet :

* Transformer les tableaux GET, POST, et FILES en structures de données avec lesquelles
  vous êtes familiers.
* Fournir une introspection de l'environnement se rapportant à la demande. Des choses comme
  les envois d'en-têtes (headers), l'adresse IP du client et les informations des
  sous-domaines/domaines sur lesquels le serveur de l'application tourne.
* Fournit un accès aux paramètres de la requête à la fois en tableaux indicés et en propriétés
  d'un objet.

Accéder aux paramètres de la requête
====================================

CakeRequest propose plusieurs interfaces pour accéder aux paramètres de la requête. La première
est des tableaux indexés, la seconde est à travers ``$this->request->params``, et la troisième
est des propriétés d'objets::

    <?php
    $this->request['controller'];
    $this->request->controller;
    $this->request->params['controller']

Tout ce qui est au-dessus retournera la même valeur. Plusieurs façons d'accéder
aux paramètres a été fait pour faciliter la migration des applications existantes.
Tous les éléments de route :ref:`route-elements` sont accessibles à travers cette
interface.

En plus des éléments de routes :ref:`route-elements`, vous avez souvent besoin d'accéder
aux arguments passés :ref:`passed-arguments` et aux paramètres nommés :ref:`named-parameters`.
Ceux-ci sont aussi tous les deux disponibles dans l'objet request::

    <?php
    // Arguments passés
    $this->request['pass'];
    $this->request->pass;
    $this->request->params['pass'];

    // Paramètres nommés
    $this->request['named'];
    $this->request->named;
    $this->request->params['named'];

Il vous fournira un accès aux arguments passés et ax paramètres nommés.
Il y a de nombreux paramètres importants et utiles que CakePHP utiise en
interne, il sont aussi trouvables dans les paramètres de la requête:

* ``plugin`` Le plugin gèrant la requête, va être nul pour les non-plugins.
* ``controller`` Le contrôleur gère la requête courante.
* ``action`` L'action gère la requête courante.
* ``prefix`` Le prefix pour l'action courante. Voir :ref:`prefix-routing` pour
  plus d'informations.
* ``bare`` Présent quannd la requête vient de requestAction() et inclut l'option bare.
  Les requêtes vides n'ont pas de layout de rendu.
* ``requested`` Présent et mis à true quand l'action vient de requestAction.


Accéder aux paramètres Querystring
==================================

Les paramètres Querystring peuvent être lus en utilisant :php:attr:`CakeRequest::$query`::

    <?php
    // url is /posts/index?page=1&sort=title
    $this->request->query['page'];

    //  Vous pouvez aussi y accéder par un tableau d'accès
    $this->request['url']['page'];

Accéder aux données POST
========================

Toutes les données POST peuvent être accédées par :php:attr:`CakeRequest::$data`. N'importe quelle
forme de tableau qui contient un prefix ``data``, va avoir sa donnée prefixée retirée. Par exemple::

    <?php
    // An input with a name attribute equal to 'data[Post][title]' is accessible at
    $this->request->data['Post']['title'];

Vous pouvez soit accéder directement à la propriété des données, soit vous pouvez
utiliser :php:meth:`CakeRequest::data()` pour lire le tableau de données sans erreurs.
N'importe quel clé qui n'existe pas va retourner ``null``::

    <?php
    $foo = $this->request->data('Value.that.does.not.exist');
    // $foo == null

Accéder aux données XML ou JSON
===============================

Les applications employant :doc:`/development/rest` échangent souvent des données
dans des organes post non encodées en URL. Vous pouvez lire les données entrantes
dans n'importe quel format en utilisant :php:meth:`CakeRequest::input()`. En fournissant
une fonction de décodage, vous pouvez recevoir le contenu dans un format déserializé::

    <?php
    // Obtenir les données encodées JSON soumises par une action PUT/POST
    $data = $this->request->input('json_decode');

Depuis que certaines méthodes de déserialization ont besoin de paramètres additionnels
quand elles sont appelées, comme le paramètre 'en tant que tableau' ('as array') pour
``json_decode`` ou si vous voulez convertir les XML en objet DOMDocument,
:php:meth:`CakeRequest::input()` supporte le passement dans des paramètres additionnels
aussi::

    <?php
    // Obtenir les données encodées en Xml soumises avec une action PUT/POST
    $data = $this->request->input('Xml::build', array('return' => 'domdocument'));

Accéder aux informations du chemin
==================================

CakeRequest fournit aussi des informations utiles sur les chemins dans votre application.
:php:attr:`CakeRequest::$base` et :php:attr:`CakeRequest::$webroot` sont utiles pour générer
des urls, et déterminer si votre application est ou n'est pas dans un sous-dossier.

.. _check-the-request:

Inspecter la requête
====================

Détecter des conditions variées de la requête utilisée utilisant
r various request conditions used to require using
:php:class:`RequestHandlerComponent`. Ces méthodes ont été déplacées dans
``CakeRequest``, et offrent une nouvelle interface compatible avec les utlisations
anciennes::

    <?php
    $this->request->is('post');
    $this->request->isPost();

Les deux méthodes appellées vont retourner la même valeur. Pour l'instant,
les méthodes sont toujours disponibles dans RequestHandler, mais sont depréciées
et pourraient être retirées avant la version finale. Vous pouvez aussi facilement
étendre les détecteurs de la requête qui sont disponibles, en utilisant
:php:meth:`CakeRequest::addDetector()` pour créer de nouveaux types de détecteurs.
Il y a quatre différents types de détecteurs que vous pouvez créer:

* Comparaison avec valeur d'environnement - Une comparaison de la valeur d'environnement,
  compare une valeur attrapée à partir de :php:func:`env()` pour une valeur connue, la valeur
  d'environnement est vérifiée équitablement avec la valeur fournie.
* La comparaison de la valeur modèle - La comparaison de la valeur modèle vous autorise à comparer
  une valeur attrapée à partir de :php:func:`env()` à une expression régulière.
* Comparaison basée sur les options -  La comparaison basée sur les options utilise une liste
  d'options pour créer une expression régulière. De tels appels pour ajouter un détecteur
  d'options déjà définie, va fusionner les options.
* Les détecteurs de Callback - Les détecteurs de Callback vous permettrent de fournir un type
  'callback' pour gérer une vérfication. Le callback va recevoir l'objet requête comme seul
  paramètre.

Quelques exemples seraient::

    <?php
    // Ajouter un détecteur d'environment.
    $this->request->addDetector('post', array('env' => 'REQUEST_METHOD', 'value' => 'POST'));
    
    // Ajouter un détecteur de valeur modèle.
    $this->request->addDetector('iphone', array('env' => 'HTTP_USER_AGENT', 'pattern' => '/iPhone/i'));
    
    // Ajouter un détecteur d'options
    $this->request->addDetector('internalIp', array(
        'env' => 'CLIENT_IP', 
        'options' => array('192.168.0.101', '192.168.0.100')
    ));
    
    // Ajouter un détecteur de callback detector. Peut soit être une fonction anonyme
    ou un callback régulier.
    $this->request->addDetector('awesome', array('callback' => function ($request) {
        return isset($request->awesome);
    }));

``CakeRequest`` also includes methods like :php:meth:`CakeRequest::domain()`,
:php:meth:`CakeRequest::subdomains()` and :php:meth:`CakeRequest::host()` to
help applications with subdomains, have a slightly easier life.

There are several built-in detectors that you can use:

* ``is('get')`` Check to see if the current request is a GET.
* ``is('put')`` Check to see if the current request is a PUT.
* ``is('post')`` Check to see if the current request is a POST.
* ``is('delete')`` Check to see if the current request is a DELETE.
* ``is('head')`` Check to see if the current request is HEAD.
* ``is('options')`` Check to see if the current request is OPTIONS.
* ``is('ajax')`` Check to see of the current request came with 
  X-Requested-with = XmlHttpRequest.
* ``is('ssl')`` Check to see if the request is via SSL
* ``is('flash')`` Check to see if the request has a User-Agent of Flash
* ``is('mobile')`` Check to see if the request came from a common list
  of mobile agents.


CakeRequest and RequestHandlerComponent
=======================================

Since many of the features ``CakeRequest`` offers used to be the realm of
:php:class:`RequestHandlerComponent` some rethinking was required to figure out how it
still fits into the picture.  For 2.0, :php:class:`RequestHandlerComponent` 
acts as a sugar daddy.  Providing a layer of sugar on top of the utility 
`CakeRequest` affords. Sugar like switching layout and views based on content 
types or ajax is the domain of :php:class:`RequestHandlerComponent`.  
This separation of utility and sugar between the two classes lets you 
more easily pick and choose what you want and what you need.

Interacting with other aspects of the request
=============================================

You can use `CakeRequest` to introspect a variety of things about the request.
Beyond the detectors, you can also find out other information from various
properties and methods.

* ``$this->request->webroot`` contains the webroot directory.
* ``$this->request->base`` contains the base path.
* ``$this->request->here`` contains the full address to the current request
* ``$this->request->query`` contains the query string parameters.


CakeRequest API
===============

.. php:class:: CakeRequest

    CakeRequest encapsulates request parameter handling, and introspection.

.. php:method:: domain()

    Returns the domain name your application is running on.

.. php:method:: subdomains() 

    Returns the subdomains your application is running on as an array.

.. php:method:: host() 

    Returns the host your application is on.

.. php:method:: method() 

    Returns the HTTP method the request was made with.

.. php:method:: referer() 

    Returns the referring address for the request.

.. php:method:: clientIp() 

    Returns the current visitor's IP address.

.. php:method header()

    Allows you to access any of the ``HTTP_*`` headers that were used 
    for the request::

        <?php
        $this->request->header('User-Agent');

    Would return the user agent used for the request.

.. php:method:: input($callback, [$options])

    Retrieve the input data for a request, and optionally pass it through a
    decoding function.  Additional parameters for the decoding function
    can be passed as arguments to input().

.. php:method:: data($key) 

    Provides dot notation access to request data.  Allows for reading and
    modification of request data, calls can be chained together as well::

        <?php
        // Modify some request data, so you can prepopulate some form fields.
        $this->request->data('Post.title', 'New post')
            ->data('Comment.1.author', 'Mark');
            
        // You can also read out data.
        $value = $this->request->data('Post.title');

.. php:method:: is($check)

    Check whether or not a Request matches a certain criteria.  Uses
    the built-in detection rules as well as any additional rules defined
    with :php:meth:`CakeRequest::addDetector()`.

.. php:method:: addDetector($name, $callback)

    Add a detector to be used with is().  See :ref:`check-the-request`
    for more information.

.. php:method:: accepts($type)

    Find out which content types the client accepts or check if they accept a 
    particular type of content.

    Get all types::

        <?php 
        $this->request->accepts();
 
    Check for a single type::

        <?php
        $this->request->accepts('json');

.. php:staticmethod:: acceptLanguage($language)

    Get either all the languages accepted by the client,
    or check if a specific language is accepted.

    Get the list of accepted languages::

        <?php
        CakeRequest::acceptLanguage(); 

    Check if a specific language is accepted::

        <?php
        CakeRequest::acceptLanguage('es-es'); 

.. php:attr:: data

    An array of POST data. You can use :php:meth:`CakeRequest::data()`
    to read this property in a way that suppresses notice errors.

.. php:attr:: query

    An array of query string parameters.

.. php:attr:: params

    An array of route elements and request parameters.

.. php:attr:: here

    Returns the current request uri.

.. php:attr:: base

    The base path to the application, usually ``/`` unless your 
    application is in a subdirectory.

.. php:attr:: webroot

    The current webroot.

.. index:: $this->response

CakeResponse
############

:php:class:`CakeResponse` is the default response class in CakePHP.  It 
encapsulates a number of features and functionality for generating HTTP 
responses in your application. It also assists in testing, as it can be 
mocked/stubbed allowing you to inspect headers that will be sent.  
Like :php:class:`CakeRequest`, :php:class:`CakeResponse` consolidates a number
of methods previously found on :php:class:`Controller`,
:php:class:`RequestHandlerComponent` and :php:class:`Dispatcher`.  The old
methods are deprecated in favour of using :php:class:`CakeResponse`.

``CakeResponse`` provides an interface to wrap the common response related 
tasks such as:

* Sending headers for redirects.
* Sending content type headers.
* Sending any header.
* Sending the response body.

Changing the response class
===========================

CakePHP uses ``CakeResponse`` by default. ``CakeResponse`` is a flexible and 
transparent to use class.  But if you need to replace it with an application 
specific class, you can override and replace ``CakeResponse`` with
your own class.  By replacing the CakeResponse used in index.php.

This will make all the controllers in your application use ``CustomResponse``
instead of :php:class:`CakeResponse`.  You can also replace the response
instance used by setting ``$this->response`` in your controllers. Overriding the 
response object is handy during testing, as it allows you to stub 
out the methods that interact with ``header()``.  See the section on 
:ref:`cakeresponse-testing` for more information.

Dealing with content types
==========================

You can control the Content-Type of your application's responses with using
:php:meth:`CakeResponse::type()`.  If your application needs to deal with
content types that are not built into CakeResponse, you can map those types
with ``type()`` as well::

    <?php
    // Add a vCard type
    $this->response->type(array('vcf' => 'text/v-card'));

    // Set the response Content-Type to vcard.
    $this->response->type('vcf');

Usually you'll want to map additional content types in your controller's
``beforeFilter`` callback, so you can leverage the automatic view switching 
features of :php:class:`RequestHandlerComponent` if you are using it.

Sending attachments
===================

There are times when you want to send Controller responses as files for
download.  You can either accomplish this using :doc:`/views/media-view`
or by using the features of ``CakeResponse``.
:php:meth:`CakeResponse::download()` allows you to send the response as file for
download::

    <?php
    public function sendFile($id) {
        $this->autoRender = false;

        $file = $this->Attachment->getFile($id);
        $this->response->type($file['type']);
        $this->response->download($file['name']);
        $this->response->body($file['content']);
    }

The above shows how you could use CakeResponse to generate a file download
response without using :php:class:`MediaView`.  In general you will want to use
MediaView as it provides a few additional features above what CakeResponse does.

Setting headers
===============

Setting headers is done with the :php:meth:`CakeResponse::header()` method.  It
can be called with a few different parameter configurations::

    <?php
    // Set a single header
    $this->response->header('Location', 'http://example.com');

    // Set multiple headers
    $this->response->header(array('Location' => 'http://example.com', 'X-Extra' => 'My header'));
    $this->response->header(array('WWW-Authenticate: Negotiate', 'Content-type: application/pdf'));

Setting the same header multiple times will result in overwriting the previous
values, just like regular header calls.  Headers are not sent when
:php:meth:`CakeResponse::header()` is called either.  They are just buffered
until the response is actually sent.

Interacting with browser caching
================================

You sometimes need to force browsers to not cache the results of a controller
action.  :php:meth:`CakeResponse::disableCache()` is intended for just that::

    <?php
    public function index() {
        // do something.
        $this->response->disableCache();
    }

.. warning::

    Using disableCache() with downloads from SSL domains while trying to send
    files to Internet Explorer can result in errors.

You can also tell clients that you want them to cache responses. By using
:php:meth:`CakeResponse::cache()`::

    <?php
    public function index() {
        //do something
        $this->response->cache(time(), '+5 days');
    }

The above would tell clients to cache the resulting response for 5 days,
hopefully speeding up your visitors' experience.


.. _cake-response-caching:

Fine tuning HTTP cache
======================

One of the best and easiest ways of speeding up your application is using HTTP
cache. Under this caching model you are only required to help clients decide if
they should use a cached copy of the response by setting a few headers such as
modified time, response entity tag and others.

Opposed to having to code the logic for caching and for invalidating (refreshing)
it once the data has changed, HTTP uses two models, expiration and validation
which usually are a lot simpler than having to manage the cache yourself.

Apart from using :php:meth:`CakeResponse::cache()` you can also use many other
methods to fine tune HTTP cache headers to take advantage of browser or reverse
proxy caching.

The Cache Control header
------------------------

.. versionadded:: 2.1

Used under the expiration model, this header contains multiple indicators
which can change the way browsers or proxies use the cached content. A
Cache-Control header can look like this::

    Cache-Control: private, max-age=3600, must-revalidate

``CakeResponse`` class helps you set this header with some utility methods that
will produce a final valid Cache-Control header. First of them is :php:meth:`CakeResponse::sharable()`
method, which indicates whether a response in to be considered sharable across
different users or clients or users. This method actually controls the `public`
or `private` part of this header. Setting a response as private indicates that
all or part of it is intended for a single user. To take advantage of shared
caches it is needed to set the control directive as public

Second parameter of this method is used to specify a `max-age` for the cache,
which is the number of seconds after which the response is no longer considered
fresh.::

    <?php
    public function view() {
        ...
        // set the Cache-Control as public for 3600 seconds
        $this->response->sharable(true, 3600);
    }

    public function my_data() {
        ...
        // set the Cache-Control as private for 3600 seconds
        $this->response->sharable(false, 3600);
    }

``CakeResponse`` exposes separate methods for setting each of the components in
the Cache-Control header.

The Expiration header
---------------------

.. versionadded:: 2.1

Also under the cache expiration model, you can set the `Expires` header, which
according to the HTTP specification is the date/time after which the response is
no longer considered fresh. This header can be set using the
:php:meth:`CakeResponse::expires()` method::

    <?php
    public function view() {
        $this->response->expires('+5 days');
    }

This method also accepts a DateTime or any string that can be parsed by the
DeteTime class.


The Etag header
---------------

.. versionadded:: 2.1

Cache validation in HTTP is often used when content is constantly changing, and
asks the application to only generate the response contents if the cache is no
longer fresh. Under this model, the client continues to store pages in the
cache, but instead of using it directly, it asks the application every time
whether the resources changed or not. This is commonly used with static
resources such as images and other assets.

The Etag header (called entity tag) is string that uniquely identifies the
requested resource. It is very much like the checksum of a file, caching
will compare checksums to tell whether they match or not.

To actually get advantage of using this header you have to either call manually
:php:meth:`CakeResponse::checkNotModified()` method or have the :php:class:`RequestHandlerComponent`
included in your controller::

    <?php
    public function index() {
        $articles = $this->Article->find('all');
        $this->response->etag($this->Article->generateHash($articles));
        if ($this->response->checkNotModified($this->request)) {
            return $this->response;
        }
        ...
    }

The Last Modified header
------------------------

.. versionadded:: 2.1

Also under the HTTP cache validation model, you can set the `Last-Modified`
header to indicate the date and time at which the resource was modified for the
last time. Setting this header helps CakePHP respond to caching clients whether
the response was modified or not based on the client cache.

To actually get advantage of using this header you have to either call manually
:php:meth:`CakeResponse::checkNotModified()` method or have the :php:class:`RequestHandlerComponent`
included in your controller::

    <?php
    public function view() {
        $article = $this->Article->find('first');
        $this->response->modified($article['Article']['modified']);
        if ($this->response->checkNotModified($this->request)) {
            return $this->response;
        }
        ...
    }

The Vary header
---------------

In some cases you might want to serve different contents using the same url.
This is often the case when you have a multilingual page or respond with
different HTML according to the browser that is requesting the resource. For
such circumstances, you use the Vary header::

    <?php
        $this->response->vary('User-Agent');
        $this->response->vary('Accept-Encoding', 'User-Agent');
        $this->response->vary('Accept-Language');

.. _cakeresponse-testing:

CakeResponse and testing
========================

Probably one of the biggest wins from ``CakeResponse`` comes from how it makes
testing controllers and components easier.  Instead of methods spread across
several objects, you only have a single object to mock as controllers and
components delegate to ``CakeResponse``.  This helps you get closer to a 'unit'
test and makes testing controllers easier::

    <?php
    public function testSomething() {
        $this->controller->response = $this->getMock('CakeResponse');
        $this->controller->response->expects($this->once())->method('header');
        // ...
    }

Additionally you can more easily run tests from the command line, as you can use
mocks to avoid the 'headers sent' errors that can come up from trying to set
headers in CLI.


CakeResponse API
================

.. php:class:: CakeResponse

    CakeResponse provides a number of useful methods for interacting with
    the response you are sending to a client.

.. php:method:: header() 

    Allows you to directly set one or many headers to be sent with the response.

.. php:method:: charset() 

    Sets the charset that will be used in the response.

.. php:method:: type($type) 

    Sets the content type for the response.  You can either use a known content
    type alias or the full content type name.

.. php:method:: cache()

    Allows you to set caching headers in the response.

.. php:method:: disableCache()

    Sets the headers to disable client caching for the response.

.. php:method:: sharable($isPublic, $time)

    Sets the Cache-Control header to be either `public` or `private` and
    optionally sets a `max-age` directive of the resource

    .. versionadded:: 2.1

.. php:method:: expires($date)

    Allows to set the `Expires` header to a specific date.

    .. versionadded:: 2.1

.. php:method:: etag($tag, $weak)

    Sets the `Etag` header to uniquely identify a response resource.

    .. versionadded:: 2.1

.. php:method:: modified($time)

    Sets the `Last-Modified` header to a specific date and time in the correct
    format.

    .. versionadded:: 2.1

.. php:method:: checkNotModified(CakeRequest $request)

    Compares the cache headers for the request object with the cache header from
    the response and determines if it can still be considered fresh. In that
    case deletes any response contents and sends the `304 Not Modified` header.

    .. versionadded:: 2.1

.. php:method:: compress()

    Turns on gzip compression for the request.

.. php:method:: download() 

    Allows you to send the response as an attachment and set the filename.

.. php:method:: statusCode() 

    Allows you to set the status code for the response.

.. php:method:: body()

    Set the content body for the response.

.. php:method:: send()

    Once you are done creating a response, calling send() will send all
    the set headers as well as the body. This is done automatically at the
    end of each request by :php:class:`Dispatcher`



.. meta::
    :title lang=en: Request and Response objects
    :keywords lang=en: request controller,request parameters,array indices,purpose index,response objects,domain information,request object,request data,interrogating,params,previous versions,introspection,dispatcher,rout,data structures,arrays,ip address,migration,indexes,cakephp
