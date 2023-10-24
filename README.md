# minecraft_sheep
You need a bunch of wool for your minecraft building project. You are setting up a pasture where you can raise sheep and collect their wool. Should you pack the pasture as densely as possible to fit in the most sheep? Well, probably not. Minecraft sheep, once sheared, will only regrow their wool when they eat grass. If the pasture is full, the sheep will eat all the grass and they won't be able to grow any more wool. But if you don't put enough sheep in the pasture, you're missing out on a bunch of wool you could be gathering. So clearly there is some happy medium amount of sheep to put in your pasture to get the most efficiency. The purpose of this project is to determine the most efficient density with which to pack your pasture.

*This investigation was conducted in minecraft version 1.19.4.*

## Minecraft background

*Minecraft* is an open-world sandbox video game. The game takes place in a procedurally generated world made of cubic voxels, or *blocks*. The game has no goal, the player can explore wilderness, 

## Key assumptions and notes
There are a couple key assumptions being made here:
- Grass growth is a simple process in which all dirt blocks have an equal chance to turn into a grass block on a tick. This is not entirely realistic, since grass growth is contingent on spreading from an adjacent block, but as long as dirt blocks consistently have adjacent grass blocks it should work.
-  The actual rate at which the farm produces wool is limited by the rate at which the farmer shears the sheep. Initially, we will assume the farmer is infinitely fast, so really the goal is to have the greatest rate of sheep eating grass. Later, we will look at the mechanics of a farm where the farmer harvests the wool at regular intervals.
- Another important thing to note is that pastures can be discussed in terms of *blocks* or *area*. One block is canonically one square meter, So pasture size has units of area. However, area in minecraft is discretized, i.e. you cannot have fractional area. For this reason, pasture area will be discussed using the unitless quantity *blocks*.

## Math
### The sheep
We can find a mathematical expression for the rate at which sheep eat grass.

On any given tick, sheep have a chance to attempt to eat 1 block of grass. We can assume that the rate of a single sheep attempting to eat is constant. We'll call this $R$ \[s<sup>-1</sup>].

The rate at which a flock of sheep attempts to eat is $R$ times the number of sheep. We'll call the number of sheep $N$ \[unitless]. So the rate of the flock attempting to eat is $NR$ \[s<sup>-1</sup>].

The rate at which the flock *succeeds* in eating is dependent on what fraction of the flock is standing on grass. If the sheep and grass are uniformly distributed, the fraction of the flock standing on grass is equal to the fraction of the pasture that is grassy. Let's call the fraction of grass cover $g$ \[unitless]. So the actual rate at which the flock eats is $NRg$. 

### The grass
The rate at which grass grows is a little complicated. If there is barely any grass, then grass growth will be limited by the lack of places the grass can spread *from*. If there is barely any dirt, grass growth will be limited by a lack of places for the grass to spread *to*. This can be modelled by the logistic function. This is a function with a special derivative:

$$f'(x)=kf(x)(1-f(x))$$

If $f(x)$ is small, $f'(x)$ will also be small, because there is a lack of grass to spread. If $f(x)$ is close to 1, then $f'(x)$ will be small because there is a lack of places for the grass to spread to. In our case, $f(x)$ is the fraction of the pasture that is grassy, i.e. $g$. We also have a constant, $k$ \[s<sup>-1</sup>], which represents the rate at which grass attempts to spread.

Putting this all together

$$g'=kg(1-g)$$

Let's define the pasture size $A$ \[unitless]. If we multiply the above equation by $A$, we can express the rate of change of the total grass area: 

$${Ag'= kAg(1-g)}\tag{1}$$

### Putting it together

We can now create an expression for the rate at which the sheep are eating.

After the flock has been munching for a while, the rate at which the flock is eating should be equal to the rate at which grass is regrowing. Setting our two expressions equal to each other, we can say that at steady state,

$$NRg=Ag'=kAg(1-g)$$



This expression is important because it gives us the equilibrium grass cover as a function of sheep population - a little algebra gives 

$${g=1-\frac{NR}{Ak}}\tag{2}$$

Plugging this into our logistic model for grass growth (1) gives

$${Ag' = NR - \frac{N^2R^2}{Ak}}\tag{3}$$

This is important - the steady-state grass growth rate as a function of sheep population. This is also equal to the rate at which the sheep are eating the grass.

If the farmer is shearing sheep as quickly as they get their wool, they are getting 1 block of wool for every block of grass the sheep eat. Let's define $F$ \[s<sup>-1</sup>] as the rate at which the farmer gathers wool.

$${F_{fast}=Ag'=NR - \frac{N^2R^2}{Ak}}\tag{4}$$

We can find the maximum by differentiating (4) with respect to $N$, setting the derivative to 0 and solving for $N$:

$$\frac{dF}{dN}=R-\frac{2NR^2}{Ak}=0$$

$${N=\frac{Ak}{2R}}\tag{4}$$

According to the assumptions we've made, this is the sheep population that will produce the maximum rate of grass growth. At this population,

$${F_{fast}=\frac{Ak}{4}}$$

It's important to note that if we plug (4) back into (2), our expression for equilibrium grass cover, we get $g = 0.5$. This should make sense - the derivative of the logistic function is maximum when $f(x) = 0.5$.

Let's also note that there is a certain population of sheep at which all the grass will be eaten:

$$N=\frac{Ak}{R}$$

### The realistically slow farmer

For the case of the infinitely fast farmer, the rate at which wool is harvested is the same as the rate at which the sheep are eating. To account for the farmer being slow, we'll need to think a bit more.

The farmer is off doing other things, and every $T$ seconds, they come back to shear the sheep. Upon shearing the sheep, all the sheep are wool-less. By the time they come back for the next shearing, the sheep have regrown some amount of wool. Let's say the number of sheep that have their wool is $w$ \[unitless]. So when the farmer shears the sheep, they get $w$ wool. Then the average rate at which the farmer gathers wool, per block of pasture, is

$$F_{slow}=\frac{w}{T}$$

Now we need to consider the value of $w$. A sheep will only regrow wool upon eating if it doesn't already have any, so we should expect that the rate at which sheep are regrowing wool is the rate at which they are eating, $NRg$, times the fraction of sheep that are wool-less. The rate at which the sheep are eating is $g'$, the rate at which sheep are regrowing their wool is:

$$\frac{dw}{dt}=\frac{N-w}{N}NRg=(N-w)Rg$$

At time $t=0$, the farmer shears all the sheep, leaving them with $w=0$, and then at time $t=T$, the farmer comes back and shears them again, gathering $w$ blocks of wool. How many sheep grew their wool back in time $T$ \[s]? We can find out by solving the above equation for $w$. We'll separate it into $w$ terms and non $w$ terms:

$$\frac{1}{N-w}dw=(Rg)dt$$

and integrate from initial state $(t=0, w=0)$ to the final state $(t=T, w=w_T)$. We're assuming the pasture has reached steady state, so $g$ is independent of $t$ and can be treated as a constant.

$$\int_{0}^{w_T}\frac{1}{N-w}dw=\int_{0}^{T}Rgdt$$

$$-\ln(\frac{N-w_T}{N})=TRg$$

And solve for $w_T$:

$$w_T=N\left(1-e^{-TRg}\right)$$

So when the farmer comes back to shear the sheep at $t=T$, they will get $N-Ne^{-TRg}$ blocks of wool. If the farmer keeps coming back every $T$ seconds, they will be gathering wool at a rate

$$F_{slow}=\frac{N\left(1-e^{-TRg}\right)}{T}$$

Finally, we can plug (2) for g:

$${F_{slow}=\frac{N\left(1-e^{-TR+\frac{TR^2N}{Ak}}\right)}{T}}\tag{5}$$

This is the expression for the rate of wool gathering by a slow farmer. The optimal density of sheep is now dependent on T. We can employ the following approximation, for small $x$:

$$e^x-1\approx x$$

to see that for small values of $T$, $F_{slow}\approx F_{fast}$:

$$F_{slow}\approx NR-\frac{N^2R^2}{Ak}=F_{fast}$$

To find the optimal sheep population, we can take the same approach as we did with the fast farmer case. Differentiating (5) w.r.t. $N$:

$$\frac{dF}{dN}=\frac{1-\left(\frac{TR^2N}{Ak}+1\right)e^{-TR+\frac{TR^2N}{Ak}}}{T}=0$$

Attempting to solve for $N$ leads to

$$\left(\frac{TR^2N}{Ak}+1\right)e^{\frac{TR^2N}{Ak}+1}=e^{TR+1}$$

At this point we must invoke the lambert $W$ function, a nonelementary function satisfying

$$W(x)e^{W(x)}=x$$

applying this leads to

$$W(e^{TR+1})=\frac{TR^2N}{Ak}+1$$

$$N=\frac{Ak}{TR^2}(W(e^{TR+1})-1)$$

Plugging this back into the (5), we get

$$F_{slow}=\frac{Ak(W(e^{TR+1})-1)(1-e^{-TR+W(e^{TR+1})-1})}{R^2T^2}$$

-f_slow looks linear w.r.t. N for low values of N and high values of T?

## Experimental Work
Now what? It seems like we have the answer we wanted, but what about k and R? we need to know the actual rate at which sheep eat, and the rate at which grass grows.

The user-maintained minecraft wiki says that sheep have a 1/1000 chance of attempting to eat on every other tick (a tick being 1/20 of a second). With ten chances per second at a probability of 1/1000, the expected value is 0.01. So we can estimate that R=0.01. However, this is based on data from a user-operated wiki with no citation standards and questionable accountability. So we will be conducting an in-game experiment to determine R.

The grass mechanics are sufficiently complicated that it will also be easier for us to set up an experiment in-game to estimate the logistic growth parameters.

### Sheep eating experiment
Determining R is not terribly difficult. We can simply set a timer and count how many times a sheep eats within that time.

Counting the number of times the sheep eats is done as follows. The sheep is put in a 1x1 block enclosure, standing on a grass block. Facing the grass block is an observer. The observer is connected to a dropper, full of items and facing a chest. When the sheep eats the grass, it turns into dirt, activating the observer and causing an item to be placed in the chest. The number of times the sheep eats is counted by the number of items in the chest.

The observer is also connected to a command block, which is set to place a grass block under the sheep using `/setblock`. When the sheep eats the grass block, turning it into dirt, the command block will immediately replace the dirt with another grass block, ensuring that the sheep is always standing on grass.

Ten of these sheep cells were allowed to run for 1 hour. The experiment was started and stopped using command blocks to `/summon` and `/kill` sheep in all cells simultaneously.

In one hour, the sheep consumed **320** blocks of grass. This gives an R value of 0.00888 m<sup>2</sup>s<sup>-1</sup>.

### Grass growth experiment

Determining k is a little more complicated than R. The idea with this experiment is to begin with a large area of dirt with uniformly distributed grass blocks, and allow it to spread while recording the grass cover at regular intervals.

The experiment was set up as follows. A 30x30 flat plot of dirt, isolated from any other nearby dirt and grass blocks, was partially covered in grass in this pattern:

## Simulations
Finally, to validate all this math and get some actual insight into the sheep and their activities, we will be constructing first-principles simulations of grazing sheep.