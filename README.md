# Probabilistic-RNN-DA-Classifier


## Overview

An LSTM for Dialogue Act (DA) classification on the Switchboard Dialogue Act Corpus.
This is the implementation for the paper [Probabilistic Word Association for Dialogue Act Classification with Recurrent Neural Networks](https://www.springerprofessional.de/en/probabilistic-word-association-for-dialogue-act-classification-w/16055614).
The repository contains two LSTM models implemented in [Keras](https://keras.io/).
da_lstm.py uses utterance representations generated from pre-trained Word2Vec and GloVe word embeddings 
and probabilistic_lstm.py uses utterance representations generated from keywords selected for their frequency association with
certain DAs. 

Both models use the same architecture, with the ouput of the LSTM at each timestep combined using a max-pooling layer
before a final feed forward layer outputs the probability distribution over all DA labels for that utterance.

<p align="center">
<img src="/models/architecture.png">
</p>


## Datasets

The data directory contains pre-processed Switchboard DA Corpus data in raw-text (.txt) and .pkl format.
The same training and test splits as used by [Stolcke et al. (2000)](https://web.stanford.edu/~jurafsky/ws97) and an additional validation set is included.
The development set is a subset of the training set to speed up development and testing.

|Dataset    |# Transcripts  |# Utterances   |
|-----------|:-------------:|:-------------:|
|Training   |1115           |192,768        |
|Development|300            |51,611         |
|Test       |19             |4,088          |
|Validation |21             |3,196          |

## Metadata
words.txt and labels.txt contain full lists of the vocabulary and labels along with how frequently they occur.
metadata.pkl contains useful pre-processed data such as vocabulary and vocabulary size, DA label-to-index conversion dictionaries and maximum utterance length.

- num_utterances = Total number of utterance in the full corpus.
- max_utterance_len = Number of words in the longest utterance in the corpus.
- vocabulary = List of tuples (word, word frequency).
- vocabulary_size = Number of words in the vocabulary.
- index_to_word = Dictionary mapping vocabulary index to word.
- word_to_index = Dictionary mapping vocabulary word to index.
- labels = List of tuples (label, label frequency).
- num_labels = Number of labels used from the Switchboard data.
- label_to_index = Dictionary mappings label to index.
- index_to_label = Dictionary mapping index to label.

## Usage
#### Traditional Word Embeddings
To run da_lstm.py an embedding matrix must first be created from pre-trained embeddings such as word2vec or GloVe.
To generate the matrix simply run generate_embeddings.py after specifying the embeddings filename and directory (default = 'embeddings').
Then run da_lstm.py after specifying the name of the .pkl embeddings file generated by generate_embeddings.py.

#### Probabilistic Word Embeddings
To run probabilistic_lstm.py a probability matrix must first be created from the raw swiitchboard data.
Run generate_word_frequencies.py specifying the frequency threshold (freq_thresh) i.e. how many times a word may appear in the corpus to be considered (default = 2).
Then run probabilistic_lstm.py specifying the same word frequency (word_frequency) parameter.

#### Utility Files
- process_all_swbd_data.py - processes the entire corpus into raw-text and generates the metadata.pkl file.
- process_batch_swbd_data.py - processes only a specified list of transcripts from a text file i.e. test_split.txt.
- utilities.py - contains utility functions for saving and loading data and models as well as processing data for use at runtime.
- swda.py - contains utility functions for loading and iterating the switchboard transcripts and utterances in .csv format.
This file is part of the repository developed by Christopher Potts, and is available [here](https://github.com/cgpotts/swda).
