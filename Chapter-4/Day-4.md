```javascript
pub contract BabyJacob {
  pub var totalSupply: UInt64

  // This is an NFT resource that contains a name,
  // favouriteFood, and luckyNumber
  pub resource NFT {
    pub let id: UInt64

    pub let name: String
    pub let favouriteFood: String
    pub let luckyNumber: Int

    init(_name: String, _favouriteFood: String, _luckyNumber: Int) {
      self.id = self.uuid

      self.name = _name
      self.favouriteFood = _favouriteFood
      self.luckyNumber = _luckyNumber
    }
  }

  // This is a resource interface that allows us to disable/hide the withdraw function from public access
  // When implemented, this interface forces the resource to include the deposit, getIDs, and borrowNFT functions
  pub resource interface CollectionPublic {
    pub fun deposit(token: @NFT)
    pub fun getIDs(): [UInt64]
    pub fun borrowNFT(id: UInt64): &NFT
  }

    // This is a resource that implements the CollectionPublic Interface and all of its functions
    // as well as the ownedNFTs variable and the withdraw function
  pub resource Collection: CollectionPublic {
    pub var ownedNFTs: @{UInt64: NFT}

    //This function allows nfts to be deposited into a Collection (by anyone)
    pub fun deposit(token: @NFT) {
      self.ownedNFTs[token.id] <-! token
    }

    //This function allows existing nfts to be withdrawn (by the AuthAccount)
    // If the nft is not in the account, the panic keyword returns an error message
    pub fun withdraw(withdrawID: UInt64): @NFT {
      let nft <- self.ownedNFTs.remove(key: withdrawID) 
              ?? panic("This NFT does not exist in this Collection.")
      return <- nft
    }

    // This function allows a transaction/script to get the IDs of the NFTs the account owns
    pub fun getIDs(): [UInt64] {
      return self.ownedNFTs.keys
    }

    // The borrow function returns a reference to the ownedNfts and lets us read the nft metadata
    pub fun borrowNFT(id: UInt64): &NFT {
      return &self.ownedNFTs[id] as &NFT
    }

    init() {
      self.ownedNFTs <- {}
    }

    destroy() {
      destroy self.ownedNFTs
    }
  }
    // This function creates the Collection that will hold all the nfts in this account
  pub fun createEmptyCollection(): @Collection {
    return <- create Collection()
  }

    // This resource contains the createMinter function
    // Also includes the createNft function, which ensures the safety of the contract (by placing this function within the minter resource, minting is restricted to the minter)
  pub resource Minter {
      // This function does the cool job of creating/minting the nft along with its metadata: fav food, lucky number and name
    pub fun createNFT(name: String, favouriteFood: String, luckyNumber: Int): @NFT {
      return <- create NFT(_name: name, _favouriteFood: favouriteFood, _luckyNumber: luckyNumber)
    }
    // This function creates the minter resource restricts nft minting to only the contract creator (or assigned minter)
    pub fun createMinter(): @Minter {
      return <- create Minter()
    }

  }

  init() {
    self.totalSupply = 0
    self.account.save(<- create Minter(), to: /storage/Minter)
  }
}
```
