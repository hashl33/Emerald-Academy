1. Interfaces are used to (1) set up a structure that a resource/struct adheres to (interface must be fully implemented, but you can add variables to a resource that are not required in interface) and also (2) you can force a resource to match the interface completely, as shown in the example below, to restrict access to certain people. 

![image](https://user-images.githubusercontent.com/100004665/157470805-00811140-cb6b-408e-b194-73d38beefd9f.png)


save progress

pub contract myNfts {

 pub resource interface IMyNft {
    pub let color: String
 }

 pub resource MyNft: IMyNft{
    pub let color: String
    pub let animal: String
    init() {
    self.color = "Green"
    self.animal = "Dog"
    }
 }

 pub fun noInterface(){
    let test: @MyNft <- create MyNft()
   log(test.animal) // dog
    destroy test
 }

 pub fun withInterface(){
   let test2: @MyNft{IMyNft} <- create MyNft()
   log(test2.animal) // error
   destroy test2
 }
}


3. 
