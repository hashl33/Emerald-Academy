### Why did we add a Collection to this contract? List the two main reasons.
  - Without a collection we need to save to a new storage path for each nft we own. 
  - No one can mint us an nft without having a collection first (I need to understand this further)

### What do you have to do if you have resources "nested" inside of another resource? ("Nested resources")
  - You have to destroy the resources using destroy function

### Brainstorm some extra things we may want to add to this contract. Think about what might be problematic with this contract and how we could fix it.

Idea #1: Do we really want everyone to be able to mint an NFT? (insert thinking emoji here).

No, createNFt() should be restricted use, maybe create an admin resource with permissions set when contract is deployed?

Idea #2: If we want to read information about our NFTs inside our Collection, right now we have to take it out of the Collection to do so. Is this good?

I am not sure if this is good or not, but it can be a hassle. I think the solution is to use references to access the information, rather than moving the resources around. 
