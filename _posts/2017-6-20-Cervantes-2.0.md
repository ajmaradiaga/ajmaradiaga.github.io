---
layout: post
title: Cervantes 2.0
---
![Miguel Cervantes de Saavedra]({{ site.baseurl}}/images/cervantes.jpg)

## Background

[Deep Learning](https://en.wikipedia.org/wiki/Deep_learning) is a new area of [Machine Learning](https://en.wikipedia.org/wiki/Machine_learning), which has attracted a lot of attention lately due to the amazing results produced by Deep Learning models. With Deep Learning, it is now possible for an algorithm to predict things, classify images (objects) with great accuracy, detect fraudulent transactions, generate image, sound and text. These are tasks that were previously not possible to achieve by an algorithm and now perform better than a human.

In this project we will focus on Text Generation. Text Generation is part of [Natural Language Processing](https://en.wikipedia.org/wiki/Natural_language_processing) and can be used to [transcribe speech to text](http://www.jmlr.org/proceedings/papers/v32/graves14.pdf), perform [machine translation](http://arxiv.org/abs/1409.3215), generate handwritten text, image captioning, generate new blog posts or news headlines. 

RNNs are [very effective](http://karpathy.github.io/2015/05/21/rnn-effectiveness/) when understanding sequence of elements and have been used in the past to generate text. I will use a Recurrent Neural Network to generate text inspired on the works of Cervantes.

![Basic RNN -> Unrolled RNN]({{ site.baseurl}}/images/basic_unrolled_RNN.png)

In order to generate text, we will look at a class of Neural Network where connections between units form a directed cycle, called Recurrent Neural Network (RNNs). RNNs use an internal memory to process sequences of elements and is able to learn from the syntactic structure of text. Our model will be able to generate text based on the text we train it with.

## Problem

[Miguel de Cervantes Saavedra](https://en.wikipedia.org/wiki/Miguel_de_Cervantes), was a Spanish writer who is regarded as the greater writer in Spanish language. Famous for his novel, [Don Quixote](https://en.wikipedia.org/wiki/Don_Quixote), considered one of the best fiction novels ever written.

Unfortunately, Cervantes passed away 500 years ago and he will not be publishing new novels any time soon…. But, wouldn’t it be great if we could generate some text inspired on Don Quixote and other novels he published?

To solve our problem, we can use text from novels written by Cervantes in combination with the incredible power of Deep Learning, in particular RNNs, to generate text. Our deep learning model will be trained on existing Cervantes works and will output new text, based on the internal representation of the text it was trained on, in the Neural Network.  

![LSTM Cell]({{ site.baseurl}}/images/lstm_cell.png)

LSTM Cell

For our model to learn, we will use a special type of RNN called LSTMs (Long Short Term Memory), capable of learning long-term dependencies. LSTM can use its memory to generate complex, [realistic sequences](https://arxiv.org/pdf/1308.0850.pdf) containing long-range structure, just like the sentences that we want to generate. It will be able to remember information for a period of time, which will help at generating text of better quality. 

## Repository

The solution explained below is available on this Github repository: [Cervantes - Text Generation](https://github.com/ajmaradiaga/cervantes-text-generation)

## Dataset

To train our model we will use the text from his most famous novel (Don Quixote) and other [less known](http://www.gutenberg.org/cache/epub/14420/pg14420.txt) like Lady Cornelia, The Deceitful Marriage, The Little Gipsy Girl, etc. Also, we will not include any Plays, e.g. [Numancia](https://en.wikipedia.org/wiki/Miguel_de_Cervantes#La_Numancia), to train our model as it’s writing style differs from the novels and we want the generated text to follow the structure of a novel. All the novels are no longer protected under copyright and thanks to the [Gutenberg Project](https://www.gutenberg.org/), we are able to access all the text of [these books](https://www.gutenberg.org/ebooks/author/505).

Even though Miguel de Cervantes native language was Spanish, the text used to train our model will be in English. This is to make it easier for the reader to understand the input and output of our model.

Our Dataset is small as it is composed of only 2 files - Don Quixote and Exemplary Novels with a total size of 3.4 MB. Bigger datasets work better when training an RNN but for our case that is very specific it will be enough. Some additional information of the contents of the files below:

<table>
  <tr>
    <td>**File**</td>
    <td></td>
    <td>**Totals**</td>
    <td></td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td>*Name*</td>
    <td>*Size*</td>
    <td>*Pages*</td>
    <td>*Lines*</td>
    <td>*Words*</td>
    <td>*Unique Words*</td>
  </tr>
  <tr>
    <td>DonQuixote.txt</td>
    <td>2.3 MB</td>
    <td>690</td>
    <td>40,008</td>
    <td>429,256</td>
    <td>42154</td>
  </tr>
  <tr>
    <td>ExemplaryNovels.txt</td>
    <td>1.1 MB</td>
    <td>303</td>
    <td>17,572</td>
    <td>189,037</td>
    <td></td>
  </tr>
</table>
* Note: Values in the table above will change after preprocessing.

There is some manual preprocessing that we will need to do as the text retrieved from Gutenberg Project contains additional content that is not necessary to train the model, for example:

* Preface
* Translator’s Preface
* About the author
* Index
* Dedications
* Footnotes included in Exemplary Novels

**Note:** The files included in the dataset folder no longer contain the additional content mentioned above.

### Dataset Stats

- Unique words: 39229
- Number of chapters: 135
- Average number of sentences in each chapters: 392.7925925925926
- Number of lines: 53162
- Average number of words in each line: 10.975941461946503

### Extra Preprocessing 
We need to prepare our data for our RNN, lets do some additional preprocessing:
* Lookup table: We need to create [word embeddings](https://www.tensorflow.org/tutorials/word2vec#motivation_why_learn_word_embeddings) to facilitate the training of our model. 

* Tokenize punctuation: This is to simplify training for our neural network. Making it easy for it to distinguish between *mad* and *mad!*

### Tokenize Punctuation
We'll be splitting the script into a word array using spaces as delimiters.  However, punctuations like periods and exclamation marks make it hard for the neural network to distinguish between the word "mad" and "mad!".

Implement the function `token_lookup` to return a dict that will be used to tokenize symbols like "!" into "||Exclamation_Mark||".  Create a dictionary for the following symbols where the symbol is the key and value is the token:
- Period ( . )
- Comma ( , )
- Quotation Mark ( " )
- Semicolon ( ; )
- Exclamation mark ( ! )
- Question mark ( ? )
- Left Parentheses ( ( )
- Right Parentheses ( ) )
- Dash ( -- )
- Return ( \n )

This dictionary will be used to token the symbols and add the delimiter (space) around it.  This separates the symbols as it's own word, making it easier for the neural network to predict on the next word. Make sure you don't use a token that could be confused as a word. Instead of using the token "dash", try using something like "||dash||".

After preprocessing all data, it is saved in the local folder. This file is available in the repository as well - [preprocess.p](https://github.com/ajmaradiaga/cervantes-text-generation/blob/master/preprocess.p)