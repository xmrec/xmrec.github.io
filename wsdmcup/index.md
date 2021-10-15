# XMRec: Cross-Market Recommendation Competition @ WSDM 2022

## Overview
E-commerce companies often operate across markets; for instance [Amazon](https://www.amazon.com)
has expanded their operations and sales to 18 markets (i.e. countries) around the globe. 
The cross-market recommendation concerns the problem of recommending relevant products to users in a target market (e.g., a resource-scarce market) by leveraging data from similar high-resource markets, e.g. using data from the U.S. market to improve recommendations in a target market. 
The key challenge, however, is that data, such as user interaction data with products (clicks, purchases, reviews), convey certain biases of the individual markets. 
Therefore, the algorithms trained on a source market are not necessarily effective in a different target market.
Despite its significance, small progress has been made in cross-market recommendation, mainly due to a lack of experimental data for the researchers. In this WSDM Cup challenge, we provide user purchase and rating data on various markets, enriched with review data in different languages, with a considerable number of shared item subsets. The goal is to improve individual recommendation systems in these target markets by leveraging data from similar auxiliary markets.


## Task
### Problem Definition

Given a global set of items I, 
we define a market M as the collection of its users U(M) together with their interactions (i.e. ratings and reviews) with items from I.
Generally, a user can interact with different markets, but for simplicity, we assume that the set of users in each market are mutually disjoint with any other parallel market.
In this competition, we have three source markets, s_1, s_2, s_3, and two target markets, t_1, t_2. 

The goal is to have the best possible recommender system in terms of nDCG@10 on the target markets t_1, t_2.
For that, you can use the data on these markets and also get help from the data available from the source markets s_1, s_2, s_3.


### Train, Validation, and Test Split

The source markets are used for training, and therefore they are provided in a single **Train** split.
For the target markets, we leave one interaction of each user out for the **Test** split, and one other interaction for the **Validation** split. All the remaining interactions are given as the **Train** split. As usual, you are given only the Train and Validation splits.


### Submissions

Your submissions should be two tab separated files (*```target_1.scores.tsv```* and *```target_2.scores.tsv```*) with each line containing three columns as follows:

```
userId	itemId	score
```
where the first column ```userId``` is the user unique id, the second column ```itemId``` is the item unique id, and the third column ```score``` is the score your model assigns to this (user, item) pair.
For example, each of your *```.score.tsv```* files should look like this:
```
VA	E2	1.0 
VA	FQ	0.9 
VA	WS	1.1
...
```
For each user (i.e. each unique value for ```userId```), the items are sorted based on their score in a descending order (equal scores are handled randomly) and the top 10 items in the ranked list are used for evaluation.


## Evaluation

We evaluate the submissions based on their average nDCG@10.
As discussed in submission guidlines, the scores of items are sorted for each user and the top 10 items are considered for evaluation.
For the total evaluation, we concatenate the users of the target markets (*```target_1.scores.tsv```* and *```target_2.scores.tsv```*) and compute the nDCG@10 on the resulting list. The teams are ranked based on this metric.
<br>
For information purposes we also report separate nDCG@10 and HR@10 for each target market, too.


## Data
The training and validation as well as the test run can be downloaded [here](#).
The data is structured as follows:

- There are three folders 
*```s_1```*,
*```s_2```*, and 
*```s_3```*;
containing the data of the source markets. Inside each, the following file can be found:
    - *```train.tsv```*: A tab separated file, containing the **Training** data with the following format:
        ```
            userId	itemId	rating
        ```
        where the first column ```userId``` is the user unique id, the second column ```itemId``` is the item unique id, and the third column ```rating``` is the rating (an integer ranging from 1 to 5).
        This means that our training data only contains the **positive** samples. All the other (user, item) pairs are unknown and can be considered **negative** during training.

- There are two folders
*```t_1```*, and 
*```t_2```*; containing the data of the target markets. Inside each, the following files can be found:
    - *```train.tsv```*: 
        A tab separated file, containing the **Training** data with ```userId```, ```itemId```, and ```rating``` fields the same as above.
    - *```valid_qrel.tsv```*:
        The **Validation positive** samples, with a structure similar to the *```train.tsv```*.
        Note that, the validation set only has one positive sample per user.
    - *```valid_qrun.tsv```*:
        The **Validation** samples. For consistency of results between different teams, we provide you 99 negative samples for each unique ```userId``` as follows:
        ```
            userId	itemId1,itemId2,...,itemId100
        ```
        where the two columns are separated by a tab and the list of items are separated by commas. There are 99 negative samples and 1 positive sample (identified in the *```valid_qrel.tsv```* file) in the list. Your model should rerank these 100 items per user.
    - *```test_qrun.tsv```*:
        The **Test candidate** samples. As is common in recommendation systems, we provide you 100 candidate items for each unique ```userId``` as follows:
        ```
            userId	itemId1,itemId2,...,itemId100
        ```
        where the two columns are separated by a tab and the list of negative items are separated by commas.
        These 100 items should be reranked by your models and the top 10 items for each user should be reported with their scores.
        Only one item from this list of 100 items is a positive sample and the rest 99 items are negative samples.

## Sponsorship
The XMRec WSDM Cup looks for a wide range of financial supports, ranging from the support of top teams award to computational power. Please check the [Call for Sponsorship](https://xmrec.github.io//wsdmcup/sponsorship.pdf) for more information.
        
## Terms and Conditions
The XMRec dataset is free to download for research purposes.
Each team is allowed to submit one run per day and a maximum of 100 runs in total.

Notice that we ask the top teams to provide the code to their models to run them locally after the end of the competition.

At the end of the challenge, each team is encouraged to open source the source code that was used to generate their final challenge solution under the MIT license. To be eligible for the leaderboard or prizes, winning teams are also required to submit papers describing their method to the WSDM Cup Workshop, and present their work at the workshop. Refer to the "Call for Papers" section on the WSDM Cup 2022 webpage for more details.

## Organizers
 - [Mohammad Aliannejadi](http://aliannejadi.com), University of Amsterdam, The Netherlands
 - [Hamed Bonab](https://people.cs.umass.edu/~bonab/), University of Massachusetts Amherst, USA
 - Ali Vardasbi, University of Amsterdam, The Netherlands
 - [Evangelos Kanoulas](https://staff.fnwi.uva.nl/e.kanoulas/), University of Amsterdam, The Netherlands
 - [James Allan](http://ciir.cs.umass.edu/~allan/), University of Massachusetts Amherst, USA
 - [Vanessa Murdock](https://www.amazon.science/author/vanessa-murdock), Amazon Inc., USA

## Contact
For queries, please contact us via [m.aliannejadi@uva.nl](mailto:m.aliannejadi@uva.nl).

