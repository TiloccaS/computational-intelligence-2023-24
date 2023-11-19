# Lab2 Nim Sum 
## Solution made by Tilocca Salvatore s305938 and Natale Davide s318967 
In this laboratory we tried to implement the evolutionary strategy in the context of the game of nim. In particular our method develops 6 different strategies,  you will use each of them with a probability directly proportional to the weight associated with it, the weight for each strategy is calculated through the evolutionary strategy. 
### We have 4 different possible ES proposals:
- 1+λ
- 1,λ
- 1+λ Adaptive
- 1,λ Adaptive

  
The latter two, however, cannot be used with many iterations, as as the number of iterations increases we have a convergence of sigma to 0. 
The different ES will return a vector containing the weights of each strategy, the fitness for the weights is calculated as a weighted average with the number of win on 100 matches.
### The matches are made according to 4 scenarios:
1. Making the first move against Optimal
2. Making the first move against Pure Random
3. Making the first move to the opponent who as he used the optimal strategy
4. Making the first move to the opponent who as he used the optimal strategy
This way we can have a more balanced test
### The 6 strategies are:
- Strategy 1: very similar to the optimal but with the difference that in case we were with a row left we remove all the sticks except 1, in this way the opponent is obliged to draw the last
- Strategy 2: Optimal 
- Strategy 3: Similar to the optimal but it search the steps that give us NimSum=0
- Strategy 4: Here we search to maintain the same nim sum of the actual state, or the nim sum more similar to it
- Strategy 5: Is the gabriele strategy
- Strategy 6: We select the row with most sticks, and from it we remove random sticks 

Here the results:

| 1+λ | 1,λ |  1+λ Adaptive | 1,λ Adaptive |
|-----|-----|---------------|--------------|
| 69  | 67  |       61      | 64|

On the solution not adaptive we set σ=1 and λ=20 ,in the adaptive instead λ=100 and we obtained in both cases sigma close to zero.


NB the result could be different beacause the test are also made with random strategy.
### Review Done
Add review to [Tartaglione Salvatore](https://github.com/SaSa0011/Computational-Intelligence/issues/2) and [Cabras Salvatore](https://github.com/SalvoCab99/Computational-Intelligence/issues/1)

