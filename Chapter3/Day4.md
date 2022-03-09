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
