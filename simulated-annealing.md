---
title: "Finding the Best Starting Eleven using Simulated Annealing"
author: Peter Moskvichev
date: 2022-11-23
use_math: true
---

# A complete graph for a complete squad

At the 2022 FIFA World Cup in Qatar, each team has a squad of 26 players, but only 11 can be selected in the starting lineup. This means there are $\binom{26}{11} = 7726160$ different combinations for the manager to choose from! National team managers are typically very busy people and don't have the time to check each possibility, prompting them to seek the help of a mathematician. 

To assist us, the coach provides a crucial piece of information: a 'score' of how well each pair of footballers play together. For example, if two players go well in the same team, the score between them would be low. On the other hand, if the two make a horrid combination, like what could happen if they play in the same position, then their score would be very high. Typically, good players would have low scores with all their teammates, while fringe players would have high scores. 

We begin with a simpler scenario to better understand the problem. Consider a squad with only five players and suppose you need to select a team of three. Each player can be thought of as a vertex on a graph. The verticies are connected to each other by weighted edges, where the weighting corresponds to the score between players. Below is what such a graph could look like.

<img src="assets/5PlayerGraph.png" width="400">

Since every vertex is connected to all others by a weighted edge, we have a *complete weighted graph*. If we choose three verticies and keep the edges that connect them, we get a weighted subgraph, and the *cost* is the sum of the weights. For example, if we select players 1, 2 and 3, then the cost of the team would be 1 + 2 + 3 = 6. 

More formally, we can represent the weight between players with the symmetric matrix

<img src="assets/WeightMatrix.png" width="300">

where the entry $W_{ij}$ is equal to the score between players $i$ and $j$. Then, for a team of players $x = (x_1,x_2,x_3)$, where each $x_i$ is a unique number in the set $\{1,2,3,4,5\}$, the cost is defined by

$ f(x) = \frac{1}{2}\sum_{i=1}^3 \sum_{j=1}^3 W_{x_i, x_j}.$

The task is to find a team of three players $x$ which minimise the cost $f(x)$. As we do not have too many players, we can just list all possible combinations and find their cost. 

| Combination: $x$ | Cost: $f(x)$ |
| -----------: | ----: |
| 1 2 3     | 6 |
| 1 2 4     | 3 |
| 1 2 5     | 4 |
| 1 3 4     | 6 |
| 1 3 5     | 4 |
| 1 4 5     | 5 |
| 2 3 4     | 5 |
| 2 3 5     | 7 |
| 2 4 5     | 6 |
| 3 4 5     | 6 |

Using this exhaustive search we see that the three player combination with the lowest score is 1 2 4. However, as the number of players increases, listing every combination becomes infeasible, and more sophisticated methods are required to find the best team.

# The Simulated Annealing optimisation algorithm

In mathematical terms, we are dealing with a non-convex optimisation problem with a discrete search space. Luckily, there exists a search method called *simulated annealing* which suits our problem particularly well. Essentially, we iterate in a special way over the search space in an attempt to find the global minimiser. Here's a breif outline of how it works

1. Begin with some guess of the solution
2. Choose a 'neighbouring' solution and determine the change in cost $\Delta f$
3. If $\Delta f \leq 0$, we keep the new solution
4. If $\Delta f > 0$, we may or may not keep the solution, based on the probability of acceptance.
    - The probability of acceptance decreases as $\Delta f$ increases, and
    - The probability of acceptance decreases as the 'temperature' $T$ decreases
5. Repeat the process for as long as you have time for or until the global minimum is reached. 


There are a number of things to unpack here. First of all, in this problem we will define two solutions as neighbouring if they differ by only one vertex. In the previous example, we would say 1 2 3 and 1 2 4 are neighbouring. This ensures our step sizes are not too large. 

Many different acceptance functions can be used for simmulated annealing, but the one we will go for is 
$ 
P(accept | \Delta f) = \exp(\frac{-\Delta f}{T}).
$

Finally, there is the idea of the temperature $T$. At the beginning of the search, the temperature is high, meaning we are more likely to accept a new solution even if the cost increases. As the search continues, the temperature decreases, making it more likely to accept a solution only if it decreases the cost. The reason for the temperature is that it prevents us from converging to a local minimum. 





# Example


Let's help out the Australian's coach Graham Arnold pick the best starting lineup for the Socceroos. Unfortunately, Mr. Arnold was unable to provide a list of player pair scores, so we will have to make do with my estimate. 

![Player pair scores](/assets/PlayerChem.png)

If you wish to change the above weights, fell free to download the excel file using the link below. 

<a href="assets/SocceroosGraph.xlsx" download>Download the Excel file</a>



```matlab
this is code
```
