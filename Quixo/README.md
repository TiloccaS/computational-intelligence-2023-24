# Quixo
This project was done by Tilocca Salvatore s305938 and [Davide Natale](https://github.com/Davide-Natale/Computational-Intelligence.git) . 
Quixo is a strategic board game created by Thierry Chapeau and published by GIGAMIC. The game is a variation of the classic game "Noughts and Crosses" (Tic-Tac-Toe), featuring a 5x5 board on which players try to align five of their symbols (circles or crosses) horizontally, vertically, or diagonally to win. The unique aspect of Quixo is that players can move the rows or columns of the board instead of individual pieces, adding an element of strategy and tactics.
The goal of this project is to create a player able to play and get good results with the game of quixo, using techniques seen in class such as Adversarial Search and Reinforcement Learning

## MinMax Strategy
First, we implemented a player based on MinMax strategy, our implementation is inspired by the following repository: https://github.com/Berkays/Quixo. 

The MinMax is proposed with alpha beta pruning, with the addition of a fitness function that allows you to give a value to the state even if it is not the end of the game but in the final state of the recursion due to the limited depth (we set depth=2 as default), the values returned by the fitness are the following:
* 100/-100 if there is a winner 
* +5/-5  for each row, column or diagonal that has 4 checkers consecutive
* +20/-20 if the player take the centre of the board
* The difference between the number of pieces of the x and the o in the board

The code has been adapted to the board proposed by our project and other
improvements has been done, such as, for instance, the way we check the consecutive checkers or the fact of considering the occurrences also in 
the main and secondary diagonal, in addition we slightly changed the score giving from the fitness.

### Results Obtained
We tested our player against a Random player, we made them play 1000 games considering the MinMax player as player1 and 1000 games considering as player2. The results obtained are impressive:
* As player 1 &rarr; 100% of win
* As player 2 &rarr; 100% of win

## RL Strategy
Then we tried to implement a Reinforcement Learning player.
For the RL strategy we used QLearning, creating two separate dictionaries, one for `x` player and one for `o` player, due to the fact that unlike tic tac toe, a move that is valid for the first player may not be valid for the second player.
As rewards we used:
* 1 if the first player wins 
* -1 if the second player wins
* 0 if they draw

We decided to use the defaultdict as it offered the possibility to make the training faster, avoiding checking the presence of keys in the dictionary. 
We decided to use .pik file with dill module to save the dictionaries as it allowed to save custom classes in the file, without convert it in string.
For the training we used 100000 steps and the following hyperparameters:
* learning rate = 0.1
* discount factor = 0.7

### How we manage symmetry in RL:
Since Quixo is a complex game, we tried to reduce the dimension of our dictionaries by managing simmetries of the board. 
Below there is an example of 3x3 matrix that shows few cases, but we adapted it for a 5x5 matrix and we considered all possible rotations and mirrors board.

Giving the Game._board, we extract its state in a namedtuple, for each state we have the coordinates of the cells selected:

    State = namedtuple('state', ['x', 'o'])

Then we create the CustomState class, in which we have this namedtuple which represents the current state.
In this way for each state we have an instance of CustomState class that we used as the key for the two dictionaries, in order to do that we implemented the following two functions:
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


Despite the use of symmetry techniques to reduce the size of the dictionaries, the RL has not proved to be a good strategy, as it has excessive memory consumption, probably it takes other techniques to try to reduce its consumption even more.

### Results Obtained
Even for this player, we used the same testing approch. 
Since the dictionary has not been fully trained due the excessive memory consumption the results obtained are not good (the length of both two dictionaries is approximately 3.5 million):
* As player 1 &rarr; 49.5% of win
* As player 2 &rarr; 45.6% of win

As we can see from the results obtained, the player with the best performance appears to be the MinMax player, but most likely with more training the RL player would have obtained better performances than those shown.

## Installation:
This code has been tested on python 3.9.16.
**First of all unzip the dictionary file contained in train_results folder**
```

git clone https://github.com/TiloccaS/computational-intelligence-2023-24.git
python3 -m venv my_env
source my_env/bin/activate
cd Quixo
pip install --upgrade pip
pip install -r requirements.txt

```
## Test players

```

pyhton main.py
--strategy <0: min max and rl, 1: minmax, 2: rl>
--pretrain_path_o <.train_results/rl_o.pik>
--pretrain_path_o <.train_results/rl_x.pik>
--save_model_path_o <path where you want save the dictionary of o>
--save_model_path_x <path where you want save the dictionary of x>
--train < True if you want train new dictionary False Otherwise>
--max_steps <how many epoch for train the dictionary>
--player <0: you will test with first and second player, 1: you will test with first player , 2: you will test with second player>
--num_games <how many games to test the strategies>

```




