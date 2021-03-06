# Meetup 1: Noeud bitcoin et transaction
Pré-requis: Il est demandé aux participants d'avoir un ordinateur linux ou windows avec plus de 16 GB de libre et une bonne connection internet. Lors de ce premier meetup, 1GB sera téléchargé. Pour les utilisateurs de windows, nous conseillons d'installer virtualBox, de télécharger l'image du CD (fichier iso) d'ubuntu, de créer un nouvelle machine virtuelle et d'y installer ubuntu. L'installation d'ubuntu peut prender 30 minutes. Des explications additionnelles sont données ici @@@.

L'objectif est de faire tourner un noeud bitcoin regtest, de faire des transactions (P2PKH) et de comprendre (introduction) les transactions.

## Noeud: compilation, installation et configuration

### VirtualBox / VMware
La majeure partie des attaques visant les ordintaeurs des particuliers (virus et trojans) concernent le système d'exploitation Windows tandis que peu/pas de virus et menaces existent sur Linux. C'est pour cela que Linux est préféré à Windows pour les cryptomonnaies. Néanmoins, il faut veiller à se renseigner afin de correctement configurer et sécuriser son système Linux. La plupart des tutoriaux techniques sur les cryptomonnaies utilisent Linux alors que la majeure partie des internautes utilisent Windows. Des outils comme VirtualBox permettent de faire tourner une machine virtuelle sur un PC Windows qui contiendra elle-même un système Linux. Il est possible de sauvegarder l'entièreté de la machine virtuelle en copiant collant les fichiers qui la composent (après l'avoir éteinte).

... lien + printscreens
- télécharger l'ISO du CD d'ubuntu ou debian
https://releases.ubuntu.com/18.04.4/
+ verify checksum
- télécharger virtualbox
https://www.virtualbox.org/wiki/Downloads
- Installer VirtualBox
- Cliquer sur Nouvelle
- Type:Linux, Ubuntu
- 1 GB de RAM, disque de 10 GB
- cliquer sur suivant jusqu'à ce que la VM soit créée
- cliquer droit sur la VM dans le menu à gauche, configuration
- cliquer sur stockage dans le menu à gauche, sélectionner le CD vide, les attributs à droites s'adaptent
- cliquer sur l'icône de CD bleue dans les attributs à droite du menu déroulant "Lecteur optique"
- cliquer sur Choose a disk file


+ verify checksum

### Linux: ubuntu/Debian
Les versions majeures (les plus stables) de bitcoin existent dans le repo ### donc sur ubuntu/debian, il est possible d'installer bitcoin sans devoir le compiler. Les binaires génériques fonctionneront mais une compilation sur la machine réelle permet d'avoir de meilleures performances pour la machine.

... Lien + traduire les instructions

### Windows

... Lien + traduire les instructions

### Configuration
Explications des principales options et paramètres

... Lien + traduire les instructions

#### Réseaux mainet, testnet, regtest, simnet

#### En pratique, synchroniser son noeud
lancer bitcoind sur le regtest et miner 10 blocks
La blockchain de bitcoin testnet fait plus de 12 GB, on l'utilisera au prochain meetup. Pour ce premier meetup, on va utiliser le regtest où votre noeud sera seul et vous devrez tout faire (votre noeud étant le seul du réseau, il doit tout faire: miner, faire des transactions alors que sur le testnet ou mainet, il faut attendre pour que des blocs soient minés).

+++ commandes

## Rappels
### Fonction de hash

MD5, SHA1, SHA256D, HASH160

### Chifrement symétrique
3DES
### Chifrement asymétrique
RSA, EC

## Adresse (clé publique) et clé privée
A partir d'une clé privée, je peux calculer la clé publique (pubkey) qui permet d'obtenir l'adresse bitcoin (en fonction du réseau: testnet ou mainet).

## Transactions

### Types de transactions
Avant l'arrivée de segwit, il y avait 5 types standards de scripts de transaction: pay-to-public-key-hash (P2PKH), public-key, multi-signature (limited to 15 keys), pay-to-script-hash (P2SH), et data output (OP_RETURN).

The five standard types of transaction scripts are pay-to-public-key-hash (P2PKH), public-key, multi-signature (limited to 15 keys), pay-to-script-hash (P2SH), and data output (OP_RETURN), which are described in more detail in the following sections.

Avec segwit sont arrivées les transactions P2WPKH (Pay to Witness Public Key Hash), P2WSH (Pay to Witness Script Hash) et P2W* over P2SH (e.g. P2WPKH over P2SH).

https://programmingblockchain.gitbook.io/programmingblockchain/other_types_of_ownership/p2wpkh_pay_to_witness_public_key_hash
https://programmingblockchain.gitbook.io/programmingblockchain/other_types_of_ownership/p2wsh_pay_to_witness_script_hash
https://programmingblockchain.gitbook.io/programmingblockchain/other_types_of_ownership/p2w_over_p2sh

#### Pay-to-Public-Key-Hash (P2PKH)
The vast majority of transactions processed on the bitcoin network are P2PKH transactions. These contain a locking script that encumbers the output with a public key hash, more commonly known as a bitcoin address. Transactions that pay a bitcoin address contain P2PKH scripts. An output locked by a P2PKH script can be unlocked (spent) by presenting a public key and a digital signature created by the corresponding private key.

For example, let’s look at Alice’s payment to Bob’s Cafe again. Alice made a payment of 0.015 bitcoin to the cafe’s bitcoin address. That transaction output would have a locking script of the form:

OP_DUP OP_HASH160 <Cafe Public Key Hash> OP_EQUAL OP_CHECKSIG
The Cafe Public Key Hash is equivalent to the bitcoin address of the cafe, without the Base58Check encoding. Most applications would show the public key hash in hexadecimal encoding and not the familiar bitcoin address Base58Check format that begins with a “1”.

The preceding locking script can be satisfied with an unlocking script of the form:

<Cafe Signature> <Cafe Public Key>

#### Pay-to-Public-Key (P2PK)
Pay-to-public-key is a simpler form of a bitcoin payment than pay-to-public-key-hash. With this script form, the public key itself is stored in the locking script, rather than a public-key-hash as with P2PKH earlier, which is much shorter. Pay-to-public-key-hash was invented by Satoshi to make bitcoin addresses shorter, for ease of use. Pay-to-public-key is now most often seen in coinbase transactions, generated by older mining software that has not been updated to use P2PKH.

A pay-to-public-key locking script looks like this:

<Public Key A> OP_CHECKSIG
The corresponding unlocking script that must be presented to unlock this type of output is a simple signature, like this:

<Signature from Private Key A>

#### Multi-Signature
Multi-signature scripts set a condition where N public keys are recorded in the script and at least M of those must provide signatures to release the encumbrance. This is also known as an M-of-N scheme, where N is the total number of keys and M is the threshold of signatures required for validation. For example, a 2-of-3 multi-signature is one where three public keys are listed as potential signers and at least two of those must be used to create signatures for a valid transaction to spend the funds. At this time, standard multi-signature scripts are limited to at most 15 listed public keys, meaning you can do anything from a 1-of-1 to a 15-of-15 multi-signature or any combination within that range. The limitation to 15 listed keys might be lifted by the time this book is published, so check the isStandard() function to see what is currently accepted by the network.

#### Data Output (OP_RETURN)
Bitcoin’s distributed and timestamped ledger, the blockchain, has potential uses far beyond payments.

the OP_RETURN operator creates an explicitly provably unspendable output, which does not need to be stored in the UTXO set. OP_RETURN outputs are recorded on the blockchain, so they consume disk space and contribute to the increase in the blockchain’s size, but they are not stored in the UTXO set and therefore do not bloat the UTXO memory pool and burden full nodes with the cost of more expensive RAM.

#### Pay-to-Script-Hash (P2SH)
Pay-to-script-hash (P2SH) was developed to resolve these practical difficulties and to make the use of complex scripts as easy as a payment to a bitcoin address. With P2SH payments, the complex locking script is replaced with its digital fingerprint, a cryptographic hash. When a transaction attempting to spend the UTXO is presented later, it must contain the script that matches the hash, in addition to the unlocking script. In simple terms, P2SH means “pay to a script matching this hash, a script that will be presented later when this output is spent.”

Table 5-4. Complex script without P2SH
Locking Script
2 PubKey1 PubKey2 PubKey3 PubKey4 PubKey5 5 OP_CHECKMULTISIG
Unlocking Script
Sig1 Sig2

Table 5-5. Complex script as P2SH
Redeem Script
2 PubKey1 PubKey2 PubKey3 PubKey4 PubKey5 5 OP_CHECKMULTISIG
Locking Script
OP_HASH160 <20-byte hash of redeem script> OP_EQUAL
Unlocking Script
Sig1 Sig2 redeem script

P2SH addresses use the version prefix “5”, which results in Base58Check-encoded addresses that start with a “3”.

https://www.oreilly.com/library/view/mastering-bitcoin/9781491902639/ch05.html

### Signature bitcoin



## Faire une transaction
### Créer une adresse
getnewaddress "legacy"
### Envoyer des bitcoins (P2PKH)

#### sendtoaddress rpc
sendtoaddress rpc: choix des inputs automatique
miner des blocs sur le regtest (ou attendre que des blocs soient minées sur le testnet ou mainet)

#### raw transaction rpc
permet de sélectionner les inputs, grand risque de perdre des bitcoins si on oublie le change
listunspent
Copier coller les détails d'une UTXO dans notepad.
createrawtransaction
signrawtransaction
sendrawtransaction

https://en.bitcoin.it/wiki/Raw_Transactions#createrawtransaction_.5B.7B.22txid.22:txid.2C.22vout.22:n.7D.2C....5D_.7Baddress:amount.2C....7D

#### hex raw transaction à la main
Il est possible d'écrire l'hexadécimal manuellement (sans utiliser le programme bitcoin). On peut écrire sois-même le résultat de createrawtransaction. Cela demande une bonne connaissance du protocole et du détail des transactions (sujet du prochain meetup tech).



# Meetup 2: Qu'est-ce qu'une adresse bitcoin, une signature et comment fonctionne une transaction?
Pré-requis: Il est demandé aux participants d'avoir synchronisé bitcoin sur le testnet (blockchain de 12 GB), des bitcoins testnet seront donnés donc pas besoin d'avoir des bitcoins testnet. Les participants peuvent préparer des images, ils pourront s'échanger ces images contre les bitcoins testnet (sans valeur monétaire).

L'objectif est de faire tourner un noeud bitcoin sur le réseau testnet et faire des transactions (P2PKH) entre les participants. L'exposé portera sur le détail des adresses, transactions et signatures bitcoin dans le cadre de transactions P2PKH.

## Introduction

## Rappels
### raw transaction rpc
### hex raw transaction à la main

## Calcul d'une adresse PKH (bash & openssl)

## Transaction bitcoin et signature bitcoin
avec bitcoin, avec openssl
