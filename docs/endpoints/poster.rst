Poster
======

endpoint: /poster/:id
---------------------

Get the image representation of object *:id* as thumbnail url
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

**Request type: GET**

Return the thumbnail url of an image representation of the object *:id*.
The thumbnail returned depends from the object type of *:id* and from
its relations, in particular:

1. if object *:id* has a 'poster' relation with an image object then it
   returns a thumbnail of that image 2.else if the object is an image
   object then it returns a thumbnail of the object
2. else if the object has an 'attach' relation with an image object then
   it returns a thumbnail of that image

The response will be

.. code:: json

    {
      "api": "poster",
      "data": {
        "id": 5,
        "uri": "http://media.server/path/to/thumb/thumbnail.jpg"
      },
      "method": "get",
      "params": [],
      "url": "https://example.com/api/poster/5"
    }

**Query Url Parameters**

-  ``width`` the thumbnail width
-  ``height`` the thumbnail height
