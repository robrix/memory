- [ ] Enumeration
- [ ] What folds are
- [ ] Why folds are universal
- [ ] Following Oleg Kiselyov, generalizing to early termination
- [ ] Following Oleg Kiselyov, generalizing to nonrecursion
- [ ] Iteration
- [ ] Following Rich Hickey, reducers
- [ ] Following Rich Hickey, transducers?
- [ ] Following Rich Hickey, parallel folds via associativity
- [ ] The relationship to Erik Meijer’s reactive (signal) types (Observable, etc)
- [ ] Generalizing to signals

## Folds

Enumeration can be implemented most generally in terms of _folds_. A fold (or _reduce_) is a function or method which enumerates a collection and combines each element with the value computed by the previous iteration (starting from some seed). For example, summing the numbers in a collection is a fold:

1. Start with a sum of 0.
2. For each element of the collection, call + with the current sum and that element.

Written imperatively, that has this structure:

```swift
var sum = 0 // the running tally, or “accumulator,” starts with this value
for each in numbers { // the collection being iterated over
	sum = sum + each // combine each element with the accumulator
}
return sum // the final result
```

There are actually two ways of combining; the above is a _left fold_ (because the repeated combinations associate to the left), but _right folds_ also exist. We can generalize this to a `fold` method on a hypothetical `List`:

```swift
func fold<Accumulator>(var seed: Accumulator, combine: (Accumulator, Element) -> Accumulator) -> Accumulator {
	for each in self {
		seed = combine(seed, each)
	}
	return seed
}
```

We can also write an equivalent pure implementation using recursion:

```swift
func fold<Accumulator>(seed: Accumulator, combine: (Accumulator, Element) -> Accumulator) -> Accumulator {
	if let first = self.first {
		return self.rest.foldLeft(combine(seed, first), combine)
	}
	return seed
}
```
