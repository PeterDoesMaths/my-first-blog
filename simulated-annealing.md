---
title: "Finding the Best Starting Eleven using Simulated Annealing"
author: Peter Moskvichev
date: 2022-11-23
use_math: true
---

# A complete graph for a complete squad

At the 2022 FIFA World Cup in Qatar, each team has a squad of 26 players, but only 11 can be selected in the starting lineup. This means there are $\binom{26}{11} = 7726160$ different combinations for the manager to choose from! National team managers are usually very busy people and don't have the time to check each possibility, prompting them to seek the help of a mathematician. 

To assist us, the coach provides a crucial piece of information: a 'score' of how well each pair of footballers play together. For example, if two players go well in the same team, the score between them would be low. On the other hand, if the two make a horrid combination, like what could happen if they play in the same position, then their score would be very high. Typically, good players would have low scores with all their teammates, while fringe players have high scores. 

We begin with a simpler scenario to better understand the problem. Consider a squad with only five players and suppose you need to select a team of three. Each player can be thought of as a vertex on a graph. The verticies are connected to each other by weighted edges, where the weighting corresponds to the score between players. Below is what such a graph could look like.

<img src="assets/5PlayerGraph.png" width="400">

Since every vertex is connected to all others by a weighted edge, we have a *complete weighted graph*. If we choose three verticies and keep the edges that connect them, we get a weighted subgraph, and the *cost* is the sum of the weights. For example, if we select players 1, 2 and 3, then the cost of the team would be 1 + 2 + 3 = 6. 

More formally, we can represent the weight between players with the symmetric matrix

<img src="assets/WeightMatrix.png" width="300">

where the entry $W_{ij}$ is equal to the weight of the edge joining verticies $i$ and $j$. Then, for a team of players $x = (x_1,x_2,x_3)$, where each $x_i$ is a unique number in the set $\{1,2,3,4,5\}$, the cost is defined by

$ f(x) = \frac{1}{2}\sum_{i=1}^3 \sum_{j=1}^3 W_{x_i, x_j}.$

The task is to find three players $x$ which minimise the cost $f(x)$. As we do not have too many vertices, we can just list all possible combinations and find their cost. 

| Combination: $x$ | Cost: $f(x)$ |
| -----------: | ----: |
| (1,2,3)    | 6 |
| (1,2,4)    | 3 |
| (1,2,5)    | 4 |
| (1,3,4)    | 6 |
| (1,3,5)    | 4 |
| (1,4,5)    | 5 |
| (2,3,4)    | 5 |
| (2,3,5)    | 7 |
| (2,4,5)    | 6 |
| (3,4,5)    | 6 |

Using this exhaustive search we see that the three player combination with the lowest cost is (1,2,4). However, as the number of players increases, listing every combination becomes infeasible, and more sophisticated methods are required to find the best team.

# The Simulated Annealing optimisation algorithm

In mathematical terms, we are dealing with a non-convex optimisation problem with a discrete search space. Luckily, there exists a search method called *simulated annealing* which suits our problem particularly well. Essentially, we iterate in a special way over the search space in an attempt to find the global minimiser. Here's a breif outline of how it works

1. Begin with some guess of the solution
2. Choose a 'neighbouring' solution and determine the change in cost $\Delta f$
3. If $\Delta f \leq 0$, we keep the new solution
4. If $\Delta f > 0$, we may or may not keep the solution, based on the probability of acceptance.
    - The probability of acceptance decreases as $\Delta f$ increases, and
    - The probability of acceptance decreases as the 'temperature' $T$ decreases
5. Decrease the temperature and repeat the process for as long as you have time for or until the global minimum is reached. 


There are a number of things to unpack here. First of all, in this problem we will define two solutions as neighbouring if they differ by only one vertex. In the previous example, we would say (1,2,3) and (1,2,4) are neighbouring. This ensures our step sizes are not too large. 

The probability of acceptance is based on an acceptance function. Many different acceptance functions can be used for simmulated annealing, but the one we will go for is 

$ P(accept | \Delta f) = \exp(\frac{-\Delta f}{T}).$

In practice, whether or not a step is accepted is determined by generating a random number between 0 and 1, and comparing it with $P(accept | \Delta f)$. If $P(accept | \Delta f)$ is greater than the random number, we accept the new $x$, otherwise, we keep the previous $x$. 

Finally, there is the idea of the temperature $T$. At the beginning of the search, the temperature is high, which makes $P(accept | \Delta f)$ high, meaning we are more likely to accept a new solution even if the cost increases. As the search continues, the temperature decreases, making it more likely to accept a solution only if it decreases the cost. The reason for the temperature is that it prevents us from converging to a local minimum. 

Here is how a simulated annealing search may look for out previous example, starting with the initial guess $x = (2,3,5)$. 

| $x$ | $\Delta f$ | Notes |
| -----------: | ----: | --- | 
| (2,3,5)    |  | We start with an initial guess, and then randomly choose a neighbour|
| (1,3,5)    | -3 | The cost decreases so we accept the new solution |
| (1,4,5)    | 1  | The cost has increased, but we are early in the search and the temperature is high so we may still accept the new solution |
| (1,3,5) | 1 | The cost again increases and this time we might reject the new solution and stick with (1,4,5)|
|(1,2,4) | - 2 | The cost decreases and we have reached the global minimum |


# Simulated Annealing for Arnold's Socceroos

Now that we understand how simulated annealing works, let's help out Australian's coach Graham Arnold pick the best starting lineup for the Socceroos. Unfortunately, Mr. Arnold was unable to provide a list of player pair scores, so we will have to make do with my estimate. 

![Player pair scores](/assets/PlayerChem.png)

**Note: These are complete guesses on my end and I by no means claim to be a football expert! If you wish to change the above weights, fell free to download the Excel file using the link.**

<a href="assets/SocceroosGraph.xlsx" download>Download the Excel file</a>

In the spreadsheet, all 26 players of the Australian squad are listed along with their jersey number and position. The table of values represents the matrix of weights $W$ which we defined earlier. The lower the score between players, the more green the cell is. 

A few things may stand out. Clearly, it would not be a good idea to have two goalkeepers in the starting lineup, so the score between them is very high at 100. In all other cases, the score between players is an integer between 1 and 10. 


The optimisation problem is to choose an $x$, where the elements of $x$ are eleven unique integers between 1 and 26, that minimises the cost function

$ f(x) = \frac{1}{2}\sum_{i=1}^{11} \sum_{j=1}^{11} W_{x_i, x_j}.$



```matlab
this is code
```
