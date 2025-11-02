
# Solution Stage 1

Given

```
Device settings: SM=40, Threads=1024, WarpsPerSM=32,
sharedMemPerBlock=48(kb), sharedMemPerMultiprocessor=64(kb),
deviceTotalGlobalMem=14(gb), FP32Flops=4070400
```

Let's define one unit of work.

One unit of work is colouring a square of 32 x 32 pixels. Let's assume that there are maxN circles that touch that square.
1024 x 1024 contains 1024 such squares.

For each square we can create a vector of triangles that touch it.

This is our first step.

Second step is to colour everything. For simplicity let's now colour just blocks of 32x32 as one thread block. (This could be not optimal).

So in next step each pixel goes throught the list of circles and applies painting.

This will be a simple first implementation.

The only implementation question - is handling lists of undefined lengths for circles. For now let's just allocate enough memory initially.

# Solution Stage 2

It appears that the formula of calculation of colour can be represented in this way:

``
pixel_colour = circle_1 / 2 + circle_2 / 4 + circle_3 / 8 + ..._
```

Where circle_1 is the closest circle colour, circle_2 is the second closest circle colour, etc.

So taking 0..255 grade of colour component - in reality only first 9 pixel-covering circles matter where remaining circles can add less than 0.5/256 of colour so taking into account some error we can ignore them.

Let's take 10 circles just in case... (and not forget background colour)

Let's make a simple change to reverse shading where we stop at 10 circles max.
