Authentication ``/auth``
========================

It used to retrieve an ``access_token`` to access protected items, renew
``access_token`` and remove permissions. The ``access_token`` is a `Json Web Token <http://jwt.io>`_
(`IETF <https://tools.ietf.org/html/rfc7519>`_). More info on :doc:`authentication </authentication>`

.. note::

    Because of JWT is digital signed using ``'Security.salt'`` you should
    always remember to change it in ``app/config/core.php`` file:

    .. code-block:: php

        Configure::write('Security.salt', 'my-security-random-string');

    It is possible to invalidate all ``access_token`` released simply
    changing that value.

Obtain an access token
----------------------

.. http:post:: /auth

    Authenticate an user to obtain an ``access_token``.

    :reqjson string username: the username
    :reqjson string password: the password
    :reqjson string grant_type: `"password"`, the grant type to apply (password is the default, it can be ommitted)

    :resheader Content-Type: application/json
    :status 200: response contains ``access_token`` and ``refresh_token``
    :status 400: when required parameters are missing or the request is malformed
    :status 401: when authentication fails

    **Example request**:

    .. sourcecode:: http

        POST /auth HTTP/1.1
        Host: example.com
        Accept: application/json, text/javascript
        Content-Type: application/json

        {
            "username": "test",
            "password": "test",
            "grant_type": "password"
        }

    **Example response**:

    .. sourcecode:: http

        HTTP/1.1 200 OK
        Content-Type: application/json

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


    .. note::

        Once you received the access token you have to use it in every request
        that requires authentication. It can be used in HTTP header

        ::

            Authorization: Bearer eyJ0eXAi.....

        or as query string ``/api/endpoint?access_token=eyJ0eXAi.....``

Renew the access token
----------------------

If the access token was expired you need to generate a new one started
by refresh token. **In this case do not pass the expired access token**

.. http:post:: /auth

    Renew an `access_token`.

    :reqjson string refresh_token: the ``refresh_token`` to use to renew ``access_token``
    :reqjson string grant_type: `"refresh_token"`, the grant type to apply

    :resheader Content-Type: application/json
    :status 200: respond with `access_token` and `refresh_token`
    :status 400: when required parameters are missing or the request is malformed
    :status 401: when ``refresh_token`` is invalid

    **Example request**:

    .. sourcecode:: http

        POST /auth HTTP/1.1
        Host: example.com
        Accept: application/json, text/javascript
        Content-Type: application/json

        {
            "grant_type": "refresh_token",
            "refresh_token": "51a3f718e7abc712e421f64ea497a323aea4e76f"
        }

    **Example response**:

    .. sourcecode:: http

        HTTP/1.1 200 OK
        Content-Type: application/json

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
-----------------------------------------------

.. http:get:: /auth

    It returns the updated ``expires_in`` time for ``access_token``

    :reqheader Authorization: the ``access_token`` as Bearer token

    :resheader Content-Type: application/json
    :status 200: no error, payload contains the updated ``expires_in`` value
    :status 400: the request is malformed
    :status 401: the ``access_token`` is invalid

    **Example request**:

    .. sourcecode:: http

        GET /auth HTTP/1.1
        Host: example.com
        Accept: application/json, text/javascript

    **Example response**:

    .. sourcecode:: http

        HTTP/1.1 200 OK
        Content-Type: application/json

        {
            "api": "auth",
            "data": {
                "access_token": "rftJasd3.....",
                "expires_in": 48
            },
            "method": "get",
            "params": [ ],
            "url": "https://example.com/api/auth"
        }

Revoking a ``refresh_token``
----------------------------

In order to invalidate an ``access_token`` you need to remove it from client and revoke the ``refresh_token``

.. http:delete:: /auth/(string:refresh_token)

    Revoke a ``refresh_token``

    :reqheader Authorization: the ``access_token`` as Bearer token
    :param string refresh_token: the ``refresh_token`` to revoke

    :status 204: the refresh token was deleted
    :status 400: the request is malformed
    :status 401: the ``access_token`` is invalid or
    :status 404: the ``refresh_token`` was already revoked or not exists
