Using cURL to Authenticate
###########################
:date: 2013-09-05 09:51
:tags: rackspace, Cloud, api, authentication
:category: \*nix

Variables For Access
--------------------

These Variables are not needed though can help with scripting. These are also placed here so you can see the commands easier

.. code:: bash

  USERNAME=" YOUR USERNAME"
  APIKEY=" YOUR API KEY"


Authentication v1.0
-------------------

* US - `AUTHURL='https://auth.api.rackspacecloud.com/v1.0'`
* UK - `AUTHURL='https://lon.auth.api.rackspacecloud.com/v1.0'`


.. code:: bash

  curl -D - -H " X-Auth-User: ${USERNAME}" -H " X-Auth-Key: ${APIKEY}" ${AUTHURL}


Authentication v2.0
-------------------

US - `AUTHURL='https://identity.api.rackspacecloud.com/v2.0/'`
UK - `AUTHURL='https://lon.identity.api.rackspacecloud.com/v2.0/'`


You can authenticate against v2.0 API by two ways. You can request a token by providing your username and your API key or username and password.


Username and APIkey authentication
----------------------------------

.. code:: bash

  curl -X POST ${AUTHURL}/tokens -d "{\"auth\":{ \"RAX-KSKEY:apiKeyCredentials\":{\"username\":\"${USERNAME}\", \"apiKey\":\"${APIKEY}\" }}}" -H "Content-type: application/json" | python -m json.tool


Username and Password authentication
------------------------------------


.. code:: bash

  curl -X POST ${AUTHURL}/tokens -d "{\"auth\":{ \"passwordCredentials\":{\"username\":\"${USERNAME}\", \"password\":\"${PASSWORD}\" }}}" -H "Content-type: application/json" | python -m json.tool


Example of authenticating against identity w/ xml format
--------------------------------------------------------

.. code:: bash

  curl -X POST ${AUTHURL}/tokens -d "{\"auth\":{\"passwordCredentials\":{\"username\":\"${USERNAME}\",\"password\":\"${PASSWORD}\"}}}" -H "Content-type: application/json" -H "Accept: application/xml" | tidy -xml


Example JSON structure when POST'ing data for Authentication
------------------------------------------------------------

apiKey Authentication:

  .. code:: json

  {
    "auth":{
      "RAX-KSKEY:apiKeyCredentials":{
        "username": "${USERNAME}",
        "apiKey": "${APIKEY}"
      }
    }
  }


Password Authentication:

  .. code:: json

  {
    "auth":{
      "passwordCredentials":{
        "username": "${USERNAME}",
        "password": "${PASSWORD}"
      }
    }
  }
