
<h1 align="center">Ontology </h1>
<h4 align="center">Version 0.8 </h4>

[![GoDoc](https://godoc.org/github.com/ontio/ontology?status.svg)](https://godoc.org/github.com/ontio/ontology)
[![Go Report Card](https://goreportcard.com/badge/github.com/ontio/ontology)](https://goreportcard.com/report/github.com/ontio/ontology)
[![Travis](https://travis-ci.org/ontio/ontology.svg?branch=master)](https://travis-ci.org/ontio/ontology)
[![Gitter](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/ontio/ontology?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge)

English | [中文](README_CN.md)

Welcome to Ontology's source code library!

Ontology is dedicated to creating a modularized, freely configurable, interoperable cross-chain, high-performance, and horizontally scalable blockchain infrastructure system. Ontology makes deploying and invoking decentralized applications easier.

The code is currently alpha quality, but is in the process of rapid development. The master code may be unstable; stable versions can be downloaded in the release page.

The public test network is described below. We sincerely welcome and hope more developers join Ontology.

## Features

- Scalable lightweight universal smart contract
- Scalable WASM contract support
- Crosschain interactive protocol (processing)
- Multiple encryption algorithm support
- Highly optimized transaction processing speed
- P2P link layer encryption (optional module)
- Multiple consensus algorithm support (VBFT/DBFT/RBFT/SBFT/PoW)
- Quick block generation time


## Contents

* [Build development environment](#build-development-environment)
* [Get Ontology](#get-ontology)
	* [Get from source code](#get-from-source-code)
	* [get from release](#get-from-release)
* [Server deployment](#server-deployment)
	* [Select network](#select-network)
		* [Public test network Polaris sync node deployment](#public-test-network-polaris-sync-node-deployment)
		* [Single-host deployment configuration](#single-host-deployment-configuration)
		* [Multi-hosts deployment configuration](#multi-hosts-deployment-configuration)
	* [Implement](#implement)
	* [ONT transfer sample](#ont-transfer-sample)
* [Contributions](#contributions)
* [Open source community](#open-source-community)
	* [Site](#site)
	* [Developer Discord Group](#developer-discord-group)
* [License](#license)

## Build development environment
The requirements to build Ontology are:

- Golang version 1.9 or later
- Glide (a third party package management tool)
- Properly configured Go language environment
- Golang supported operating system

## Get Ontology
### Get from source code

Clone the Ontology repository into the appropriate $GOPATH/src/github.com/ontio directory.

```
$ git clone https://github.com/ontio/ontology.git
```
or
```
$ go get github.com/ontio/ontology
```
Fetch the dependent third party packages with glide.

```
$ cd $GOPATH/src/github.com/ontio/ontology
$ glide install
```

Build the source code with make.

```
$ make
```

After building the source code sucessfully, you should see two executable programs:

- `ontology`: the node program/command line program for node control

### get from release
You can download at [release page](https://github.com/ontio/ontology/releases).

## Server deployment
### Select network
To run Ontology successfully,  nodes can be deployed by two ways:

- Public test network Polaris sync node deployment
- Single-host deployment
- Multi-hosts deployment

#### Public test network Polaris sync node deployment
1.Create account
- Through command line program, create wallet wallet.dat needed for node implementation.
    ```
    $ ./ontology account add -d
    use default value for all options
    Enter a password for encrypting the private key:
    Re-enter password:
    
    Create account successfully.
    Address:  TA9TVuR4Ynn4VotfpExY5SaEy8a99obFPr
    Public key: 120202a1cfbe3a0a04183d6c25ceff1e34957ace6e4899e4361c2e1a2bc3c817f90936
    Signature scheme: SHA256withECDSA
    ```
    Here's a example of host configuration:
   
    Directory structure

    ```shell
    $ tree
    └── ontology
        ├── ontology
        └── wallet.dat
    ```        

2.Start ontology  
  PS: There is no need of config.json file, will use the default setting.

**NOTE**: The format of wallet file has been changed. Old wallets can not be used now. Please generate new wallet.

#### Single-host deployment configuration

Create a directory on the host and store the following files in the directory:
- Node program + Node control program  `ontology`
- Wallet file`wallet.dat`

Run command `$ ./ontology --testmode` can start single-host test net.

Here's a example of single-host configuration:

- Directory structure

    ```shell
    $ tree
    └── ontology
        ├── ontology
        └── wallet.dat
    ```

#### Multi-hosts deployment configuration

We can perform a quick deployment by modifying the default configuration file `config.json`.

1. Copy related file into target host, including:

   - Default configuration file`config.json`
   - Node program `ontology`

2. Seed nodes configuration

   - Select at least one seed node out of 4 hosts and fill the seed node address into the `SeelList` of each configuration file. The format is `Seed node IP address + Seed node NodePort`.

3. Create wallet file

   - Through command line program, on each host create wallet wallet.dat needed for node implementation.
        ```
        $ ./ontology account add -d
        use default value for all options
        Enter a password for encrypting the private key:
        Re-enter password:

        Create account successfully.
        Address:  TA9TVuR4Ynn4VotfpExY5SaEy8a99obFPr
        Public key: 120202a1cfbe3a0a04183d6c25ceff1e34957ace6e4899e4361c2e1a2bc3c817f90936
        Signature scheme: SHA256withECDSA
        ```

4. Bookkeepers configuration

   - While creating a wallet for each node, the public key information of the wallet will be displayed. Fill in the public key information of all nodes in the `Bookkeepers` field of each node's configuration file.

     Note: The public key information for each node's wallet can also be viewed via the command line program:

        ```shell
        $ ./ontology account list -v
        * 1     TA9TVuR4Ynn4VotfpExY5SaEy8a99obFPr
                Signature algorithm: ECDSA
                Curve: P-256
                Key length: 256 bit
                Public key: 120202a1cfbe3a0a04183d6c25ceff1e34957ace6e4899e4361c2e1a2bc3c817f90936 bit
                Signature scheme: SHA256withECDSA
        ```

        Now multi-host configuration is completed, directory structure of each node is as follows:
        ```shell
        $ ls
        config.json ontology wallet.dat
        ```

A configuration file fragment can refer to the config-dbft.json file in the root directory.

### Implement

Run each node program in any order and enter the node's wallet password after the `Password:` prompt appears.
```
$ ./ontology --nodeport=20338 --rpcport=20336
$ - Input your wallet password
```

Run `./ontology --help` for details.

### ONT transfer sample
 -- from: transfer from； -- to: transfer to； -- amount: ont amount；
```shell
  ./ontology asset transfer  --to=TA4Xe9j8VbU4m3T1zEa1uRiMTauiAT88op --amount=10
```
If transfer asset successd, the result will show as follow:

```
Transfer ONT
From:TA6edvwgNy3c1nBHgmFj8KrgQ1JCJNhM3o
To:TA4Xe9j8VbU4m3T1zEa1uRiMTauiAT88op
Amount:10
TxHash:10dede8b57ce0b272b4d51ab282aaf0988a4005e980d25bd49685005cc76ba7f
```
TxHash is the transfer transaction hash, we can query transfer result by txhash.
Because of generate block time, the transfer transaction will not execute befer at least generate one block.

### Query transfer status sample

--hash:transfer transaction hash
```shell
./ontology asset status --hash=10dede8b57ce0b272b4d51ab282aaf0988a4005e980d25bd49685005cc76ba7f
```
result：
```shell
Transaction:transfer success
From:TA6edvwgNy3c1nBHgmFj8KrgQ1JCJNhM3o
To:TA4Xe9j8VbU4m3T1zEa1uRiMTauiAT88op
Amount:10
```

### Query account balance sample

--address: account address

```shell
./ontology asset balance --address=TA4Xe9j8VbU4m3T1zEa1uRiMTauiAT88op
```
result：
```shell
BalanceOf:TA4Xe9j8VbU4m3T1zEa1uRiMTauiAT88op
ONT:10
ONG:0
ONGApprove:0
```

## Contributions

Please open a pull request with a signed commit. We appreciate your help! You can also send your code as emails to the developer mailing list. You're welcome to join the Ontology mailing list or developer forum.

Please provide detailed submission information when you want to contribute code for this project. The format is as follows:

Header line: explain the commit in one line (use the imperative).

Body of commit message is a few lines of text, explaining things  in more detail, possibly giving some background about the issue  being fixed, etc.

The body of the commit message can be several paragraphs. Please do proper word-wrap and keep columns shorter than 74 characters or so. That way "git log" will show things  nicely even when it is indented.

Make sure you explain your solution and why you are doing what you are  doing, as opposed to describing what you are doing. Reviewers and your  future self can read the patch, but might not understand why a  particular solution was implemented.

Reported-by: whoever-reported-it &
Signed-off-by: Your Name [youremail@yourhost.com](mailto:youremail@yourhost.com)

## Open source community
### Site

- <https://ont.io/>

### Developer Discord Group

- <https://discord.gg/4TQujHj/>

## License

The Ontology library is licensed under the GNU Lesser General Public License v3.0, read the LICENSE file in the root directory of the project for details.
