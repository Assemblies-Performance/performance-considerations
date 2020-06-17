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

## First improvements

The improvements:
- multiprocessed initiation - using multiple processes when generating random values for the connections between areas and stimuli
- numpy stuff - used numpy functionalities better

Here are the graphs:
|first_projects| many_projects |
|--|--|
| ![enter image description here](https://github.com/Assemblies-Performance/graphs/blob/master/perforamance1/first_projects.jpg?raw=true) | ![enter image description here](https://github.com/Assemblies-Performance/graphs/blob/master/perforamance1/many_projects.jpg?raw=true) |
