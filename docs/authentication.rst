By default all GET requests don't require client and user
authenticatication unless the object requested has permission on it. In
that case the user has to be authenticated before require the resource.
Other operations as writing/deleting objects (POST, PUT, DELETE on
``/objects`` endpoint) are always protected instead and they always
require authentication.

The API follow a **token based authentication flow** using a `Json Web
Token <http://jwt.io>`__ as ``access_token`` and an opaque token as
``refresh_token`` useful to renew the ``access_token`` without ask again
the user credentials. See
`here <https://github.com/bedita/bedita/wiki/REST-API:-endpoints#endpoint-auth>`__
to more details on how to obtain ``access_token`` and ``refresh_token``.

::

    +--------+                                     +---------------+
    |        |--(A)- Authorization Request ------->|   Resource    |
    |        |                                     |     Owner     |
    |        |<-(B)-- Authorization Grant ---------|               |
    |        |                                     +---------------+
    |        |
    |        |                                     +---------------+
    |        |--(C)-- Authorization Grant -------->| Authorization |
    | Client |                                     |     Server    |
    |        |<-(D)----- Access Token (JWT) -------|               |
    |        |                and                  |               |
    |        |           Refresh Token             |               |
    |        |                                     +---------------+
    |        |
    |        |                                     +---------------+
    |        |--(E)----- Access Token (JWT) ------>|    Resource   |
    |        |                                     |     Server    |
    |        |<-(F)--- Protected Resource ---------|               |
    +--------+                                     +---------------+

The ``access_token`` must be used in every request that require
permission. To use the ``access_token`` it has to be sent in HTTP
headers as **bearer token** ``Authorization: Bearer eyJ0eXAi.....``.

--------------

Because of JWT is digital signed using ``'Security.salt'`` you should
always remember to change it in ``app/config/core.php`` file:

.. code:: php

    Configure::write('Security.salt', 'my-security-random-string');

It is possible to invalidate all ``access_token`` released simply
changing that value.

--------------

All the logic to handle authentication is in ``ApiAuth`` component and
``ApiBaseController`` use it for you so authentication works out of the
box. If you need to protect `custom
endpoints <https://github.com/bedita/bedita/wiki/REST-API:-customize-endpoints>`__
you have to add to custom method

.. code:: php

    protected function customEndPoint() {
        if (!$this->ApiAuth->identify()) {
            throw new BeditaUnauthorizedException();
        }    
    }

Customize authentication
------------------------

If you need to customize or change the authentication you can define
your own auth component. To maintain the component method signature used
in ``ApiBaseController`` your component should implements the interface
``ApiAuthInterface``.

Remember that REST API are thought to implement token based
authentication with the use of both ``access_token`` and
``refresh_token`` so the interface define methods to handle these
tokens. If you need something different probably you would also override
authenication methods of ``ApiBaseController``.

In case you only need some little change it should be better to directly
extend ``ApiAuth`` component that already implements the interface, and
override the methods you need.

For example supposing you want to add additional check to user
credentials, you can simply override ``ApiAuth::authenticate()`` method
which deals with it:

.. code:: php

    App::import('Component', 'ApiAuth');

    class CustomAuthComponent extends ApiAuthComponent {

        public function authenticate($username, $password, array $authGroupName = array()) {
            // authentication logic here
        }

    }

and finally to activate the component all you have to do is define in
configuration file ``config/frontend.ini.php`` the auth component you
want to use.

.. code:: php

    $config['api'] = array(
        'baseUrl' => '/api',
        'auth' => array(
            'component' => 'CustomAuth'
        )
    );

In ``ApiController`` you will have access to ``CustomAuth`` instance by
``$this->ApiAuth`` attribute.
