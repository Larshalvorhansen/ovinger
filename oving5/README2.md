This README2 file only contains what remains to be done/reviewed before "godkjenning" tomorrow.

[x] Semaphores
[ ] Condition Variables
[ ] Protected Objects
[ ] Message passing (2 ways)

The part where you do the thing
-------------------------------

### Part 4: Message Passing
The problem is that there is no way to prioritize the cases, as Go will [choose a random case](https://golang.org/ref/spec#Select_statements) if multiple are available. Since we need a priority select mechanism, we will have to hack it with the parts that are available, specifically `default`: Try sending to a high-priority user, then default back to waiting for either one.

- You will find starter code in [the `messagepassing` folder](/messagepassing). You should complete both variants.
- You will need [a Go compiler](https://golang.org/dl/). Run the code with `go run request.go` and `go run priorityselect.go`.

*Note: "Resource Manager" is a highly mediocre name (suggestions for alternatives are welcome). Be sure to not confuse this with the "resource manager" from transactions. Naming things is one of the top two hardest things in programming, along with cache invalidation and off-by-one errors.*

### Part 5: Reflecting
See reflections.md for answers.
