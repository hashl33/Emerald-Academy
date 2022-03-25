## What does "force casting" with as! do? Why is it useful in our Collection?
 Force casting allows us to downcast from the generic implemented interface to, in our case, the more specific contract level resource. In our collection this is useful   o restrict the type of NFT
 that can be deposited to the NFT type we define in our contract, not just any NFT that meets the Standard NFT contract.
 
## What does auth do? When do we use it?
  Auth is used to downcast from a generic to a specific type. Auth is required to downcast references.
  
## This last quest will be your most difficult yet. Take this contract:

Contract:
```javascript
import NonFungibleToken from 0x02

pub contract BabyJacob: NonFungibleToken {
  pub var totalSupply: UInt64

  pub event ContractInitialized()
  pub event Withdraw(id: UInt64, from: Address?)
  pub event Deposit(id: UInt64, to: Address?)
  pub event Minted(id: UInt64, name: String, favouriteFood: String, luckyNumber: Int)

  pub resource NFT: NonFungibleToken.INFT {
    pub let id: UInt64

    pub let name: String
    pub let favouriteFood: String
    pub let luckyNumber: Int

    init(_name: String, _favouriteFood: String, _luckyNumber: Int) {
      self.id = self.uuid

      self.name = _name
      self.favouriteFood = _favouriteFood
      self.luckyNumber = _luckyNumber

      emit Minted(id: self.id, name: self.name, favouriteFood: self.favouriteFood, luckyNumber: self.luckyNumber)
    }
  }

    pub resource interface CollectionPublic {
        pub fun deposit(token: @NonFungibleToken.NFT)
        pub fun getIDs(): [UInt64]
        pub fun borrowNFT(id: UInt64): &NonFungibleToken.NFT
        pub fun borrowEntireNFT(id: UInt64): &NFT
    }

  pub resource Collection: NonFungibleToken.Provider, NonFungibleToken.Receiver, NonFungibleToken.CollectionPublic, CollectionPublic {
    pub var ownedNFTs: @{UInt64: NonFungibleToken.NFT}

    pub fun withdraw(withdrawID: UInt64): @NonFungibleToken.NFT {
      let nft <- self.ownedNFTs.remove(key: withdrawID) 
            ?? panic("This NFT does not exist in this Collection.")
      emit Withdraw(id: nft.id, from: self.owner?.address)
      return <- nft
    }

    pub fun deposit(token: @NonFungibleToken.NFT) {
      let nft <- token as! @NFT
      emit Deposit(id: nft.id, to: self.owner?.address)
      self.ownedNFTs[nft.id] <-! nft
    }

    pub fun getIDs(): [UInt64] {
      return self.ownedNFTs.keys
    }

    pub fun borrowNFT(id: UInt64): &NonFungibleToken.NFT {
      return &self.ownedNFTs[id] as &NonFungibleToken.NFT
    }

    pub fun borrowEntireNFT(id: UInt64): &NFT {
      let ref= &self.ownedNFTs[id] as auth &NonFungibleToken.NFT 
      return ref as! &NFT
    }

    init() {
      self.ownedNFTs <- {}
    }

    destroy() {
      destroy self.ownedNFTs
    }
  }

  pub fun createEmptyCollection(): @NonFungibleToken.Collection {
    return <- create Collection()
  }

  pub resource Minter {

    pub fun createNFT(name: String, favouriteFood: String, luckyNumber: Int): @NFT {
      return <- create NFT(_name: name, _favouriteFood: favouriteFood, _luckyNumber: luckyNumber)
    }

    pub fun createMinter(): @Minter {
      return <- create Minter()
    }

  }

  init() {
    self.totalSupply = 0
    emit ContractInitialized()
    self.account.save(<- create Minter(), to: /storage/Minter)
  }
}

```
Setup Transaction:
```javascript
import BabyJacob from 0x01
import NonFungibleToken from 0x02

transaction {
  prepare(signer: AuthAccount) {
  let collection <- BabyJacob.createEmptyCollection()
  signer.save(<- collection, to: /storage/Collection)
  signer.link<&BabyJacob.Collection{NonFungibleToken.CollectionPublic, BabyJacob.CollectionPublic}>(/public/Collection, target: /storage/Collection)
  }

  execute {
    log("Stored a collection for our NFT")
  }
}
```
Mint Nft Transaction:
```javascript
import NonFungibleToken from 0x02
import BabyJacob from 0x01

transaction(address: Address, name: String, favouriteFood: String, luckyNumber: Int){
    prepare(signer: AuthAccount) {
    //get a ref to the minter
    let minter = signer.borrow<&BabyJacob.Minter>(from: /storage/Minter)
        ?? panic("Imposter")

    //Get ref to public collection
     let publicCollection = getAccount(address).getCapability(/public/Collection)
                            .borrow<&BabyJacob.Collection{BabyJacob.CollectionPublic}>()
                            ?? panic("This account does not have a Collection")

    let nft <- minter.createNFT(name: name, favouriteFood: favouriteFood, luckyNumber: luckyNumber)

    publicCollection.deposit(token: <- nft)

    }
}
```
Script Get Ids :
```javascript
import BabyJacob from 0x01

pub fun main(address: Address): [UInt64] {
  let publicCollection = getAccount(address).getCapability<&BabyJacob.Collection{BabyJacob.CollectionPublic}>(/public/Collection).borrow()
                                ?? panic ("No Collection is borrowed.")
  
  return publicCollection.getIDs()
}
```
Script Read MetaData:
```javascript
import BabyJacob from 0x01

pub fun main(address: Address, id: UInt64) {
  let ViewCollection = getAccount(address).getCapability<&BabyJacob.Collection{BabyJacob.CollectionPublic}>(/public/Collection).borrow()
                                ?? panic ("No Collection Ref is borrowed.")
  
  let nftRef = ViewCollection.borrowEntireNFT(id: id)
  
  log(nftRef.name)
  log(nftRef.favouriteFood)
  log(nftRef.luckyNumber)
}
```

Screenshots of last script:
![image](https://user-images.githubusercontent.com/100004665/160171502-1a7f1db2-a634-4b02-8a24-1630b90652fd.png)


