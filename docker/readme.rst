====
AKHQ
====

About
=====
**AKHQ** [1]_ (formerly: KafkaHQ) is an open source Kafka GUI to manage topics, topics data, consumers group and schema registry.

Version
-------
AKHQ version **0.20.0** deployed based on the official Docker Hub image: [2]_. 

License
-------
**Apache 2** [3]_

Pre-requisites
==============
* *docker* installed
* access to DIGITbrain private docker repo (username, password) to pull the image:
  
  - ``docker login dbs-container-repo.emgora.eu``
  - ``docker pull dbs-container-repo.emgora.eu/akhq:0.20.0``

Usage
=====
.. code-block:: bash

  docker run -d --rm \
      --name akhq \
      -e ADVERTISED_IP=<<kafka-domain-name-or-ip>> \
      -e SSL_PORT=9093 \
      -p 8080:8080 \
      akhq:0.20.0 

where ADVERTISED_IP parameter is the IP address or domain name of the Kafka server, 
SSL_PORT is the SSL port of the Kafka server (must be set, even if it is the default 9093),
and web UI port opened on host port 8080.

The open a browser with URL: https://<<container-host>>:8080/ (in Chrome type in: 'thisisunsafe' to proceed).

Security
========
The image uses SSL/TLS client authentication to connect to the Apache Kafka server; it contains
a DIGITbrain client certificate, which is signed by DIGITbrain CA. 

The web UI http\s://<<container-host>>:8080/ui/login/ uses self-signed certificate created by AKHQ at start,
which causes a security warning , and requires username and password: *digitbrain/digitbrain*.

You can override all these settings with your own (certificates, ports, application.yml), see *volumes* parameters below.

Configuration
=============

Environment variables
---------------------
.. list-table:: 
   :header-rows: 1

   * - Name
     - Example
     - Comment
   * - *Kafka server*
     - ``-e ADVERTISED_IP=xxx.xxx.xxx.xxx``
     - Apache Kafka host domain name or IP to connect
   * - *SSL port*
     - ``-e SSL_PORT=9093``
     - Username for a newly created user
   * - *Plaintext port*
     - ``-e PLAINTEXT_PORT=9092``
     - Unencrypted port of Kafka. To use this, you have to alter and mount your application.yml.

Ports
-----
.. list-table:: 
  :header-rows: 1

  * - Container port
    - Host port bind example
    - Comment
  * - *HTTP (web UI)*
    - ``-p 10008:8080``
    - Default AKHQ web UI port 8080 is opened as port 10008 on the host  

Volumes
-------
.. list-table:: 
  :header-rows: 1

  * - Name
    - Volume mount example
    - Comment
  * - *application.yml*    
    - ``-v $PWD/application.yml:/app/application.yml``
    - Custom application.yml. See [4]_ for details.
  * - *Java truststore*    
    - ``-v $PWD/certificates/kafka-server.truststore.jks:/app/kafka-server.truststore.jks``  
    - Contains the Certificate Authority (CA) certificate
  * - *Java keystore*    
    - ``-v $PWD/certificates/kafka-client.keystore.jks:/app/kafka-client.keystore.jks``  
    - Contains the client certificate and the private key

References
==========
.. [1] https://akhq.io

.. [2] https://hub.docker.com/r/tchiotludo/akhq

.. [3] https://github.com/tchiotludo/akhq

.. [4] https://github.com/tchiotludo/akhq/blob/dev/application.example.yml


