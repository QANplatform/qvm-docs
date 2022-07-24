# Writing a contract

This example will show how to write a smart contract in Golang.

## Sample contract functionality

The sample contract serves as a demonstration of general QVM logic.

It is capable of:

- registering users into the database
- storing the last registered user's name in the database
- echoing the previous user's name to STDOUT
- incrementing the total registered user count
- storing above counter in the database

## Sample code

```go
package main

import (
	"fmt"
	"os"
	"strconv"
)

func main() {

	// THIS SAMPLE ONLY SUPPORTS THE "register" FUNCTION
	if len(os.Args) == 3 && os.Args[1] == "register" {

		// GET THE CURRENT USER'S NAME
		previousName := os.Getenv("DB_USER_CURRENT")

		// OR DEFAULT TO "unknown" IF THIS IS THE FIRST CALL
		if len(previousName) == 0 {
			previousName = "unknown"
		}

		// GET THE TOTAL USER COUNT
		totalUserCount, _ := strconv.Atoi(os.Getenv("DB_TOTALUSERS"))

		// WRITE PREVIOUS USER NAME TO STDOUT
		os.Stdout.WriteString(fmt.Sprintf("OUT=prevname: %s\n", previousName))

		// UPDATE CURRENT USER NAME BY WRITING IT TO DB
		os.Stdout.WriteString(fmt.Sprintf("DBW=USER_CURRENT=%s\n", os.Args[2]))

		// STORE USER NAME UNDER A STORAGE SLOT FOR PERSISTENCE (CURRENT GETS OVERWRITTEN ON EACH CALL)
		os.Stdout.WriteString(fmt.Sprintf("DBW=USER_%d=%s\n", totalUserCount, os.Args[2]))

		// INCREMENT THE TOTAL USER COUNT
		os.Stdout.WriteString(fmt.Sprintf("DBW=TOTALUSERS=%d\n", totalUserCount+1))
		os.Exit(0)
	}
    if len(os.Args) >= 2 {
        os.Stderr.WriteString(fmt.Sprintf("Wrong CMD:%s\n", os.Args[1]))
        os.Exit(1)
    }
    os.Stderr.WriteString("Wrong args!\n")
    os.Exit(1)
}
```
