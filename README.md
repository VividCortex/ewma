# Vivid Cortex :: EWMA

This is a simple implementation of the Exponentially Weighted Moving Average algorithms,
you can find more information about it on [it's wikipedia page](https://en.wikipedia.org/wiki/Moving_average).

![Build Status](https://circleci.com/gh/VividCortex/moving_average.png?circle-token=1459fa37f9ca0e50cef05d1963146d96d47ea523)

A picture of a cat:

![Moving Average Cat](http://f.cl.ly/items/1z3T2C2S2c1K2Z2Q3j05/Image%202013.07.05%2018%3A36%3A23.jpeg)


### Currently implemented flavours:


#### Exponentially Weighted Moving Average

For an explanation of the algorithm see [Exponentially weighted moving average](http://en.wikipedia.org/wiki/Moving_average#Exponential_moving_average) on wikipedia.


##### SimpleEWMA

A SimpleEWMA represents the exponentially weighted moving average of a
series of numbers. It **will** have different behavior than the VariableEWMA
for multiple reasons. It has no warm-up period and it uses a constant
decay.  These properties let it use less memory.  It will also behave
differently when it's equal to zero, which is assumed to mean
uninitialized, so if a value is likely to actually become zero over time,
then any non-zero value will cause a sharp jump instead of a small change.


##### VariableEWMA

VariableEWMA represents the exponentially weighted moving average of a series of
numbers. Unlike SimpleEWMA, it supports a custom age, and thus uses more memory.


## Usage

```go
package main
import "github.com/VividCortex/ewma"

func main() {
  samples := [100]float64{
    4599, 5711, 4746, 4621, 5037, 4218, 4925, 4281, 5207, 5203, 5594, 5149,
  }

  e := ewma.NewMovingAverage()       //=> Returns a SimgpleEWMA if called without params
  a := ewma.NewMovingAverage(5)      //=> returns a VariableEWMA with a decay of 2 / (5 + 1)

  for _, f := range samples {
    e.Add(f)
    a.Add(f)
  }

  e.Value() //=> 13.577404704631077
  a.Value() //=> 1.5806140565521463e-12
}
```

## Contribute

Contributions are welcome, check out [the Moving Average wikipedia page](https://en.wikipedia.org/wiki/Moving_average) for other flavours of the algorithm to implement.


## Licence

MIT, see LICENCE file.
