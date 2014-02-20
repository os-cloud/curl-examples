Using cURL with Cloud Load Balancers
####################################
:date: 2013-09-05 09:51
:tags: rackspace, Cloud Load Balancer, lb, api
:category: \*nix


Variables For Access
--------------------

These Variables are not needed though can help with scripting. These are also placed here so you can see the commands easier.


.. code:: bash

  AUTHTOKEN="Authorization Token"
  FILENAME="Location and File Name"
  DATACENTER="What is the Data Center Designation"
  DDI="Account Number"
  LBID="ID Number of the Load Balancer"



Create a Custom Error Page
--------------------------

.. code:: bash

  curl -D - -H "X-Auth-Token: ${AUTHTOKEN}" -H "content-type: application/json" -T "$FILENAME" -X PUT https://${DATACENTER}.loadbalancers.api.rackspacecloud.com/v1.0/${DDI}/loadbalancers/${LBID}/errorpage


JSON Example:

.. code:: json

  {
    "errorpage": {
      "content": "\n<html>\n<h1>FAIL</h1>\n</html>"
    }
  }


Upload SSL Certificate
----------------------

.. code:: bash

  curl -D - -X PUT -H "X-AUTH-TOKEN: ${AUTHTOKEN}" -H "Content-type: application/json" --data-binary  @${FILENAME} https://${DATACENTER}.loadbalancers.api.rackspacecloud.com/v1.0/${DDI}/loadbalancers/${LBID}/ssltermination


JSON Example:

.. code:: json

  {
    "certificate": "-----BEGIN CERTIFICATE-----\nADD YOUR KEY RELATED CONTENTS HERE\n-----END CERTIFICATE-----\n",
    "enabled":true,
    "secureTrafficOnly":false,
    "privatekey":"-----BEGIN RSA PRIVATE KEY-----\nADD YOUR CERTIFICATE RELATED CONTENTS HERE\n-----END RSA PRIVATE KEY-----\n",
    "intermediateCertificate":"-----BEGIN CERTIFICATE-----\nADD YOUR CERTIFICATE RELATED CONTENTS HERE\n-----END CERTIFICATE-----\n",
    "securePort":443
  }


Update Load Balancer To Use SSL
-------------------------------

.. code:: bash

  CURL -D - -X PUT -H "X-AUTH-TOKEN: ${AUTHTOKEN}" -H "Content-type: application/json" --data-binary  @${FILENAME} https://${DATACENTER}.loadbalancers.api.rackspacecloud.com/v1.0/${DDI}/loadbalancers/${LBID}/


JSON Example:

.. code:: json

  {
    "enabled": "true",
    "securePort": 443,
    "secureTrafficOnly": "true"
  }
