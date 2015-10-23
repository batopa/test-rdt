User profile ``/me``
====================

.. warning::

   This endpoint is highly volatile. It could be completely changed
   or removed in next versions.

Obtain information about authenticated user
-------------------------------------------

.. http:get:: /me

    Return information about current authenticated user

    :reqheader Authorization: (**required**) :term:`access token`

    :resheader Content-Type: application/json

    :status 200: Success
    :status 401: The request is not authorized
    :status 404:
