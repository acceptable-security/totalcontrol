# total control

Simulation, or something like that. No idea what this is supposed to be but I had a bunch of ideas I wanted to write
down.

## GOAP

The idea behind GOAP is that instead of writing FSMs for each AI which can induce lots of code duplication and overly
complex systems, chain a series of small actions together to move a state from one place to another. So instead of
writing, `if the AI has a hunger level lower than 80 and it has food, eat.` You have a state that represents the AI
hunger level, and several actions that can modify the state of the AI. One of these actions can include eating. However,
the state of eating has prerequisites: the AI's state must also include food. If the prerequisites are met, then the AI
can execute the action in hopes that the state after the action is what he wanted: hunger level higher. But what if the
AI doesn't have food? Let's draw a picture first:

    current state:    desired state:
    +------------+    +-------------+
    | hunger: 50 |    | hunger: 100 |
    | food: 0    |    | food: **    |
    +------------+    +-------------+

    action: eat
    +-----------------+
    | requires:       |
    |  - food > 1     | <---+
    | outputs:        |     |
    |  - hunger += 10 |     |
    +-----------------+     |
                            |
    action: hunt            |
    +--------------+        |
    | requires:    |        |
    |  -           |        |
    | outputs:     |        |
    |  - food += 1 | -------+
    +--------------+

We want to go from the current state to the desired state, and we have a series of actions we can take to change our
state in a deterministic way. This becomes a problem of path finding (and in implementation can even use the A*
algorithm). Once all possible paths are found, any state change that is a consequence of the action that wasn't intended
can be considered the cost of function, and the sequence of actions with the lowest cost or most optimal cost should be
selected and executed.

## Communication & Collaboration

What happens when an AI has no path to a desired state? Theres simply nothing within his grasp that he can do to achieve
something, maybe he *needs* wood but simply doesn't have an axe or any way to get wood. If there's another AI around,
my idea for communication is the following: the states of the two AI's are summed up, and the capabilities merged, and
a new path using the merged state is calculated. By working together the AI's can open up new possibilities that
otherwise wouldn't've been possible. The cost applied to each individual has to also be taken into account, and the
amount of cost an individual can take for someone else's benefit could also be up to an individual - a hypothetical
"empathy" value: how much someone is willing to sacrifice of themselves for the benefit of someone else.

## Environmental Knowledge

My idea for individuals acknowledging the environment boils down to a quad/octree (depending on the dimensions of the
game). For this example we'll use a 2D world - thus a quadtree. At the beginning, when the AI is just put into the
world, the AI has no knowledge of the world. Thus the quadtree has no subdivisions. Every time the AI observes something
about the environment, the quadtree representing his knowledge of the environment gets more information. Sometimes that
could be a new wall, a new tree, etc. If an AI needs to find a new resource - say it needs wood but it doesn't know
where it could find trees - then it has to explore. Exploration is simply picking the closest area that hasn't been
filled in, and going there to add it's information. Knowledge of the environment could also be shared with others around
the AI.
