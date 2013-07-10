# Vivid Cortex :: EWMA

This repo provides Exponentially Weighted Moving Average algorithms, or EWMAs for short.

![Build Status](https://circleci.com/gh/VividCortex/moving_average.png?circle-token=1459fa37f9ca0e50cef05d1963146d96d47ea523)

A picture of a cat:

![Moving Average Cat](http://f.cl.ly/items/1z3T2C2S2c1K2Z2Q3j05/Image%202013.07.05%2018%3A36%3A23.jpeg)

### Exponentially Weighted Moving Average

An exponentially weighted moving average is a way to continuously compute a type of
average for a series of numbers, as the numbers arrive. After a value in the series is
added to the average, its weight in the average decreases exponentially over time. This
biases the average towards more recent data. EWMAs are useful for several reasons, chiefly
their inexpensive computational and memory cost, as well as the fact that they represent
the recent central tendency of the series of values.

The EWMA algorithm requires a decay factor, alpha. The larger the alpha, the more the average
is biased towards recent history. The alpha must be between 0 and 1, and is typically
a fairly small number, such as 0.04. We will discuss the choice of alpha later.

The algorithm works thus, in pseudocode:

1. Multiply the next number in the series by alpha.
2. Multiply the current value of the average by 1 minus alpha.
3. Add the result of steps 1 and 2, and store it as the new current value of the average.
4. Repeat for each number in the series.

There are special-case behaviors for how to initialize the current value, and these vary
between implementations. One approach is to start with the first value in the series;
another is to average the first 10 or so values in the series using an arithmetic average,
and then begin the incremental updating of the average. Each method has pros and cons.

It may help to look at it pictorially. Suppose the series has five numbers, and we choose
alpha to be 0.50 for simplicity. Here's the series, with numbers in the neighborhood of 300.

![Data Series](http://f.cl.ly/items/2W0I230b3b1B3p3o181O/data%20series.png)

Now let's take the moving average of those numbers. First we set the average to the value
of the first number.

![EWMA Step 1](http://f.cl.ly/items/003E0i1T1H2t373n3L3g/ewma-1.png)

Next we multiply the next number by alpha, multiply the current value by 1-alpha, and add
them to generate a new value.

![EWMA Step 2](http://f.cl.ly/items/2W2Z0b3J18122y1F3F2u/ewma-2.png)

This continues until we are done.

![EWMA Step N](http://f.cl.ly/items/0R3Y2V2o1t2Q1B082L3c/ewma.png)

Notice how each of the values in the series decays by half each time a new value
is added, and the top of the bars in the lower portion of the image represents the
size of the moving average. It is a smoothed, or low-pass, average of the original
series.

For another explanation of the algorithm see [Exponentially weighted moving average](http://en.wikipedia.org/wiki/Moving_average#Exponential_moving_average) on wikipedia.

#### SimpleEWMA

A SimpleEWMA represents the exponentially weighted moving average of a
series of numbers. It **will** have different behavior than the VariableEWMA
for multiple reasons. It has no warm-up period and it uses a constant
decay.  These properties let it use less memory.  It will also behave
differently when it's equal to zero, which is assumed to mean
uninitialized, so if a value is likely to actually become zero over time,
then any non-zero value will cause a sharp jump instead of a small change.


#### VariableEWMA

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
