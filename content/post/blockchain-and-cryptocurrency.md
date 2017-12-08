+++
banner = "banners/placeholder.png"
categories = ["UFRN", "blockchain", "cryptocurrency"]
date = "2017-12-07T16:53:00-03:00"
menu = ""
tags = ["UFRN", "bitcoin", "ethereum", "blockchain", "cryptocurrency", "implementation", "english"]
title = "Blockchain and cryptocurrency"
+++

## Introduction
These days a lot of new technologies have been created since the posts of Satoshi Nakamoto about the blockchain and cryptocurrency. But who is Satoshi and what is a blockchain? well, nobody knows who is him because this is a pseudonym, but his (or her) creation have a great potential to change the world, starting with the economy.
The incredible invention is the blockchain. I can resume this technology as a "confidence protocol", but it's much more than that. The focus is decentralize the data to be safer and guarantee that everything inside the block of information is valid. Basically, there are different nodes (computers) with the same shared public list of registers and data. With the shared list same nodes validate every information that requests to be part of the block of information (the block) and after the validation, every node knows about the new data, so insert unchecked input or to modify something into the list is necessary to modify in all nodes.    
This new technology can help a lot, especially with data that that must be used only after validation. The most famous and common case is the cryptocurrency. It's a new way to validate, organize and won currency. The first cryptocurrency to use the blockchain was the famous bitcoin. This currency is the inspiration for a lot of new altcoins like Ethereum and Monero. These coins have different abilities to different scenarios, this makes the currency safer (because of the blockchain) and easier to supply the needs of each one.

### The blockchain

The blockchain can be resumed as a distributed database that stores permanently a data (usually a currency transaction). The blockchain have blocks, each block has same informations and when the filling block is full another one is generated.  
The name is blockchain because of how it works. The blockchain is made by blocks (like a list data structure, a block have the hash of the previus block), these blocks are the where the data is stored, the blockchain is distributed to every computer that wants to access the data stored and all the block are download, starting by the last to the genesis block (the fisrt block), this guaranty that if someone tries to modify what is inside the others computers (nodes) going to reject the modification (block + chain, a block filled with data and distributed do everyone).  
The distributed block uses a network to be distributed. This network is P2P (peer-to-peer) because with that there's no need for a server that will regulate anything. Depending on the implementation each node knows every node of the network or each node know at least one node of the network, with one of these characteristics all the nodes going to have the same blockchain (the data will always update).

### Cryptocurrency

Since 2010~2012 a lot of peoples has listened about cryptocurrency and a lot was speculated. But what are altcoin and cryptocurrency? well, to start it's necessary to know that altcoin is all the virtual coins created to virtual transactions. Cryptocurrency is the set of all altcoins that use the blockchain, to be more specific the altcoins that use the encrypt ability of the blockchain.  
The Bitcoin and Ethereum are the most famous cryptocurrency. The first cryptocurrency was the bitcoin, created in 2008, using the first blockchain generation. Recently (in december of 2017) the bitcoin has the value of ten thousand dollars per coin. It's the biggest value since the creation of the currency. And Ethereum was created in 2012 and since this year the value has grown 4250%.  
Because of the blockchain, the altcoins can't be regulated by anyone, not even by the govern. To use it's necessary to download the wallet to the computer, this will generate a private and a public key. The private key is to decrypt the transactions made to you, and the public to when someone wants to send you a number of coins.
But are cryptocurrency 100% trustable? no, unfortunately, the most attractive characteristic of the altcoins (no one can try to regulate this currency) it's at the same time a problem. For any reason, the price of the bitcoin, for example, can grow or drow in seconds. So, if the bitcoin had a govern behind, would be easier predict the price in the next days or even months, but without a responsable organization regalamenting the cryptocurrencys, the virtual coins can be a gold miner or a black hole.

#### Now, came implement a blockchain together

To do this you going to need the NodeJS installed and with NPM download the module "crypto-js", use the command:

```
$  npm install crypto-js
```

Now let's start. First we going to import the "crypto-js" to have access to the SHA256 that going the generate the hash of each block:

```
const SHA = require("crypto-js/sha256");
```

Create the class Block class, that going to be responsible to instanciate the block object:

```
  class Block {}
```

Now create a constructor that receive the index of the block at the chain (the list of blocks), the timestamp, the data, and the hash of the previus block:

```
  constructor(index, timestamp, data, previusHash = '') {
    this.index = index;
    this.timestamp = timestamp;
    this.data = data;
    this.previusHash = previusHash;
    this.hash = this.calculateHash();
  }
```

As you can see the value of the hash of the actual block are generated by a method called calculateHash(), we need to use the function SHA256 import from the "crypto-js":

```
  calculateHash() {
    return SHA(this.index + this.timestamp + this.previusHash + JSON.stringfy(this.data)).toString();
  }
```

With the above code you shoult have this:

```
  const SHA = require("crypto-js/sha256");

    class Block {
      constructor(index, timestamp, data, previusHash = '') {
        this.index = index;
        this.timestamp = timestamp;
        this.data = data;
        this.previusHash = previusHash;
        this.hash = this.calculateHash();
      }

      calculateHash() {
        return SHA(this.index + this.timestamp + this.previusHash + JSON.stringfy(this.data)).toString();
      }
    }
```

Now that we have the Block we need the Blockchain class:

```
  class Blockchain {}
```

The constructor have no parameters and create an array with a single element inside, the genesis block (the fisrt block):

```
  constructor() {
    this chain = [];
    chain.push(this.generateGenesis());
  }
```

The genesis block going be generated by the method generateGenesis() to avoid nodes with diferent genesis:

```
  generateGenesis() {
    return new Block(0, "07/12/2017", "Genesis block", "0");
  }
```

The above code guarantee that there's no different genesis block.

Now we need to create the helper methods:

```
  getLatestBlock() {
    return this.chain[this.chain.length - 1];
  }

  addBlock(newBlock) {
    newBlock.previusHash = this.getLatestBlock(); // the obj have a new previusHash
    newBlock.hash = newBlock.calculateHash(); // to regenerate the hash
    this.chain.push(newBlock);
  }
```

So, the class final code is:

```
    class Blockchain {
      constructor() {
        this chain = [];
        chain.push(this.generateGenesis());
      }

      generateGenesis() {
        return new Block(0, "07/12/2017", "Genesis block", "0");
      }

      getLatestBlock() {
        return this.chain[this.chain.length - 1];
      }

      addBlock(newBlock) {
        newBlock.previusHash = this.getLatestBlock(); // the obj have a new previusHash
        newBlock.hash = newBlock.calculateHash(); // to regenerate the hash
        this.chain.push(newBlock);
      }
    }
```

Now we need to test the above code. Just create a BlockChain and add blocks to it:

```
  let blockchain = new Blockchain();
  blockchain.addBlock(new Block(1, "07/12/2018", "second block"));
  blockchain.addBlock(new Block(2, "07/12/2019", "third block"));

  console.log( JSON.stringfy(blockchain, null, 4) );
```

With this we should see the Blockchain javascript object print in the console.
Thank you very much to read my first blog post !!! :D !!! see you next post !!!

references:

* [blockchain wikipedia page](https://pt.wikipedia.org/wiki/Blockchain)
* [bitcoin an ethereum (portuguese)](https://g1.globo.com/tecnologia/noticia/bitcoin-e-ethereum-o-que-e-e-como-funciona-a-criptomoeda-ou-moeda-virtual.ghtml)
* [creating a blockchain](https://www.youtube.com/watch?v=zVqczFZr124)
