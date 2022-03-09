CONTRACT:
``` javascript
access(all) contract SomeContract {
    pub var testStruct: SomeStruct

    pub struct SomeStruct {

        //
        // 4 Variables
        //

        pub(set) var a: String  // WRITE: all  READ: all

        pub var b: String   // WRITE: current & inner READ: all

        access(contract) var c: String // WRITE: current & inner READ: containing contract

        access(self) var d: String // WRITE: current & inner   READ: current & inner

        //
        // 3 Functions
        //

        pub fun publicFunc() {}
        // called in all
        access(contract) fun contractFunc() {}
        // called in contract
        access(self) fun privateFunc() {}
        //called in this struct 

        pub fun structFunc() {
        /**************/
        /*** AREA 1 ***/
        /**************/

        //WRITE: a, b, c, d
        //
        //READ: a, b, c, d
        }

        init() {
            self.a = "a"
            self.b = "b"
            self.c = "c"
            self.d = "d"
        }
    }

    pub resource SomeResource {
        pub var e: Int

        pub fun resourceFunc() {
        /**************/
        /*** AREA 2 ***/
        /**************/

        //WRITE: a
        //
        //READ: a, b, c
        }

        init() {
            self.e = 17
        }
    }

    pub fun createSomeResource(): @SomeResource {
        return <- create SomeResource()
    }

    pub fun questsAreFun() {
    /**************/
    /*** AREA 3 ***/
    /**************/
    
    //WRITE: a
    //
    //READ: a, b, c
    }

    init() {
        self.testStruct = SomeStruct()
    }
} 
```

SCRIPT:
```javascript
import SomeContract from 0x01

pub fun main() {
/**************/
/*** AREA 4 ***/
/**************/

//WRITE: I almost put a, but you can't write with scripts!
//
//READ: a, b
}
```
