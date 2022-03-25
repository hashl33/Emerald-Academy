What does "force casting" with as! do? Why is it useful in our Collection?
 Force casting allows us to downcast from the generic implemented interface to, in our case, the more specific contract level resource. In our collection this is useful to restrict the type of NFT
 that can be deposited to the NFT type we define in our contract, not just any NFT that meets the Standard NFT contract.
 
What does auth do? When do we use it?
  Auth is used to downcast from a generic to a specific type. Auth is required to downcast references.
  
This last quest will be your most difficult yet. Take this contract:
