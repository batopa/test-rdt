Objects
=======

endpoint: /objects
------------------

It used to get a BEdita object or a set of objects.

Get an object /objects/:name
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

where *:name* **can be the object id or the object unique name
(nickname)**. Note that the response data fields can change depending of
BEdita object type exposed so you could see more or less fields respect
to example below.

Every object can have relations with other objects. The count of objects
related is in ``data.object.relations.<relation_name>`` where there are
``count`` (the number of related object) and ``url`` fields. The ``url``
is the complete API request url to get the object related, for example
https://example.com/api/objects/1/relations/attach

**Request type: GET**

.. code-block:: json

    {
        "api": "objects",
        "data": {
            "object": {
                "id": 218932,
                "start_date": "2015-01-08T00:00:00+0100",
                "end_date": null,
                "subject": null,
                "abstract": null,
                "body": "This is the body text",
                "object_type_id": 22,
                "created": "2015-01-30T10:04:49+0100",
                "modified": "2015-05-08T12:59:49+0200",
                "title": "hello world",
                "nickname": "hello-world",
                "description": "the description",
                "valid": true,
                "lang": "eng",
                "rights": "",
                "license": "",
                "creator": "",
                "publisher": "",
                "note": null,
                "comments": "off",
                "publication_date": "2015-01-08T00:00:00+0100",
                "languages": {
                    "ita": {
                        "title": "ciao mondo"
                    }
                },
                "relations": {
                    "attach": {
                        "count": 8,
                        "url": "https://example.com/api/objects/1/relation/attach"
                    },
                    "seealso": {
                        "count": 2,
                        "url": "https://example.com/api/objects/1/relation/seealso"
                    }
                },
                "object_type": "Document",
                "authorized": true,
                "free_access": true,
                "custom_properties": {
                    "bookpagenumber": "12",
                    "number": "8"
                },
                "geo_tags": [
                    {
                        "id": 68799,
                        "object_id": 218932,
                        "latitude": 44.4948179,
                        "longitude": 11.33969,
                        "address": "via Rismondo 2, Bologna",
                        "gmaps_lookats": {
                            "zoom": 16,
                            "mapType": "k",
                            "latitude": 44.4948179,
                            "longitude": 11.33969,
                            "range": 44052.931589613
                        }
                    }
                ],
                "tags": [
                    {
                        "label": "tag one",
                        "name": "tag-one"
                    },
                    {
                        "label": "tag two",
                        "name": "tag-two"
                    }
                ],
                "categories": [
                    {
                        "id": 266,
                        "area_id": null,
                        "label": "category one",
                        "name": "category-one"
                    },
                    {
                        "id": 323,
                        "area_id": null,
                        "label": "category two",
                        "name": "category-two"
                    }
                ]
            }
        },
        "method": "get",
        "params": [],
        "url": "https://example.com/api/objects/218932"
    }

If *:name* corresponds to a **section** or a **publication** then the
response will have ``data.object.children`` with the total count of
children, count of contents, count of sections and the related url.

.. code-block:: json

    {
        "children": {
            "count": 14,
            "url": "https://example.com/api/objects/1/children",
            "contents": {
                "count": 12,
                "url": "https://example.com/api/objects/1/contents"
            },
            "sections": {
                "count": 2,
                "url": "https://example.com/api/objects/1/sections"
            }
        }
    }

Get a list of publication's descendants /objects
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

**Request type: GET**

Return a paginated list of objects that are descendants of the frontend
publication. The response will be an array of objects as shown below.

Get a list of related objects /objects/:name/:filter\_type
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Return a list of objects related to *:name* object using *:filter\_type*
filter.

*:filter\_type* value can be 'ancestors' (not supported yet), 'parents'
(not supported yet), 'children', 'descendants', 'siblings', 'contents',
'sections' and 'relations'

The response will usually be an array of objects as:

**Request type: GET**

.. code-block:: json

    {
        "api": "objects",
        "data": {
            "objects": [
                {
                    "id": 100,
                    "title": "my title",
                    ...
                },
                {
                    "id": 42,
                    "title": "other title",
                    ...
                },
                ...
            ]
        },
        "method": "get",
        "paging": {
            "page": 1,
            "page_size": 5,
            "page_count": 5,
            "total": 50,
            "total_pages": 10
        },
        "params": [],
        "url": "https://example.com/api/objects/1/children"
    }

.. _get-a-list-of-children:

Get a list of children /objects/:name/children
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

It returns the paginated children of object *:name*.

Get a list of children of type section /objects/:name/sections
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

It returns the paginated children of object *:name*. The children are
just sections ('section BEdita object type)

Get a list of children of type contents /objects/:name/contents
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

It returns the paginated children of object *:name*. The children are
other than sections.

Get a list of descendants /objects/:name/descendants
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

It returns the paginated descendants of object *:name*.

Get a list of siblings /objects/:name/siblings
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

It returns the paginated siblings of object *:name*.

.. _get-relations-count:

Get relations count /objects/:name/relations
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

It returns a summary of relations information about *:name* object. It
show every relation with the count and the url to get the related
objects detail.

.. code-block:: json

    {
        "api": "objects",
        "data": {
            "attach": {
                "count": 1,
                "url": "https://example.com/api/objects/1/relations/attach"
            },
            "seealso": {
                "count": 2,
                "url": "https://example.com/api/objects/1/relations/seealso"
            }
        },
        "method": "get",
        "params": [],
        "url": "https://example.com/api/objects/1/relations"
    }

Get the related objects detail /objects/:name/relations/:relation\_name
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

It returns the paginated list of objects related by *:relation\_name* to
*:name* object.

.. _get-the-relation-detail:

Get the relation detail /objects/:name/relations/:relation\_name/:related\_id
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

**Request type: GET**

It returns the relation *:relation\_name* detail from main object
*:name* and related object *related\_id*

.. code-block:: json

    {
      "api": "objects",
      "data": {
        "priority": 3,
        "params": {
          "label": "here the label"
        }
      },
      "method": "get",
      "params": [],
      "url": "https://example.com/api/objects/1/relations/attach/2"
    }

Get the child position /objects/:name/children/:child\_id
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

**Request type: GET**

It returns the position (``priority`` key) of *:child\_id* relative to
all children of parent object *:name*

.. code-block:: json

    {
      "api": "objects",
      "data": {
        "priority": 3
      },
      "method": "get",
      "params": [],
      "url": "https://example.com/api/objects/1/children/2"
    }

Create/update an object
-----------------------

**Request type: POST**

**Conditions:** User has to be :doc:`authenticated </endpoints/auth>`
and has to have the permission to access to the object.

Before save objects the frontend app that serve API has to be configured
to know what objects can be written

.. code-block:: php

    $config['api'] = array(
       ....
        'validation' => array(
            // to save 'document' and 'event' object types 
            'writableObjects' => array('document', 'event')
        )
    );

**Saving new objects** user has to be :doc:`authenticated </endpoints/auth>`
and **data** from client must contain: \* ``object_type`` i.e. the
object type you want to create \* at least a parent (``parents`` key)
accessible (with right permission for user authorized) on publication
tree or at least a relation (``relations`` key) with another object
reachable (where *reachable* means an accessible object on tree or
related to an accessible object on tree).

Example of valid data from client:

.. code-block:: json

    {
        "data": {
            "title": "My title",
            "object_type": "event",
            "description": "bla bla bla",
            "parents": [1, 34, 65],
            "relations": {
                "attach": [
                    {
                        "related_id": 12,
                        "params": {
                            "label": "foobar"
                        }
                    },
                    {
                        "related_id": 23
                    }
                ],
                "seealso": [
                    {
                        "related_id": 167
                    }
                ]
            },
            "categories": ["name-category-one", "name-category-two"],
            "tags": ["name-tag_one", "name-tag-two"],
            "geo_tags": [
                {
                    "title": "geo tag title",
                    "address": "via ....",
                    "latitude": 43.012,
                    "longitude": 10.45
                }
            ],
            "date_items": [
                {
                    "start_date": "2015-07-08T15:00:35+0200",
                    "end_date": "2015-07-08T15:00:35+0200",
                    "days": [0,3,4]
                },
                {
                    "start_date": "2015-09-01T15:00:35+0200",
                    "end_date": "2015-09-30T15:00:35+0200"
                }
            ]
        }
    }

dates must be in ISO 8601 format. In case of **success** a **201
Created** HTTP status code is returned with the detail of object created
in the response body.

You can use POST also to **update an existent object**. In that case the
object ``id`` has to be passed in "data" object from client and
``object_type`` can be omitted.

Add/edit relations
------------------

**Request type: POST**

**Conditions:** User has to be :doc:`authenticated </endpoints/auth>`
and has to have the permission to access to the object.

In order to add or edit relations you can use the endpoint ``/objects``
as ``/objects/:name/relations/:relation_name`` where *:name* can be the
object id or nickname. and *:relation\_name* the relation name.
Relations data must be an array of relation data or an object with
relation data if you need to save only one relation (note that it is the
same that send an array with only one relation).

-  ``related_id`` is the related object id and is mandatory
-  ``params`` fields depend from relation type (optional)
-  ``priority``\ is the position of the relation. Relation with lower
   priority are shown before (optional)

For example to add/edit attach relations to object with id 3 you can do
a request:

``POST /objects/3/relations/attach``

valid data can be:

.. code-block:: json

    {
        "data": [
            {
                "related_id": 15,
                "params": {
                    "label": "my label"
                }
            },
            {
                "related_id": 28
            }
        ]
    }

to create/update a bunch of relations, or

.. code-block:: json

    {
        "data": {
            "related_id": 34,
            "priority": 3
        }
    }

to create/update only one relation.

If a "relation\_name" relation between main object and related object
not exists then it is created else it is updated. If at least a relation
is created a **201 Created** HTTP status code is sent and an HTTP header
**Location** is set with url of :ref:`list of related objects <get-relations-count>`.

The response body will be an array of relation data just saved.

Saving new relations you can pass the ``priority`` you want to set. If
no ``priority`` is passed it is automatically calculated starting from
the max ``priority`` in the current relation.

Edit (replace) relation data between objects
--------------------------------------------

**Request type: PUT**

**Conditions:** User has to be :doc:`authenticated </endpoints/auth>`
and has to have the permission to access to the objects.

In order to edit the relation data between two objects you can use the
endpoint ``/objects`` as
``/objects/:name/relations/:relation_name/:related_id`` where *:name*
can be the object id or nickname, *:relation\_name* the relation name
and *:related\_id* the related object id. Relations data must be an
object with data

-  ``params`` fields depend from relation type
-  ``priority`` is the position of the relation. Relation with lower
   priority are shown before

At least ``params`` or ``priority`` must be defined. If one of these is
not passed it will be set to ``null``.

So to edit attach relation between object 1 and 2 the request will be

``PUT /objects/1/relations/attach/2``

.. code-block:: json

    {
        "data": {
            "priority": 3,
            "params": {
                "label": "new label"
            }
        }
    }

In case of success the server will respond with a **200 HTTP status
code** and the response body will be the same of :ref:`Get the relation detail <get-the-relation-detail>`

Add/edit children
-----------------

**Request type: POST**

**Conditions:** User has to be :doc:`authenticated </endpoints/auth>`
and has to have the permission to access to the object.

In order to add or edit children to a area/section object type you can
use the endpoint ``/objects`` as ``/objects/:name/children`` where
*:name* can be the object id or nickname. Children data must be an array
of child data or an object with child data if you need to save only one
child (note that it is the same that send an array with only one child).

-  ``child_id`` is the child object id and is mandatory
-  ``priority`` is the position of the child on the tree

For example to add/edit children to object with id 3 you can do a
request:

``POST /objects/3/children``

valid data can be:

.. code-block:: json

    {
        "data": [
            {
                "child_id": 15,
                "priority": 3
            },
            {
                "child_id": 28
            }
        ]
    }

to create/update a bunch of children, or

.. code-block:: json

    {
        "data": {
            "child_id": 34,
            "priority": 3
        }
    }

to create/update only one child.

If a "child\_id" is a new children for parent object then it is created
on tree else it is updated. If at least a new child is created a **201
Created** HTTP status code is sent and an HTTP header **Location** is
set with url of :ref:`list of children objects <get-a-list-of-children>`.

The response body will be an array of children data just saved.

Saving new children you can pass the ``priority`` you want to set i.e.
the position on the tree. If no ``priority`` is passed every new
children is appended to parent on tree structure.

Edit children position
----------------------

**Request type: PUT**

**Conditions:** User has to be :doc:`authenticated </endpoints/auth>`
and has to have the permission to access to the objects.

In order to edit children position you can use the endpoint ``/objects``
as ``/objects/:name/children/:child_id`` where *:name* can be the object
id or nickname and *:child\_id* is the children object id. Data passed
must contain ``priority`` field that is the position of child you want
to update.

For example to edit the position of child with id 2 of parent with id 1:

``PUT /objects/1/children/2``

.. code-block:: json

    {
        "data": {
            "priority": 5
        }
    }

Delete an object
----------------

**Request type: DELETE**

**Conditions:** User has to be :doc:`authenticated </endpoints/auth>`
and has to have the permission to access to the object.

To delete an object has to be used the endpoint ``/objects/:name`` where
*:name* can be the object id or nickname.

If the object is deleted successfully a **204 No Content** HTTP status
code is sent. Further requests to delete the same object will return a
**404 Not Found** HTTP status code.

Delete a relation between objects
---------------------------------

**Request type: DELETE**

**Conditions:** User has to be :doc:`authenticated </endpoints/auth>`
and has to have the permission to access to the object.

In order to delete an existent relation between two objects you can use
the endpoint ``/objects/:name/relations/:rel_name/:related_id`` where
*:name* is the object id or nickname, *:rel\_name* is the relation name
between objects and *:related\_id* is the object id related to object
*:name*.

If the relation is succesfully deleted *204 No Content* HTTP status code
is sent. Further requests to delete the same relation will return a **404
Not Found** HTTP status code.

Remove child from a parent
--------------------------

**Request type: DELETE**

**Conditions:** User has to be :doc:`authenticated </endpoints/auth>`
and has to have the permission to access to the object.

To remove an existent child of an object the endpoint
``/objects/:name/children/:child_id`` can be used, where *:name* is the
object id or nickname of parent and *:child\_id* is id of the child
object. Note that the child will be only removed from parent's tree but
it continue to exist.

If *:child\_id* is succesfully removed from *:name* children a **204 No
Content** HTTP status code is sent. Further requests to remove the same
child will return a **404 Not Found** HTTP status code.
