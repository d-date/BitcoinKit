BitcoinKit
===========

BitcoinKit implements Bitcoin protocol in Swift. It is an implementation of the Bitcoin SPV protocol written (almost) entirely in swift.

Features
--------

- Send/receive transactions.
- See current balance in a wallet.
- Encoding/decoding addresses: P2PKH, WIF format.
- Transaction building blocks: inputs, outputs, scripts.
- EC keys and signatures.
- BIP32, BIP44 hierarchical deterministic wallets.
- BIP39 implementation.

Initial Setup
-------------

#### Build dependencies

```
sh ./setup/build_libraries.sh
```

Usage
-----

#### Creating new wallet

```swift
let privateKey = PrivateKey(network: .testnet) // You can choose .mainnet or .testnet
let wallet = Wallet(privateKey: privateKey)
```

#### Import wallet from WIF

```swift
let wallet = try Wallet(wif: "92pMamV6jNyEq9pDpY4f6nBy9KpV2cfJT4L5zDUYiGqyQHJfF1K")
```

#### Hierarchical Deterministic Wallet

```swift
// Generate mnemonic
let mnemonic = try Mnemonic.generate()

// Generate seed from the mnemonic
let seed = Mnemonic.seed(mnemonic: mnemonic)

let wallet = HDWallet(seed: seed, network: .testnet)
```

#### Key derivation

```
let mnemonic = try Mnemonic.generate()
let seed = Mnemonic.seed(mnemonic: mnemonic)

let privateKey = HDPrivateKey(seed: seed, network: .testnet)

// m/0'
let m0prv = try! privateKey.derived(at: 0, hardened: true)

// m/0'/1
let m01prv = try! m0prv.derived(at: 1)

// m/0'/1/2'
let m012prv = try! m01prv.derived(at: 2, hardened: true)
```

#### Extended Keys

```
let extendedKey = privateKey.extended()
```

#### Sync blockchain

```
let blockStore = try! SQLiteBlockStore.default()
let blockChain = BlockChain(wallet: wallet, blockStore: blockStore)

let peerGroup = PeerGroup(blockChain: blockChain)
let peerGroup.delegate = self

let peerGroup.start()
```

Installation
------------

### Carthage

BitcoinKit is available through [Carthage](https://github.com/Carthage/Carthage). To install
it, simply add the following line to your Cartfile:

`github "kishikawakatsumi/BitcoinKit"`

Contribute
----------

Feel free to open issues, drop us pull requests or contact us to discuss how to do things.

Email: [kishikawakatsumi@mac.com](mailto:kishikawakatsumi@mac.com)

Twitter: [@oleganza](http://twitter.com/k_katsumi)


License
-------

BitcoinKit is available under the Apache 2.0 license. See the LICENSE file for more info.
