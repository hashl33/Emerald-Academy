# 1. What does .link() do?
  .link() creates a, well, link to the /storage/ path so that the linked data is exposed to the public (or private)
  
  Using .link() looks like this: signer.link<&MyContract.MyResource>(/public/MyTestResource, target: /storage/MyTestResource)

# 2. In your own words (no code), explain how we can use resource interfaces to only expose certain things to the /public/ path.
  By implementing a resource interface, only the variables and functions included in the interface will be exposed to the public through capabilities. Anything not included in the implemented interface will not be accessible through the contract (but **WILL BE VISIBLE ON BLOCKCHAIN**)

# 3. Deploy a contract that contains a resource that implements a resource interface. 

```javascript
pub contract MyNft {

    pub resource interface INft{
        pub var name: String
    }

    pub resource Nft: INft    {
        pub var name: String
        pub var trait: String
        init(){
            self.name = "Ash"
            self.trait = "Gold"
        }    
    }

    pub fun createNft(): @Nft {
      return <- create Nft()  
    }
}
```

   Then, do the following: In a transaction, save the resource to storage and link it to the public with the restrictive interface.
   
```javascript
import MyNft from 0x01

transaction() {
  prepare(signer: AuthAccount){
    let testNftResource <- MyNft.createNft()
    signer.save(<- testNftResource, to: /storage/MyTestNftResource)
    signer.link<&MyNft.Nft{MyNft.INft}>(/public/MyTestNftResource, target: /storage/MyTestNftResource)
  }

  execute{
  
  }
}
```
 
   Run a script that tries to access a non-exposed field in the resource interface, and see the error pop up.
    ![image](https://user-images.githubusercontent.com/100004665/158022877-1b99445c-8d6d-4143-ba0d-bd1a73275908.png)
    
```javascript
import MyNft from 0x01
pub fun main(address: Address): String {
  let publicCapability: Capability<&MyNft.Nft{MyNft.INft}> =
    getAccount(address).getCapability<&MyNft.Nft{MyNft.INft}>(/public/MyTestNftResource)

    let myTestNftResource: &MyNft.Nft{MyNft.INft} = publicCapability.borrow() ?? panic("The capability doesn't exist")
  return myTestNftResource.trait
  }
```

   Run the script and access something you CAN read from. Return it from the script.
    ![image](https://user-images.githubusercontent.com/100004665/158022899-f019edd9-45a6-4f27-a723-98f78ce8dc36.png)
 
 ```javascript
 import MyNft from 0x01
pub fun main(address: Address): String {
  let publicCapability: Capability<&MyNft.Nft{MyNft.INft}> =
    getAccount(address).getCapability<&MyNft.Nft{MyNft.INft}>(/public/MyTestNftResource)

    let myTestNftResource: &MyNft.Nft{MyNft.INft} = publicCapability.borrow() ?? panic("The capability doesn't exist")
  return myTestNftResource.name
  }
```

