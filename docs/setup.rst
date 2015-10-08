Setup a frontend to consume API
--------------

To use REST API in your frontend app you need the next BEdita 3.6.0
version. You can already test it using 3-corylus branch.

.. note::

   Because of authentication is handled using `Json Web Token <http://jwt.io>`__(`IETF <https://tools.ietf.org/html/rfc7519>`__) 
   and the JWT is digital signed using ``'Security.salt'`` you should always remember to change
   it in ``app/config/core.php`` file:

   .. code:: php

      Configure::write('Security.salt', 'my-security-random-string');

Enable API on new frontend app
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

-  from shell ``bash  $ cd /path/to/bedita  $ ./cake.sh frontend init``

-  in ``app/config/frontend.ini.php`` define
   ``$config['api']['baseUrl']`` with your API base url, for example
   ``php  $config['api'] = array( 'baseUrl' => '/api/v1'  );``

That's all! You are ready to consume the API!

Point the browser to your API base url and you should see the list of
endpoints available, for example

.. code:: json

    {
        "auth": "http://example.com/api/v1/auth",
        "me": "http://example.com/api/v1/me",
        "objects": "http://example.com/api/v1/objects",
        "poster": "http://example.com/api/v1/poster"
    }


Enable API on old frontend app
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

-  create a new Api Controller in your frontend

\`\`\`php 'api', 'action' => 'route')); } \`\`\`

above
``Router::connect('/*', array('controller' => 'pages', 'action' => 'route'));``

That's all!

After `#570 <https://github.com/bedita/bedita/issues/570>`__ we have
implemented a new (and better) way to handle Exceptions. Remember to
update your frontend ``index.php`` file:

.. code:: php

    if (isset($_GET['url']) && $_GET['url'] === 'favicon.ico') {
        return;
    } else {
        $Dispatcher = new Dispatcher();
        $Dispatcher->dispatch();
    }

Also make sure you have defined ``views/errors/error.tpl`` in your
frontend for generic error handling.
