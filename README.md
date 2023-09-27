# minecraft_sheep
  You need a bunch of wool for your minecraft building project. You are setting up a pasture where you can raise sheep and collect their wool. Should you pack the pasture as densely as possible to fit in the most sheep? Well, probably not. Minecraft sheep, once sheared, will only regrow their wool when they eat grass. If the pasture is full, the sheep will eat all the grass and they won't be able to grow any more wool. But if you don't put enough sheep in the pasture, you're missing out on a bunch of wool you could be gathering. So clearly there is some happy medium amount of sheep to put in your pasture to get the most efficiency. The purpose of this project is to determine, using simulations, the most efficient density with which to pack your pasture.

## Key assumptions
  There are a couple key assumptions being made with this simulation.
- Grass growth is a simple process in which any dirt block has a chance to turn into a grass block on a tick. This is not entirely realistic, since grass growth is contingent on spreading from an adjacent block, but as long as dirt blocks consistently have adjacent grass blocks it should work.
- the goal is to have the greatest rate of sheep regenerating their wool. I am ignoring the fact that sheep only regenerate wool if they don't already have any. So really the goal is to have the greatest rate of sheep eating grass.
