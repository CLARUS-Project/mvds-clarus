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
* [**Current Version**](#current-version)
* [**Development Stage**](#development-stage)
* [**Requirements**](#requirements)
* [**Setup**](#setup)
* [**Important Notes**](#important-notes)
* [**Troubleshoot**](#troubleshoot)
* [**Support Team**](#support-team)
<!--* [**License**](#license)-->
<!--* [**Endpoints**](#endpoints)-->

## Architecture

![Architecture](docs/images/clarus-architecture.jpg)

## Current Version

Status: _in development stage_

## Development Stage

![Development Stage](docs/images/Development%20stage%201.png)
Status: _Work in Progress_

**Components deployed**

|    Component    | Progress |
| :-------------: | :------: |
| DAPS            | 100%     |
| CA              | 100%     |
| Metadata Broker | 80%      |

## Requirements

### Hardware Requirements

|  Name   |           Value           |
| :-----: | :-----------------------: |
|   RAM   | 4GB (8GB is reccomended)  |
| Storage |           50GB            |

It is recommended to use 64bit quad core processor to provide enough processing power for all docker containers. 

### Software Requirements

|      Name      |      Version     |             Notes        |
| :------------: | :--------------: | :----------------------: |
|     Docker     |    20.10.7       | [Docker][docker]         |
| Docker-compose |     1.27.4       | [Docker-compose][docker] |
|     Java       |       11         | [Java][java]             |
|     Maven      |      3.6.3       | [Maven][maven]           |
|     Ruby       |      2.7.0       | [Ruby][ruby]             |
|    Python      |        3         | [Python][python]         |
<!--|     Ubuntu     |   20.04.1 LTS    |--> 


## Setup

TODO...

<!--## Endpoints-->

## Important Notes

In the current state of development, the DAPS component has been properly integrated into MVDS.
The Metadata Broker still has some anomalies during interaction, investigations are being made into the cause of the problem.

## Troubleshoot

TODO...

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

[java]: https://docs.oracle.com/en/java/javase/11/ 

[maven]: https://maven.apache.org/guides/index.html

[ruby]: https://ruby-doc.org/

[python]: https://docs.python.org/3/

