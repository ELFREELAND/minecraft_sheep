# minecraft_sheep
  You need a bunch of wool for your minecraft building project. You are setting up a pasture where you can raise sheep and collect their wool. Should you pack the pasture as densely as possible to fit in the most sheep? Well, probably not. Minecraft sheep, once sheared, will only regrow their wool when they eat grass. If the pasture is full, the sheep will eat all the grass and they won't be able to grow any more wool. But if you don't put enough sheep in the pasture, you're missing out on a bunch of wool you could be gathering. So clearly there is some happy medium amount of sheep to put in your pasture to get the most efficiency. The purpose of this project is to determine, using simulations and a bit of experimental work, the most efficient density with which to pack your pasture.

## Key assumptions
  There are a couple key assumptions being made here:
- Grass growth is a simple process in which all dirt blocks have an equal chance to turn into a grass block on a tick. This is not entirely realistic, since grass growth is contingent on spreading from an adjacent block, but as long as dirt blocks consistently have adjacent grass blocks it should work.
- The goal is to have the greatest rate of sheep regenerating their wool. I am ignoring the fact that sheep only regenerate wool if they don't already have any. So really the goal is to have the greatest rate of sheep eating grass.

## Math
We can find a mathematical expression for the rate at which sheep eat grass.

On any given tick, sheep have a chance to attempt to eat grass. We can assume that the rate of a single sheep attempting to eat is constant. We'll call this R \[s<sup>-1</sup>].

The rate at which a flock of sheep attempts to eat is R times the number of sheep. We'll call the number of sheep N \[unitless]. So the rate of the flock attempting to eat is RN.

The rate at which the flock *succeeds* in eating is dependent on what fraction of the flock is standing on a grass block. If the sheep are uniformly distributed, the fraction of the flock standing on grass is equal to the fraction of blocks that are grass. Let's call the total number of blocks B, and the number of grass blocks G \[both unitless].

The rate at which grass grows is a little complicated. If there is barely any grass, then grass growth will be limited by the lack of places the grass can spread *from*. If there is barely any dirt, grass growth will be limited by a lack of places for the grass to spread *to*. This can be modelled by the logistic function. This is a function with a special derivative: `f'(x) = k * f(x) * (1-f(x))`. If `f(x)` is small, `f'(x)` will also be small, because there is a lack of grass to spread. If `f(x)` is close to 1, then `f'(x)` will be small because there is a lack of places for the grass to spread to. In our case, `f(x)` is the fraction of the blocks which are grassy, i.e. G/B. k is just a scaling factor which represents the rate at which grass attempts to spread.

Putting this all together, `(G/B)'(t) = k * (G/B) * (1 - G/B)`.
