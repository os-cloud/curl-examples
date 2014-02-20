Using cURL with Cloud Files
###########################
:date: 2013-09-05 09:51
:tags: rackspace, Cloud Files, files, api
:category: \*nix


Variables For Access
~~~~~~~~~~~~~~~~~~~~


These Variables are not needed though can help with scripting
-------------------------------------------------------------

These are also placed here so you can see the commands easier:

.. code:: bash

  AUTHTOKEN="Authorization Token"
  STORAGEURL="Storage URL"
  CONTAINER="The Name Of The Container"
  OBJECT="The Object Name"
  FILENAME="The File Name"


Storage URLs
------------

ORD:

  * Public : https://storage101.ord1.clouddrive.com/v1/${MOSSOID}
  * SNET : https://snet-storage101.ord1.clouddrive.com/v1/${MOSSOID}


DFW:

  * Public : https://storage101.dfw1.clouddrive.com/v1/${MOSSOID}
  * SNET : https://snet-storage101.dfw1.clouddrive.com/v1/${MOSSOID}


The rest of the Data Centers all follow the same convention


HTTPS CDN Access
----------------

In order to use SSL on the CDN, you simply have to change "http" to "https" and "rXX" to "ssl". For

example:

  http://c193732.r32.cf1.rackcdn.com/100mbtest


would become:

  https://c193732.ssl.cf1.rackcdn.com/100mbtest




List Containers
---------------

.. code:: bash

  curl -D - -H "X-Auth-Token: ${AUTHTOKEN}" ${STORAGEURL}


List Containers showing name, object count, and size
----------------------------------------------------

.. code:: bash

  curl -s -H "X-Auth-Token: ${AUTHTOKEN}" ${STORAGEURL}/${CONTAINER}?format=json | python -m json.tool | awk '/bytes/ || /count/ || /name/ {print $2}' | awk 'ORS=(FNR%3)?FS:RS' FS="\t" | awk '{gsub(/\"/,"",$3); gsub(/,/,"",$2); gsub(/,/,"",$1); print "Name: "$3,"\tObjects: "$2,"\tSize: "$1 / 1024 / 1024" M"}'


List Containers Contents
------------------------

.. code:: bash

  curl -s -H "X-Auth-Token: ${AUTHTOKEN}" ${STORAGEURL}/${CONTAINER}


List Containers Contents In Long Format
---------------------------------------

.. code:: bash

  curl -s -H "X-Auth-Token: ${AUTHTOKEN}" ${STORAGEURL}/${CONTAINER}?format=json


List Containers Contents showing date code
------------------------------------------

.. code:: bash

  curl -s -H "X-Auth-Token: ${AUTHTOKEN}" ${STORAGEURL}/${CONTAINER}?format=json | python -m json.tool | awk -F '"' '/name/ || /last_modified/ {print $4}' | awk 'ORS=(FNR%2)?FS:RS' FS="\t"


List Containers Contents showing date code and only after a certain date
------------------------------------------------------------------------

.. code:: bash

  curl -s -H "X-Auth-Token: ${AUTHTOKEN}" ${STORAGEURL}/${CONTAINER}?format=json | python -m json.tool | awk -F '"' '/name/ || /last_modified/ {print $4}' | awk 'ORS=(FNR%2)?FS:RS' | awk '{gsub(/-/,"",$1); print}' | awk -F 'T' '{if ($1 >= 20120904) print $1,$2}'


Download an Object
------------------

.. code:: bash

  curl -H "X-Auth-Token: ${AUTHTOKEN}" ${STORAGEURL}/${CONTAINER}/${OBJECT} -o ${FILENAME}


Upload a File
-------------

.. code:: bash

  curl -X PUT -T "${FILENAME}" -H "X-Auth-Token: ${AUTHTOKEN}" ${STORAGEURL}/${CONTAINER}/${OBJECT}


Optional Attributes:

.. code:: bash

  -H "Content-type: CONTENT/TYPE"
  -H "Etag: md5sum"
  -H "X-Meta-whateveryouwant: your-var-here"


Change Object MetaData
----------------------

.. code:: bash

  curl -X POST -H "X-Auth-Token: ${AUTHTOKEN}" -H "X-Meta-CHANGE-ME: THE-META-DATA" ${STORAGEURL}/${CONTAINER}/${OBJECT}


Optional Attributes:

.. code:: bash

  -H "Content-type: CONTENT/TYPE"
  -H "Etag: md5sum"
  -H "X-Meta-whateveryouwant: your-var-here"


Change Container MetaData
-------------------------

.. code:: bash

  curl -X POST -H "X-Auth-Token: ${AUTHTOKEN}" -H "X-Meta-CHANGE-ME: THE-META-DATA" ${STORAGEURL}/${CONTAINER}


Optional Attributes:

.. code:: bash

  -H "X-Meta-CHANGE-ME: THE-META-DATA"


Get Container Metadata
----------------------

.. code:: bash

  curl -X HEAD -D --H "X-Auth-Token: ${AUTHTOKEN}" ${STORAGEURL}/${CONTAINER}


Get Object Metadata
-------------------

.. code:: bash

  curl -X HEAD -D --H "X-Auth-Token: ${AUTHTOKEN}" ${STORAGEURL}/${CONTAINER}/${OBJECT}


Delete an Object
----------------

.. code:: bash

  curl -X DELETE -H "X-Auth-Token: ${AUTHTOKEN}" ${STORAGEURL}/${CONTAINER}/${OBJECT}


Delete a Container
------------------

.. code:: bash

  curl -X DELETE -H "X-Auth-Token: ${AUTHTOKEN}" ${STORAGEURL}/${CONTAINER}


**you will get a '409 Error Processing' if the ${CONTAINER} is not empty**


Purge Object from CDN
---------------------

.. code:: bash

  curl -X DELETE -H "X-Auth-Token: ${AUTHTOKEN}" ${MGTCDNURL}/${CONTAINER}/${OBJECT}


Get CDN Object Metadata
-----------------------

.. code:: bash

  curl -X DELETE -D - -H "X-Auth-Token: ${AUTHTOKEN}" ${MGTCDNURL}/${CONTAINER}/${OBJECT}


Enable CDN For a Container
--------------------------

.. code:: bash

  curl -s -X PUT -D - -H "X-Log-Retention: True" -H "X-CDN-Enabled: True" -H "X-TTL: 259200" -H "X-Auth-Token: ${AUTHTOKEN}" ${MGTCDNURL}/${CONTAINER}


Disable CDN For a Container
---------------------------

.. code:: bash

  curl -s -X PUT -D - -H "X-Log-Retention: False" -H "X-CDN-Enabled: False" -H "X-TTL: 900" -H "X-Auth-Token: ${AUTHTOKEN}" ${MGTCDNURL}/${CONTAINER}


Testing a specific POP for CDN
------------------------------

.. code:: bash

  curl -H "Host: c123456.r02.cf2.rackcdn.com" http://example.com/yourpic.jpg
