Authentication
==============

endpoint: /auth
---------------

It used to retrieve an ``access_token`` to access protected items, renew
``access_token`` and remove permissions. The ``access_token`` is a `Json
Web Token <http://jwt.io>`__
(`IETF <https://tools.ietf.org/html/rfc7519>`__). More info on
authentication
`here <https://github.com/bedita/bedita/wiki/REST-API:-Authentication>`__

.. note::

    Because of JWT is digital signed using ``'Security.salt'`` you should
    always remember to change it in ``app/config/core.php`` file:

.. code:: php

    Configure::write('Security.salt', 'my-security-random-string');

It is possible to invalidate all ``access_token`` released simply
changing that value.

Obtain an access token
~~~~~~~~~~~~~~~~~~~~~~

**Request type: POST**

Parameters

.. code-block:: json

    {
        "username": "test",
        "password": "test",
        "grant_type": "password"
    }

``grant_type`` is optional because it is the default used if no one is
passed.

If user is validated the response will contain the JWT, the time to
expire (in seconds) and the refresh\_token useful to renew the JWT

.. code-block:: json

    {
        "api": "auth",
        "data": {
            "access_token": "eyJ0eXAi.....",
            "expires_in": 600,
            "refresh_token": "51a3f718e7abc712e421f64ea497a323aea4e76f"
            },
        "method": "post",
        "params": [ ],
        "url": "https://example.com/api/auth"
    }

Once you received the access token you have to use it in every request
that require authentication. It can be used in HTTP header

::

    Authorization: Bearer eyJ0eXAi.....

or as query url ``/api/endpoint?access_token=eyJ0eXAi.....``

Renew the access token
~~~~~~~~~~~~~~~~~~~~~~

If the access token was expired you need to generate a new one started
by refresh token. *In this case do not pass the expired access token*

**Request type: POST**

Parameters

.. code-block:: json

    {
        "grant_type": "refresh_token",
        "refresh_token": "51a3f718e7abc712e421f64ea497a323aea4e76f"
    }

If refresh token is valid it returns the new access token

.. code-block:: json

    {
        "api": "auth",
        "data": {
            "access_token": "rftJasd3.....",
            "expires_in": 600,
            "refresh_token": "51a3f718e7abc712e421f64ea497a323aea4e76f"
            },
        "method": "post",
        "params": [ ],
        "url": "https://example.com/api/auth"
    }

Get the updated time to access token expiration
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Calling /auth in GET using the access\_token return the updated
'expires\_in' time.

**Request type: GET**

It returns

``json  {     "api": "auth",     "data": {         "access_token": "rftJasd3.....",         "expires_in": 48     },     "method": "get",     "params": [ ],     "url": "https://example.com/api/auth"  }``

Revoking a refresh token /auth/:refresh\_token
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

In order to invalidate an access\_token you need to remove it from
client and revoke the refresh token

**Request type: DELETE**

If the refresh token is deleted it responds as HTTP 204 No Content.
