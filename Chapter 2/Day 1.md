## Deploy a contract to account 0x03 called "JacobTucker". Inside that contract, declare a constant variable named is, and make it have type String. 
## Initialize it to "the best" when your contract gets deployed.
Contract:
```javascript
pub contract JacobTucker {
  pub var is: String;

  init(){
  self.is = "The Best";
  }
}
```

## Check that your variable is actually equals "the best" by executing a script to read that variable. Include a screenshot of the output.
Script:
```javascript
  
import JacobTucker from 0x03
pub fun main(): String {
  return JacobTucker.is;
}
```
Screetshot:
![image](https://user-images.githubusercontent.com/100004665/160053120-fc711bf6-1926-44bc-bb4d-36a0794bc967.png)

