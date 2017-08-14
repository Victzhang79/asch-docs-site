title: 'Dapp Development Tutorial 1: Asch Dapp Hello World'
---

# 1 Basic Process

There are three types of net in Asch system, localnet, testnet, and mainnet. The later two, testnet and mainnet, are usually deployed online, which can be accessed by public network. Meanwhile the first type, localnet, as its name is running on local machine, which is actually a private chain with only one node. Localnet is designed to help developing and testing locally.

And the development of Dapp involves all of these types of network simultaneously, which is:
- step 1: developing and testing locally on the localnet
- step 2: testing on the testnet
- step 3: deploying officially on mainnet


# 2 Start localnet

You, as a Dapp developer, can run your own localnet after downloading [asch source code](https://github.com/sqfasd/asch).

```
git clone https://github.com/sqfasd/asch
```
Then you can install and operate the environment following the instructions in README file.

# 3 Install asch-cli

```
npm instal -g asch-cli
```

NOTE: DO NOT use `cnpm` from TAOBAO since there are some **bugs** in it.

# 4 Create an application in local

First enter your Asch source code folder, and make sure the localnet is running.

```
cd <asch source code dir>
node app.js
```

Then use `dapps` command of `asch-cli` to create an application:

```
asch-cli dapps -a
```

Then there will be a set of questions for you to answer, to generate the application's genesis block:

```
? Enter secret of your testnet account *******************************************************************************
# To input a primary password of the genesis account, which can be any of the main passwords of Asch system (the one that contains 12 words)

? Enter second secret of your testnet account if you have
# By default genesis account does not set second password, so we can just type the Enter key.

? Enter DApp name Hello Dapp
# The name of DApp, we choose "Hello Dapp"

? Enter DApp description Hello world demo for asch dapp
# The description of DApp, let's leave it empty.

? Enter DApp tags hello,asch,dapp
# The tags of DApp. It can be empty to be searched more efficiently.

? Choose DApp category
  1) Common
  2) Business
  3) Social
  4) Education
  5) Entertainment
  6) News
(Move up and down to reveal more choices)
  Answer:
# The type of DApp, you can choose it according to your business type. Just input the number in front of each option.

? Enter DApp link https://github.com/sqfasd/asch-hello/archive/master.zip
# Input the zip file of DApp source code (must end by zip). The installation process needs this link to download necessary source file.

? Enter DApp icon url https://www.asch.com/logo.png
# The URL of DApp's icon file.

? Do you want publish a inbuilt asset in this dapp? No
# Input Yes if you need an asset built in DApp. For the time being, input No.

? Enter public keys of dapp forgers - hex array, use ',' for separator 8065a105c785a08757727fded3a06f8f312e73ad40f1f3502e0232ea42e67efd
# Input the list of public keys of DApp's initial delegates, seperated by comma. You can dynamically add delegates later so right now you can just input one private key for genesis account.

Creating DApp genesis block
Fetching Asch Dapps SDK
Saving genesis block
Saving dapp meta information
Registering dapp in localnet
Done (DApp id is 6299140990391157236)

# Then the program can automatically register this DApp on localnet. In this case, our application ID is 6299140990391157236
```

# 5 The folder structure

Now we can see that there is a new folder added under `dapps`, named as the DApp ID just created.

```
ls -1 dapps/<dapp id>

blockchain.json         # the database description of DApp
config.json             # the configuration file of DApp, which mainly contains the list of seed nodes. Developers can also add other configurations in it.
dapp.json               # the meta information of DApp, including name, description, source code package, and etc. This file can also be used when registering the app to other networks.
genesis.json            # indicate the genesis blcok. This file is generated by CLI automatically, but also can be written by yourself, by which the assets of genesis account can be distributed with more flexibility.
index.js                # this file contains the entry of DApp
init.js                 # this file contains the initial code of each module.
LICENSE                 # this file describes the permit license of source code.
modules                 # main code
modules.full.json       # this file indicates all the modules need to be loaded. You can add other necessary modules here.
modules.genesis.json    # (the simplified version of modules.full.json, currently not need)
node_modules            #
package.json            #
public                  # this folders contains all front-end files
routes.json             # this file contains the configuration of http route. If you want to add new interface, you need to revise this file.
```
Don't worry about the complexity of the file structure, for the time being just having a first look is enough. 

All the esential files related to developers can be found in `modules/contracts/`

There are already 4 types of built-in contacts in this folder:

```
ls -1 dapps/<dapp id>/modules/contracts/

delegates.js            # registering delegate contract
insidetransfer.js       # in-chain transfer contract
outsidetransfer.js      # XAS deposit contract
withdrawaltransfer.js   # XAS withdraw contract
```

What developers need to do is to create new contact to run your business logic. That's all. It is no need to understand any other codes.

# 6 Set DApp's genesis password

It is necessary to set the primary password that we used before in genesis block, as well as corresponding DApp ID, in `dapp` field of `config.json`.

In the future when DApp is published in real network, it still needs a machine used to configure the primary password. NOTE: only one machine is required.

```
"params": {
  "6299140990391157236": [
    "someone manual strong movie roof episode eight spatial brown soldier soup motor"
  ]
}
```

# 7 Access the front end interface

Let's take a rest to experience the fundamental functions of sidechain via the front end of DApp.

You can find the DApp entry in installed applications list of wallet UI.
Or directly access the DApp URL: `localhost:4096/dapps/<dapp id>`

In this project, we can conduct multiple tasks such as deposit, in-chain transfer, and withdraw.

Currently deposit can only be operated via command line (will be built in main wallet in the future), and all other functions can be operated through this interface.

```
asch-cli dapps -d

? Enter secret *******************************************************************************
? Enter amount 100
? DApp Id 6299140990391157236
? Enter secondary secret (if defined)
? Host and port localhost:4096
null { success: true, transactionId: '10589988261732949004' }
10589988261732949004
```

Both deposit and withdraw are refreshed every 30 seconds. After a while we can see the balance in the interface has been already updated.