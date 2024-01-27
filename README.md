# Transformer-Based-Yugioh-Comparator-Model

  This model is based on the transformer architecture with the aim to calculate how related 5 cards are from the trading card game Yugioh.

  Data was obtained from ygoprodeck.com

## Method
- I first start by building a tokenized dataset of all the important features of every card currently in the game. This involves
  taking the whole yugioh card database ,that I obtained , and cleaning it by removing any unwanted cards such as staples , removing normal monster text etc. Important features from the
  cards such as their name , text , attribute and type are concatenated together , tokenized and padded. The cards in this tokenized dataset are identifiable by their index value in relation
  to the starting dataset.

- To build the training dataset ,decks are loaded from ygoprodeck.com in a text based format , and 5 cards are then randomly chosen from a each deck (withought replacement) 30 times . They are tokenized , padded and concatenated together. They have 
  a label '1' attached to them to mark they are related since the are from the same deck. To build the part of the training dataset that has a label '0', 5 decks are sampled from the total population of decks and 1 card is chosen from each to be 
  tokenized , padded and concatenated together. This is repeated until we have an equal number of datapoints for the label '0' and the label '1'. This is done to prevent biasing the model with an unbalanced dataset.
  In theory cards from multiple decks can be related hence I have chosen slightly unrelated decks so that the model does not draw on false negatives.

- The training dataset is then split into a training/validation dataset with a split of 0.8:0.2. 

- The model is based on the transformer and specifically is an encoder only architecture . This architecture was chosen for its NLP capabilities such as the multi headed attention which could be good at finding relation between the features of the inputted card data.
  The model structure is as follows -> embedding layer -> layer norm -> Multiheaded attention -> feed forward - > layer norm -> flatten layer -> single output node with sigmoid activation. Positional encoding was removed as it would not have any significant performance improvements.
  Positional encoding is usually used to differentiate word order when understanding language sentence to sentance, however with my models, both text and features of multiple cards are being considered and positional encoding might confuse the model ; more about the absence
  of the encoding layer is discussed later.


## Guide
- Download the whole repository and when running the code for the first time , run everything up to and including the generators (Train_Split_Gen etc). This will download the necessary yugioh card dataset into the project directory. However, when running the code after
  that, do the exact same but do not run the "In case of update section" again.

- After the generator section , there is a commented code block which allows you to tweak the specifics of the training process to build and test a new model. However, if you want to test the prebuilt models instead, you can just run the archived model code block.
- The ModelStats function will give you stats about an archived model or a model you train such as its performance with changing threshold values or the predictions compared to their true label.
- The Model_tester function allows you to test a model by specifying a card list and it will give you a prediction of how related they are based on that.
- The predict_matches function allows you to find similar cards to a list of 4 cards you provide. You can set the range of cards it will search as well as the threshold value used to tune whether a card is worth recommending. This will print out a pictorial list of card matches.
  
## Important Notes
 - The archived models reach an average validation accuracy of -> ~63.5% but in reality the accuracy is shaky at best and leans more towards ~55% with the majority of models trained. This is explainable due to limitations of the data collection process which limits 
   the data quality . This hence led me to build an auto labelling process involving randomness to build a respectably sized training/validation dataset. But due to this randomness,  this dataset itself is very limited in reflecting the true essence of the card game and the
   card relationships. This is also why positional encoding was not very affective. Nonetheless, it was still a cool project to build. Ideally, a perfect dataset would involve detailed information on actual duels between people, but obtaining this data from simulators is 
   incredibly difficult due to their own policies in combatting bots and the like. Another approach could be reinforcement learning but this would involve either building or reverse engineering a simulator which is incredibly time consuming.
 

