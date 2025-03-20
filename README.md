<p align="center">
<img src="docs/images/clarus-logo.png" width="15%"/> <img src="docs/images/idsa-logo.png" width="45%"/> 
</p>

![Version Badge](https://img.shields.io/badge/Release-In%20Progress-green)

<!--the list of used link is at the bottom of the file-->

# Minimum Viable Data Space (MVDS) for Clarus Project
This repository provides a deployment scenario for a Minimum Viable Data Space ([MVDS][mvds]), defined according to the specifications of the International Data Spaces Association ([IDSA][idsa]). This solution is designed for data exchange within the CLARUS project.
The open-source TRUsted Engineering Connector ([TRUE Connector][true-connector]), developed by the Engineering group, will be utilized among the selected IDS components to implement this solution.
The installation and configuration procedures described in this document adhere to the guidelines established by the [IDS testbed][testbend].

## Table of contents
* [**Architecture**](#architecture)
* [**Development Status**](#development-status)
* [**Installation Requirements**](#installation-requirements)
* [**Setup**](#setup)
* [**Support Team**](#support-team)
<!--* [**License**](#license)-->
<!--* [**Endpoints**](#endpoints)-->

## Architecture

![Architecture](docs/images/clarus-architecture.jpg)

## Development Status

![Development Stage](docs/images/Development%20stage%201.png)
Status: _Work in Progress_

**Components deployed**

|    Component                  | Progress |
| :---------------------------: | :------: |
| Onejdn DAPS                   | 100%     |
| Metadata Broker               | 100%     |
| Clearing House                | 100%     |

## Installation Requirements

### Hardware Requirements

|  Name   |           Value           |
| :-----: | :-----------------------: |
|   RAM   | 4GB (8GB is reccomended)  |
| Storage |           50GB            |
|   CPU   |          6-Core           |

It is recommended to use 64bit quad core processor to provide enough processing power for all docker containers. 

### Software Requirements

|      Name      |      Version     |             Notes        |
| :------------: | :--------------: | :----------------------: |
|     Docker     |     24.0.7       | [Docker][docker]         |
| Docker-compose |     v2.21.0      | [Docker-compose][docker] |


## Setup

### DAPS (Omejdn)
This is the procedure to install and run the DAPS component, the configuration will use basic settings.
For a real scenario, a DNS and a proper Certification Authority are required.
 1. Download the source files from this repository. 
 2. Inside the “**DAPS**” folder there are the “**config**”, “**keys**” folders and the client registration script “**register_connector.sh**”.
 3.	The “**.env**” file contains several environment variables, each of them followed by a comment explaining the purpose.
 4. Set the variable “**OMEJDN_DOMAIN**” with the name of the domain to set up, e.g. “my-host.test”.
 5. For security purpose, set up a different username and password by editing “**ADMIN_USERNAME**” and “**ADMIN_PASSWORD**” variables.
 6. Now open the “**docker-compose.yml**” file and If it’s necessary change the host ports of “**Omejdn**” image which are currently set as 8080 and 8443 and **Save**.
 7. Run a command prompt or shell and execute the “**docker-compose pull**” command
 8. When the pull is completed, run the command “**docker-compose up -d**” to run the container.
 9. Now the DAPS service container should be up and running!
 Note: Check the “**Omejdn.yml**” file inside the “**config**” folder, where the value of the properties “**issuer**” and “**front_url**” should be the same of “**OMEJDN_ISSUER**” env variable.

For more details regarding the Omejdn server and how to use advanced featuers and configurations, please follow the official documentation 
provided by the Fraunhofer-AISEC team inside the component [GitHub repository][omejdn]

### TRUE Connector integration with DAPS
The following steps describe how to integrate a TRUE Connector instance with DAPS, for more infos and details about TRUE Connector, refer to the official repository [here][true-connector]:
 1. Generate **certificate-key pairs** (.cert and .key, respectively) using OpenSSL command, example:
 ```
 openssl req -x509 -nodes -newkey rsa:2048 -keyout test-tc-consumer-clarus.key -out test-tc-consumer-clarus.cert -sha256 -days 365 -subj "/C=IT/ST=Italy/L=Trento/O=Engineering Ingegneria Informatica SpA/OU=R&D/CN=clarus-data-space" -addext "subjectAltName=DNS:clarus-data-space"
 ```
 
 2. Now generate a **.p12 archive** with OpenSSL, using the above generated pairs:
 ```
 openssl pkcs12 -export -out clarus-data-space.p12 -inkey test-tc-consumer-clarus.key -in test-tc-consumer-clarus.cert -name clarus-data-space
 ```
 
 3. Set a password when asked
 
 4. Place the **.p12 archive** inside the **"cert"** folder of the respective TRUE Connector
 
 5. To register the connector as Client, place the **.cert** file generated on step 1 inside the **"DAPS/keys"** folder
 
 6. Now open a command shell from the **"DAPS"** folder where the **register_connector.sh** script is located and run the command:
  ```
 bash register_connector.sh test-tc-consumer-clarus.cert
  ```
  
  7. The client will be registered within the file **"config/clients.yml"**
  
  8. Restart the **DAPS docker container** to complete the registration, the registered client certificate is saved in the **"DAPS/keys/clients"** folder. The file name is encoded as **base64**, and it refers to the **client_id** value that is registered inside the **"config/clients.yml"** file.
  
  9. If the MVDS is hosted on a machine with a real DNS configured and a Certification Authority, then download the **public DAPS certificate** from the host.
  
  10. Back on TRUE Connector side, import the public DAPS certificate inside the connector trustore located inside the folder **"cert"**, example:
  ```
  keytool -import -file mvds-clarus.eu -keystore “./TRUEConnector/cert/ true-connector-consumer-truststore.jks" -alias 'mvds-clarus.eu (r3)' -storepass trustorePassword -noprompt
  ```
  Note: the trustorePassword is defined as variable named **TRUSTORE_PASSWORD** within the **.env** file of TRUE Connector.
  
  11. Edit the the below variables on **".env"** file of TRUE Connector:
  
	•	DAPS_KEYSTORE_NAME: must contain the name of “.p12” archive that was generated on step 2) of the above instructions (e.g. clarus-data-space.p12).
	•	DAPS_KEYSTORE_PASSWORD: input here the password defined for the “.p12” archive on step 3).
	•	DAPS_KEYSTORE_ALIAS: contain the alias defined on the command parameter “-name” used to generate the “.p12” archive (e.g., clarus-data-space).

  12. The **Host URLs for DAPS** must be set inside the properties of the **ECC component**, move to folder **"ecc-resources"** of TRUE Connector and open the file **"application-docker.properties"**.
  
  13. Find the property **application.isEnabledDapsInteraction** and set the value to **true**
  
  14. Now find the property **application.dapsUrl** and set the token endpoint of the DAPS as value (e.g., https://daps.mvds-clarus.eu/auth/token).
  
  15. On the property **application.dapsJWKSUrl** and set the jwks endpoint of the DAPS as value (e.g. https://daps.mvds-clarus.eu/auth/jwks.json).

Once the above configuration is completed, start the TRUE Connector using docker, and check the logs messages. If everything was set correctly the message **"Token is valid"** will be shown during each interaction and also at start up of the container.

### Metadata broker
 1. Inside the **“.env”** file set the value of the **“BROKER_DOMAIN”** with the actual host of the Metadata Broker.

 2. If necessary, open the **“docker-compose.yml”** and edit the metadata broker services ports.

 3. Run a command prompt or shell and execute the **“docker-compose pull”** command.

 4. After the pull is completed, run the command **“docker-compose up -d”** to up and running the container, the log will result like the figure below.

![Metadatabroker log](docs/images/Metadata%20broker%20log.png)

### TRUE Connector integration with Metadata broker
 1. Open a Browser and go to the Metadata Broker host url to download the certificate, then upload this certificate into the connector Trustore.

 2. Check the **"application-docker.properties"** file of ECC and look for the property called **"Application.selfdescription.brokerURL"** if the value already contains a parameter reference like **${BROKER_URL}** then move on next step else set here the Broker URL reference: https://YourBrokerhost/infrastructure.

 3. If the TRUE Connector instances are using Web Socket comunication protocol, then proceed with this step else move on the next one: In the same properties file, set the value of **“Application.selfdescription.registrateOnStartup”** as **“true”**, that option will enable the auto registration of the connector self-description when starting (or restarting) the connector container.

 4. Now check the **"docker-compose"** file and look for **"BROKER_URL"** within the **"environment"** variables of the ECC service, if it's value is a parameter reference like **${BROKER_URL}* then move on next step else put here the Broker URL reference: https://YourBrokerhost/infrastructure.

 5. Check the **".env"** file and look for **"BROKER_URL"** variable, set it to contain the Broker URL reference: https://YourBrokerhost/infrastructure

 6. Finally perform a **“docker-compose down”** and then **“docker-compose up”** commands to reset the containers of the connector.


<!--## Endpoints-->

## Support Team

| Name                        |        E-mail          |
| :-------------------------- | :--------------------: |
| [Luigi di Corrado][luigi]   | luigi.dicorrado@eng.it |
| [Gianluca Isgro'][gianluca] | gianluca.isgro@eng.it  |
| [Luca Di Lorenzo][luca]     | luca.dilorenzo@eng.it  |

<!--
## License
-->

<!--LIST OF LINKS USED-->

[luigi]: https://github.com/luidicorra

[gianluca]: https://github.com/gianluca-isgro

[luca]: https://github.com/ludilorenz

[mvds]: https://github.com/International-Data-Spaces-Association/IDS-testbed/blob/master/minimum-viable-data-space/MVDS.md

[idsa]: https://internationaldataspaces.org/

[true-connector]: https://github.com/Engineering-Research-and-Development/true-connector

[testbend]: https://github.com/International-Data-Spaces-Association/IDS-testbed/blob/master/InstallationGuide.md

[docker]: https://docs.docker.com/         

[omejdn]: https://github.com/Fraunhofer-AISEC/omejdn-server

[metadatabroker]: https://github.com/International-Data-Spaces-Association/metadata-broker-open-core

[clearinghouse]: https://github.com/International-Data-Spaces-Association/ids-clearing-house-service
