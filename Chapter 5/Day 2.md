## Explain why standards can be beneficial to the Flow ecosystem.
 Standards are neccessary to add compatibility to contracts. In order to grow the Flow eco-system, those building future tools need to expect certain events, resources etc., and if there is no standard in place, there will be a lot more work for future builders, which may 
 mean the work just doesn't get done. For example, if find.xyz had to examine each nft contract and there were no standards in place, I doubt the fine devs at .find 
 would be able to add new projects so quickly. This also goes for tooling such as the Emerald Bot, Schwwap.IO, marketplaces, wallets etc.
 
 TLDR: Standards ensure compatibility through interfaces which allows the eco-system continue to collaborate and grow.
 
 Personal Thoughts: I wonder where the standards begin to feel restrictive to those building tooling. How often are the standards updated? Will standards continue to be enforced in the future for contracts to deploy on the Flow blockchain? 


## What is YOUR favourite food?
  Shabu-Shabu しゃぶしゃぶ

## Please fix this code (Hint: There are two things wrong):
Contract Interface:
```javascript
pub contract interface ITest {
  pub var number: Int
  
  pub fun updateNumber(newNumber: Int) {
    pre {
      newNumber >= 0: "We don't like negative numbers for some reason. We're mean."
    }
    post {
      self.number == newNumber: "Didn't update the number to be the new number."
    }
  }

  pub resource interface IStuff {
    pub var favouriteActivity: String
  }
  // 1) This resource should implement IStuff (Or what is the point of the resource interface?)
  pub resource Stuff: IStuff {
    pub var favouriteActivity: String
  }
}
```
Contract:
```javascript
// 2) contract should implement ITest
pub contract Test: ITest {
  pub var number: Int
  
  pub fun updateNumber(newNumber: Int) {
    self.number = 5
  }
  
 // 3) Resource interface exists in contract interface and is redundant here
 //  pub resource interface IStuff {
 //   pub var favouriteActivity: String
 //  } 
  
// 4) Must implement ITest.IStuff specifically
  pub resources Stuff: ITest.IStuff {
    pub var favouriteActivity: String

    init() {
      self.favouriteActivity = "Playing League of Legends."
    }
  }

  init() {
    self.number = 0
  }
}
```
