---
title: "Finding the Best Starting Eleven using Simulated Annealing"
date: 2022-11-23
usemath: true
---

# A complete graph for a complete squad

At the 2022 FIFA World Cup in Qatar, each team has a squad of 26 players, but only 11 can be selected in the starting lineup. This means there are 
26 choose 11 = 7726160 different combinations for the manager to choose from! National team managers are typically very busy people and don't have the time to check each possibility, prompting them to seek the help of a mathematician. 

To assist us, the coach provides a crucial piece of information: a 'score' of how well each pair of footballers play together. For example, if two players go well in the same team, the score between them would be low. On the other hand, if the two make a horrid combination, like what could happen if they play in the same position, then their score would be very high. Typically, good players would have low scores with all their teammates, while fringe players would have high scores. 

We begin with a simpler scenario to better understand the problem. Consider a squad with only five players and suppose you need to select a team of three. Each player can be thought of as a vertex on a graph. The verticies are connected to each other by weighted edges, where the weighting corresponds to the score between players. Below is what such a graph could look like.

##### Insert image

Since every vertex is connected to all others by a weighted edge, we have a *complete weighted graph*. If we choose three verticies and keep the edges that connect them, we get a weighted subgraph, and the *cost* is the sum of the weights. For example, if we select players 1, 2 and 3, then the cost of the team would be 1 + 2 + 3 = 6. The task is to find a team of three players which minimise the cost. 

As we do not have too many players, we can just list all possible combinations and find their cost. 

##### Insert table with costs




![Player pair scores](/assets/PlayerChem.png)

<a href="assets/SocceroosGraph.xlsx" download>Download the Excel file</a>

# The Simulated Annealing optimisation algorithm

```matlab
this is code
```
