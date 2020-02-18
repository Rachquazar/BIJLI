# Bijli - Decentralized Energy Economy

A key application of Blockchain being currently explored is a Decentralized Energy Economy. The idea stems from a neighborhood where certain Residents are producing energy through Solar panels or other means,and can sell excess energy to Residents needing energy. The transactions would be based on *bijli* coins in each Resident's account. As per a pre-determined contract and rate, the coins would be debited from the consumer and credited to the producer, for a certain billing period. Each transaction would need to be atomic and added to a Blockchain ledger for trust and verification. The network can include Banks to transact coins for Fiat currency (INR/USD) under a variable echange rate determined by the supply/demand in the distribnution. The network can have Utility Company who can buy or provide energy through the network.

### Highlights

* The cash value of bijli coins varies based on demand/supply of electricity, market reaches power equilibirum eventually.  
* Consistent high throughput data API for running analytics platforms enabling reliable forcasting. 
* Toshi integration for exchaning solarcoins.
* *Prepaid electricity* could be even more felxible than prepaid mobile phone services.  
* Since each smart meter could be part of the system, no special seperate computing hardware/administration required. 
* Donations can be made to fund street lights in community, or some poor community/institution or players could fund the stadium lights during the night to practice thier sport. 

# Architecture Flow

<p align="center">
  <img width="650" height="200" src="images/arch.png">
</p>

1. The administrator interacts with Decentralized Energy UI comprising of Angular framework
2. The application processes user requests to the network through a REST API.
3. Implements requests to the Blockchain state database on Hyperledger Fabric v1
4. The REST API is used to retrieve the state of the database
5. The Angular framework gets the data through GET calls to the REST API

# Included Components

* Hyperledger Composer
* Angular Framework
* Loopback


# Running the Application
Follow these steps to setup and run this developer journey. The steps are described in detail below.

## Prerequisite
- [Docker](https://www.docker.com/) (Version 17.03 or higher)
- [npm](https://www.npmjs.com/)  (v3.x or v5.x)
- [Node](https://nodejs.org/en/) (version 6.x - note version 7 is not supported)
  * to install Node v6.x you can use [nvm](https://davidwalsh.name/nvm)
- [Hyperledger Composer](https://hyperledger.github.io/composer/installing/development-tools.html)
  * to install composer cli
    `npm install -g composer-cli`
  * to install composer-rest-server
    `npm install -g composer-rest-server`
  * to install generator-hyperledger-composer
    `npm install -g generator-hyperledger-composer`

## Steps
1. [Clone the repo](#1-clone-the-repo)
2. [Setup Fabric](#2-setup-fabric)
3. [Generate the Business Network Archive](#3-generate-the-business-network-archive)
4. [Deploy to Fabric](#4-deploy-to-fabric)
5. [Run Application](#5-run-application)
6. [Create Participants](#6-create-participants)
7. [Execute Transactions](#7-execute-transactions)

## 1. Clone the repo

Clone the `Bijli code` locally. In a terminal, run:

`git clone https://github.com/slayerjain/bijli`

## 2. Setup Fabric

These commands will kill and remove all running containers, and should remove all previously created Hyperledger Fabric chaincode images:

```none
docker kill $(docker ps -q)
docker rm $(docker ps -aq)
docker rmi $(docker images dev-* -q)
```

Set Hyperledger Fabric version to v1.0-beta:

`export FABRIC_VERSION=hlfv1`

All the scripts will be in the directory `/fabric-tools`.  Start fabric and create profile:

```
cd fabric-tools/
./downloadFabric.sh
./startFabric.sh
./createComposerProfile.sh
./createPeerAdminCard.sh
```


## 3. Generate the Business Network Archive

Next generate the Business Network Archive (BNA) file from the root directory:

```
cd ../
npm install
```

The npm install also runs the `composer archive create` command, which has created a file called `bijli-network.bna` in the `dist` folder.


## 4. Deploy to Fabric

Now, we are ready to deploy the BNA file to Hyperledger Fabric, and import the busineess network card:

```
cd dist
composer runtime install --card PeerAdmin@hlfv1 --businessNetworkName bijli-network
composer network start --card PeerAdmin@hlfv1 --networkAdmin admin --networkAdminEnrollSecret adminpw --archiveFile bijli-network.bna --file bijliadmin.card
composer card import --file bijliadmin.card
```


You can verify that the network has been deployed by typing:

```
composer network ping --card admin@bijli-network
```

## 5. Run Application

First, go into the `angular-app` folder and install the dependency:

```
cd ../angular-app/
npm install
```

To start the application:
```
npm start
```

The application should now be running at:
`http://localhost:4200`

The REST server to communicate with network is available here:
`http://localhost:3000/explorer/`


## 6. Create Participants

Once the application opens, create participants and fill in dummy data.  Create Residents, Banks and Utility Companies.


## 7. Execute Transactions

Execute transactions manually between Residents, Resident and Bank, and Resident and Utility Company.  After executing transactions, ensure the participants account values are updated.


At the end of your session, stop fabric:

```
cd ~/fabric-tools
./stopFabric.sh
./teardownFabric.sh
```

## Additional Resources
* [Hyperledger Fabric Docs](http://hyperledger-fabric.readthedocs.io/en/latest/)
* [Hyperledger Composer Docs](https://hyperledger.github.io/composer/introduction/introduction.html)

## License
[Apache 2.0](LICENSE)
