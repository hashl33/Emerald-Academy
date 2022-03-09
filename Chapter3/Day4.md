save progress

pub contract myNfts {

 pub resource interface IMyNft {
    pub let color: String
 }

 pub resource MyNft: IMyNft{
    pub let color: String
    init() {
    self.color = "Green"
    }
 }

 pub fun noInterface(){
    let test: @MyNft <- create MyNft()
   log(test.color) // green
    destroy test
 }

 pub fun withInterface(){
   let test2: @MyNft{IMyNft} <- create MyNft()
   log(test2.color) // error
   destroy test2
 }
}
