<p align="center">
<img src="docs/images/clarus-logo.png" width="15%"/> <img src="docs/images/idsa-logo.png" width="45%"/> 
</p>

![Version Badge](https://img.shields.io/badge/Release-In%20Progress-green)

<!--the list of used link is at the bottom of the file-->

# Minimum Viable Data Space (MVDS) for Clarus Project
This repository will provide a deployment scenario of a Minimum Viable Data Space ([MVDS][mvds]), defined using the International Data Spaces Assosiation ([IDSA][idsa]) specification, as a solution for data exchange on the CLARUS project.
The open source TRUsted Engineering Connector ([TRUE Connector][true-connector]), developed by the Engineering group, will be used among the IDS components chosen to implement this solution.
The installation and configuration on which this paper will be based will refer to the guidelines defined on the [IDS testbed][testbend].
The project is currently in an early state of development, therefore, more information will be included as development progresses.

## Table of contents
* [**Architecture**](#architecture)
* [**Development Status**](#development-status)
* [**Installation Requirements**](#installation-requirements)
* [**Setup**](#setup)
* [**Important Notes**](#important-notes)
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
| Metadata Broker               | 90%      |
| Clearing House                | 10%      |

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

### MVDS Components
This is the procedure to install and run the MVDS in a local environment, the configuration will use basic settings.
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
TO DO...
<!--## Endpoints-->

## Important Notes

In the current state of development, the DAPS component has been properly integrated into MVDS.
The Metadata Broker still has some anomalies during interaction, investigations are being made into the cause of the problem.

## Support Team

| Name                      |        E-mail         |
| :------------------------ | :-------------------: |
| [Luigi di Corrado][luigi] | luigi.dicorrado@eng.it|
| [Luca Di Lorenzo][luca]   | luca.dilorenzo@eng.it |

<!--
## License
-->

<!--LIST OF LINKS USED-->

[luigi]: https://github.com/luidicorra

[luca]: https://github.com/ludilorenz

[mvds]: https://github.com/International-Data-Spaces-Association/IDS-testbed/blob/master/minimum-viable-data-space/MVDS.md

[idsa]: https://internationaldataspaces.org/

[true-connector]: https://github.com/Engineering-Research-and-Development/true-connector

[testbend]: https://github.com/International-Data-Spaces-Association/IDS-testbed/blob/master/InstallationGuide.md

[docker]: https://docs.docker.com/         

[omejdn]: https://github.com/Fraunhofer-AISEC/omejdn-server

