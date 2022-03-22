### Describe what an event is, and why it might be useful to a client.
  When an event occurs in a smart contract a message is emitted to the outside world/client. Events are defined within the contract and invoked/broadcasted with an emit. 
  
  Events could be useful in an nft marketplace to update recent sales, listings, price changes etc. 
  
  Personal thoughts: It seems the Cadence NFT Standard requires events for contract
  initialization and deposit/withdraw functions. I'm curious which events are typically included in addition to the initial 3. Is "listed for sale" an event in a marketplace contract? Are there any times you should not emit an event? 
  
### Deploy a contract with an event in it, and emit the event somewhere else in the contract indicating that it happened

Contract with a event to track when an nft is named:
```javascript
pub contract MyNft {
    // Define event
    pub event nftNamed(name: String)

    pub resource NFT{

        pub let id: UInt64
        access(contract) var name: String
        access(contract) let metadata: String
        
        init(_name:String){
        self.id = self.uuid
        self.name = ""
        self.metadata = ""
        }

        pub fun setName(name: String): String{
            self.name = name
            //Invoke event and broadcast to the outside world
            emit nftNamed(name: name)
            return self.name
        }
    }
}
```

### Using the contract in step 2), add some pre conditions and post conditions to your contract to get used to writing them out.

```javascript
pub contract MyNft {
    // Define event
    pub event nftNamed(name: String)

    pub resource NFT{

        pub let id: UInt64
        access(contract) var name: String
        access(contract) let metadata: String
        
        init(_name:String){
        self.id = self.uuid
        self.name = ""
        self.metadata = ""
        }

        pub fun setName(name: String): String{
            // pre condition checks string length
            pre {
                name.length > 3: "Name must be longer than 3 characters"
            }
            // post condition verifies new name does not equal previous name
            post {
                result != before(self.name): "This is already the name!"
            }
            self.name = name
            //Invoke event and broadcast to the outside world
            emit nftNamed(name: name)
            return self.name
        }
    }
}
```

### For each of the functions below (numberOne, numberTwo, numberThree), follow the instructions.
