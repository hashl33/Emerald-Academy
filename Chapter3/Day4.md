1. Interfaces are used to (1) set up a structure that a resource/struct adheres to (interface must be fully implemented, but you can add variables to a resource that are not required in interface) and also (2) you can force a resource to match the interface completely, as shown in the example below, to restrict access to certain people. 

2.
![image](https://user-images.githubusercontent.com/100004665/157470805-00811140-cb6b-408e-b194-73d38beefd9f.png)


3.  Fix ONE: add favoriteFruit to struct \
    Fix TWO: initialize added variable \
    Fix THREE: add fuction to interface \

  pub contract Stuff {

    pub struct interface ITest {
      pub var greeting: String
      pub var favouriteFruit: String
      //FIX THREE: add function to interface
      pub fun changeGreeting(newGreeting: String): String
    }

    pub struct Test: ITest {
      pub var greeting: String
      // FIX ONE: add favouriteFruit to comply with interface
      pub var favouriteFruit: String 
      pub fun changeGreeting(newGreeting: String): String {
        self.greeting = newGreeting
        return self.greeting // returns the new greeting
      }

      init() {
        self.greeting = "Hello!"
        //FIX TWO: initialize new var
        self.favouriteFruit = "Tomato"
      }
    }

    pub fun fixThis() {
      let test: Test{ITest} = Test()
      let newGreeting = test.changeGreeting(newGreeting: "Bonjour!") // Must add to interface to access
      log(newGreeting)
    }
  }
