# minecraft_sheep
You need a bunch of wool for your minecraft building project. You are setting up a pasture where you can raise sheep and collect their wool. Should you pack the pasture as densely as possible to fit in the most sheep? Well, probably not. Minecraft sheep, once sheared, will only regrow their wool when they eat grass. If the pasture is full, the sheep will eat all the grass and they won't be able to grow any more wool. But if you don't put enough sheep in the pasture, you're missing out on a bunch of wool you could be gathering. So clearly there is some happy medium amount of sheep to put in your pasture to get the most efficiency. The purpose of this project is to determine, using simulations and a bit of experimental work, the most efficient density with which to pack your pasture.

## Key assumptions
There are a couple key assumptions being made here:
- Grass growth is a simple process in which all dirt blocks have an equal chance to turn into a grass block on a tick. This is not entirely realistic, since grass growth is contingent on spreading from an adjacent block, but as long as dirt blocks consistently have adjacent grass blocks it should work.
- The goal is to have the greatest rate of sheep regenerating their wool. I am ignoring the fact that sheep only regenerate wool if they don't already have any. So really the goal is to have the greatest rate of sheep eating grass.

## Math
### The sheep
We can find a mathematical expression for the rate at which sheep eat grass.

On any given tick, sheep have a chance to attempt to eat 1 square meter of grass. We can assume that the rate of a single sheep attempting to eat is constant. We'll call this R \[m<sup>2</sup>s<sup>-1</sup>].

The rate at which a flock of sheep attempts to eat is R times the number of sheep. We'll call the number of sheep N \[unitless]. So the rate of the flock attempting to eat is NR \[m<sup>2</sup>s<sup>-1</sup>].

The rate at which the flock *succeeds* in eating is dependent on what fraction of the flock is standing on grass. If the sheep are uniformly distributed, the fraction of the flock standing on grass is equal to the fraction of the pasture that is grassy. Let's call the fraction of grass cover G \[unitless]. So the actual rate at which the flock eats is NRG. 

### The grass
The rate at which grass grows is a little complicated. If there is barely any grass, then grass growth will be limited by the lack of places the grass can spread *from*. If there is barely any dirt, grass growth will be limited by a lack of places for the grass to spread *to*. This can be modelled by the logistic function. This is a function with a special derivative:

$f'(x)=kf(x)(1-f(x))$

If f(x) is small, f'(x) will also be small, because there is a lack of grass to spread. If f(x) is close to 1, then f'(x) will be small because there is a lack of places for the grass to spread to. In our case, f(x) is the fraction of the pasture that is grassy, i.e. G. k \[s<sup>-1</sup>] is just a scaling factor which represents the rate at which grass attempts to spread.

Putting this all together

$G'=kG(1-G)$

If we multiply by A, we can express the rate of change of the total grass area: 

$AG'= kAG(1-G)$

### Putting it together
But wait - G' is also affected by the flock eating the grass. After the flock has been munching for a while, the rate at which the flock is eating should be equal to NRG. This is equal to AG', the rate at which grass is regrowing. Setting our two expressions equal to each other, we can say that at steady state,

$NRG=kAG(1-G)$

This expression is important because it gives us the equilibrium grass cover as a function of sheep population - a little algebra gives 

$G=1-\frac{NR}{Ak}$

We'll introduce the sheep population density, D \[m<sup>-2</sup>] as the number of sheep divided by the pasture area. This gives

$G = 1 - \frac{DR}{k}$

Plugging this into our logistic model for grass growth gives

$G' = DR - \frac{D^2R^2}{k}$

This is what we've been looking for - the steady-state grass growth rate as a function of sheep population density. This is also equal to the rate at which the sheep are eating the grass. We can find the maximum by differentiating with respect to D:

$\frac{dG'}{dD}=R-\frac{2DR^2}{k}=0$

$D=\frac{k}{2R}$

According to the assumptions we've made, this is the sheep population density that will produce the maximum rate of grass growth. It's important to note that if we plug this back into our expression for equilibrium grass cover, we get G = 0.5. This should make sense - the derivative of the logistic function is maximum when f(x) = 0.5.

## Theory only gets you so far
### Parameter measurements
Now what? It seems like we have the answer we wanted, but what about k and R? we need to know the actual rate at which sheep eat, and the rate at which grass grows.

The user-maintained minecraft wiki says that sheep have a 1/1000 chance of attempting to eat on every other tick (a tick being 1/20 of a second). With ten chances per second at a probability of 1/1000, the expected value is 0.001. So we can estimate that R=0.001. However, this is based on data from a user-operated wiki with no citation standards and questionable accountability. So we will be conducting an in-game experiment to determine R.

The grass mechanics are sufficiently complicated that it will also be easier for us to set up an experiment in-game to estimate the logistic growth parameters.

### Simulations
Finally, to validate all this math and get some actual insight into the sheep and their activities, we will be constructing first-principles simulations of grazing sheep.