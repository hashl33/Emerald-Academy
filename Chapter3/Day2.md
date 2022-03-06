 Quest Chapter 3 Day 2 : Write your own smart contract that contains two state variables: an array of resources, and a dictionary of resources. Add functions to remove and add to each of them. They must be different from the examples above.
  
    pub contract myNft {

      pub var arrayOfNfts: @[Nft]
      pub var dictionaryOfNfts: @{UInt64: Nft} 

      pub resource Nft {
          pub let id: UInt64
          pub let nftDetail: String

          init(){
              self.id = self.uuid
              self.nftDetail = "Super COol Trait"
          }
      }

      pub fun addNftToArray(nft: @Nft){
          self.arrayOfNfts.append(<- nft)
      }

      pub fun removeNftFromArray(index: Int): @Nft {
         return <- self.arrayOfNfts.remove(at: index)
      }

      pub fun addNftToDictionary(nft: @Nft){
          let key = nft.id
          self.dictionaryOfNfts[key] <-! nft
      }

      pub fun removeNftFromDictionary(key: UInt64): @Nft{
          let id <- self.dictionaryOfNfts.remove(key: key) ?? panic("Yikes! ID not found")
          return <- id
      }

      init() {
          self.arrayOfNfts <- []
          self.dictionaryOfNfts <- {}
      }

    }
  
Flow Playground:   

![image](https://user-images.githubusercontent.com/100004665/156946902-cacfe435-85b3-47cd-b7e6-e12a9204de3f.png)
