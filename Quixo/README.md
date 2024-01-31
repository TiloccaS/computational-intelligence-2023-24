# Quixo
This project was done by Tilocca Salvatore s305938 and Davide Natale s318967
Quixo is a strategic board game created by Thierry Chapeau and published by GIGAMIC. The game is a variation of the classic game "Noughts and Crosses" (Tic-Tac-Toe), featuring a 5x5 board on which players try to align five of their symbols (circles or crosses) horizontally, vertically, or diagonally to win. The unique aspect of Quixo is that players can move the rows or columns of the board instead of individual pieces, adding an element of strategy and tactics.
The goal of this project is to create a player able to play and get good results with the game of quixo, using techniques seen in class such as Adversarial Search and Reinforcement Learning

## MinMax Strategy
The strategy of the MinMax was inspired by the following repo: https://github.com/Berkays/Quixo. 
The MinMax is proposed with alpha beta pruning, with the addition of a fitness function that allows you to give a value to the state even if it is not the end of the game but in the final state of the recursion due to the limited depth, the values returned by the fitness are the following:
* 100/-100 if there is a winner 
* +5/-5  for each row, column or diagonal that has 4 checkers consecutive
* +20/-20 if the player take the centre of the board
* The difference between the number of pieces of the x and the o in the board
The code has been adapted to the board proposed by our project and other evaluation strategy have been added to the fitness, such for instance the way we check the consecutive checkers or the fact of considering the occurrences also in the main and secondary diagonal, in addition we slightly change the score giving from a fitness

### Results Obtained
The results obtained are impressive:
* Player 1 = 100% of win
* Player 2 = 100% of win

## RL Strategy
For the RL strategy we used QLearning, creating two separate dictionaries, one for x and one for O, due to the fact that unlike tic tac toe, a move that is available for the first player is not said to be available for the second player. The reward has been set to 1 if the first player -1 wins if the second player wins, 0 otherwise.
We decided to use the default dict as it offered the possibility to make the training faster, avoiding checking the presence of keys in the dictionary. 
We decided to use .pik file with dill module for the dictionary as it allowed to save custom classes in the file, without convert it in string.
Despite the use of symmetry techniques to reduce the size of the dictionary, the RL has not proved to be a good strategy, as it has excessive memory consumption, probably it takes other techniques because try to reduce its consumption even more.

### Results Obtained
Since the dictionary has not been fully trained due the fact the excessive memory consumption the results obtained are not good(the length of the dictionaries is approximately 3 million):
•	Player 1 = 49.5% of win
•	Player 2 = 45.6% of win

### How we manage symmetry in RL:
For the example we show only 3 matrix with 3x3 dimension,  but we use symmetry of all possible rotations and all possible mirrors:
Giving the Game._board, we extract its state in a named tuple, for each state we have the coordinate of the cells selcted: State = namedtuple('state', ['x' 'o']), we put namedtuple in CustomState class.
We used the custom state class, which represents the current state of the board as the key for the dictionary, so we used the following __hash__  function, and the following _eq_ function, custom state take in input namedtuple('state', ['x', 'o']):
```

def __eq__(self, other_state):
        #To see if one representation is equivalent to another one we extract indices in the 'MAGIC' board 
        #and then we search those indexes in all magic boards to extract the equivalent representations
        #If one is equivalent to other_state, the two state are the same
    
        current_state = self.get_equivalent()
        other_state = other_state.get_equivalent()
        return current_state.x == other_state.x and current_state.o == other_state.o
    
    def __hash__(self):
        return hash(str(self.get_equivalent()))

```
### Example of symmetry:

![Alt text](https://github.com/TiloccaS/computational-intelligence-2023-24/blob/main/Quixo/0ffe1648c7fa12a3b0ecd3c075e28833-34.jpg)





