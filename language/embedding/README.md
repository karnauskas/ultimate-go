## Embedding

Embedding types provides the final piece of sharing and reusing state and behavior between types. Through the use of inner type promotion, an inner type's fields and methods can be directly accessed by references of the outer type.

## Notes

* Embedding types allows us to share state or behavior between types.
* The inner type never loses its identity.
* This is not inheritance.
* Through promotion, inner type fields and methods can be accessed through the outer type.
* The outer type can override the inner type's behavior.

### Exercise 1

Copy the code from the template. Declare a new type called hockey which embeds the sports type. Implement the matcher interface for hockey. When implementing the match method for hockey, call into the match method for the embedded sport type to check the embedded fields first. Then create two hockey values inside the slice of matchers and perform the search.

[Template](exercises/template1/template1.go) ([Go Playground](https://play.golang.org/p/dK0FnSnnRz)) |
[Answer](exercises/exercise1/exercise1.go) ([Go Playground](https://play.golang.org/p/ZeOIYmIw-r))

Solution:
```go
package main

import (
	"fmt"
	"strings"
)

// finder defines the behavior required for performing matching.
type finder interface {
	find(needle string) bool
}

// sport represents a sports team.
type sport struct {
	team string
	city string
}

// find checks the value for the specified term.
func (s *sport) find(needle string) bool {
	return strings.Contains(s.team, needle) || strings.Contains(s.city, needle)
}

// hockey represents specific hockey information.
type hockey struct {
	sport
	country string
}

// find checks the value for the specified term.
func (h *hockey) find(needle string) bool {
	return h.sport.find(needle) || strings.Contains(h.country, needle)
}

func main() {

	// Define the term to find.
	needle := "Miami"

	// Create a slice of finder values and assign values
	// of the concrete hockey type.
	haystack := []finder{
		&hockey{sport{"Panthers", "Miami"}, "USA"},
		&hockey{sport{"Canadiens", "Montreal"}, "Canada"},
	}

	// Display what we are trying to find.
	fmt.Println("Searching For:", needle)

	// Range of each haystack value and check the term.
	for _, hs := range haystack {
		if hs.find(needle) {
			fmt.Printf("FOUND: %+v", hs)
		}
	}
}
```
