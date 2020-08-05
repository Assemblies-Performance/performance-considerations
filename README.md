
![https://img.shields.io/badge/performance-improved-green](https://img.shields.io/badge/performance-improved-green)

## Performance

We implemented the Connectome.project method.
We started by implementing a quite naiive version of the method, but found a few ways to improve the performance.
Every improvement is compared to the original version to see how much it actually improved.
Graphs are included bellow.

The graphs:
- first_projects: first 2 projects are measured separately because they are slower.
- many_projects: 25 more projects.

More specifically, the time measurements perform the following code:
```python
# init_connectome
inst = cls(areas=[a1, a2], stimuli=[s1], p=0.05) 
# first_projects 
inst.project({s1: [a1]})  
inst.project({a1: [a2]})  
# many_projects
for i in range(25):  
inst.project({a1: [a2]})
```
Where the areas and stimuli are initiated in the following way:
```python
N = 1000
BETA = 0.05
a1 = Area(n=(N * i), k=math.floor(math.sqrt(N * i)), beta=BETA)  
a2 = Area(n=(N * i), k=math.floor(math.sqrt(N * i)), beta=BETA)
s1 = Stimulus(n=math.floor(math.sqrt(N * i)), beta=BETA)
i = 1,...,10
```

## Improvements

Initially we had two versions for the connectomes:
__________________________________________________________

### The Lazy Implementation

We started working with a lazy implementation - the connectomes are generated at runtime, so the initiation of the connectomes is very fast, but the running time is relatively slow since random values have to be generated all the time.

### The Non-Lazy Implementation

In this implementation the connectomes are generated at the beginning to have random values, and the rest of the code is deterministic. This means that the generation of the connectome is slower, but the running time after initiation is significantly shorter.
___________________________________________________________

Comparing the two types of connectomes gave the following results:

|first_projects| many_projects |
|--|--|
| ![first_projects](https://github.com/Assemblies-Performance/graphs/blob/master/performance2/first_projects.JPG?raw=true) | ![many_projects](https://github.com/Assemblies-Performance/graphs/blob/master/performance2/many_projects.JPG?raw=true) |

and so we chose to use the non-lazy implementation.

But of course that could be improved further, so we changed it some more:
- multithreaded initiation - using multiple threads when generating random values for the connections between areas and stimuli (dividing the matrix to blocks and filling each block separately with random values).
- numpy stuff - used numpy functionalities better. Particularly, we substituted python operations on numpy types with built-in numpy operations (for example, 2d-array row operation instead of a for loop). These improvements have cut a lot of the overhead created by working with python.

Here are the graphs:
|first_projects| many_projects |
|--|--|
| ![first_projects](https://github.com/Assemblies-Performance/graphs/blob/master/perforamance1/first_projects.jpg?raw=true) | ![many_projects](https://github.com/Assemblies-Performance/graphs/blob/master/perforamance1/many_projects.jpg?raw=true) |
