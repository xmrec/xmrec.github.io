# WSDM Cup on Cross-Market Recommendation Competition

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

Your submissions should be a zip file, containing two folders *```t1```* and *```t2```*.
In each of these two target folders, there should be two tab separated files *```test_scores.tsv```* (the test scores, i.e. reranked items from *```test_run.tsv```*) and *```valid_scores.tsv```* (the validation scores, i.e. reranked items from *```valid_run.tsv```*) with each line containing three columns as follows:
```
  userId	itemId	score
```
where the first column ```userId``` is the user unique id, the second column ```itemId``` is the item unique id, and the third column ```score``` is the score your model assigns to this (user, item) pair.
For example, each of your files should look like this:
```
  VA	E2	1.0 
  VA	FQ	0.9 
  VA	WS	1.1 
  ...
```
For each user (i.e. each unique value for ```userId```), the items are sorted based on their score in a descending order (equal scores are handled randomly) and the top 10 items in the ranked list are used for evaluation.


This is how your zip file should look like:

<code>
    &nbsp;submission.zip<br>&nbsp;&nbsp;
    &#x251C;&#x2500;&#x2500; t1<br>&nbsp;&nbsp;
    &nbsp;&nbsp;&nbsp;&nbsp;&#x251C;&#x2500;&#x2500; test_scores.tsv&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;/* scores of test items */<br>&nbsp;&nbsp;
    &nbsp;&nbsp;&nbsp;&nbsp;&#x2514;&#x2500;&#x2500; valid_scores.tsv&nbsp;&nbsp;&nbsp;&nbsp;/* scores of validation items */<br>&nbsp;&nbsp;	
    &#x251C;&#x2500;&#x2500; t2<br>&nbsp;&nbsp;
    &nbsp;&nbsp;&nbsp;&nbsp;&#x251C;&#x2500;&#x2500; test_scores.tsv&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;/* scores of test items */<br>&nbsp;&nbsp;
    &nbsp;&nbsp;&nbsp;&nbsp;&#x2514;&#x2500;&#x2500; valid_scores.tsv&nbsp;&nbsp;&nbsp;&nbsp;/* scores of validation items */<br>&nbsp;&nbsp;
</code>

## Evaluation

We evaluate the submissions based on their average nDCG@10.
As discussed in submission guidlines, the scores of items are sorted for each user and the top 10 items are considered for evaluation.
For the total evaluation, we concatenate the users of the target markets (*```t1/scores.tsv```* and *```t2/scores.tsv```*) and compute the nDCG@10 on the resulting list. The teams are ranked based on this metric.
<br>
For information purposes we also report separate nDCG@10 and HR@10 for each target market, too.

## Data
The training and validation as well as the test run can be downloaded [here](#).
The data is structured as follows:

<code>
	&nbsp;data<br>&nbsp;&nbsp;
	&#x251C;&#x2500;&#x2500; s1<br>&nbsp;&nbsp;
	&nbsp;&nbsp;&nbsp;&nbsp;&#x2514;&#x2500;&#x2500; [3.2M]&nbsp;&nbsp;train.tsv&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;/* train data */<br>&nbsp;&nbsp;
	&#x251C;&#x2500;&#x2500; s2<br>&nbsp;&nbsp;
	&nbsp;&nbsp;&nbsp;&nbsp;&#x2514;&#x2500;&#x2500; [2.0M]&nbsp;&nbsp;train.tsv&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;/* train data */<br>&nbsp;&nbsp;
	&#x251C;&#x2500;&#x2500; s3<br>&nbsp;&nbsp;
	&nbsp;&nbsp;&nbsp;&nbsp;&#x2514;&#x2500;&#x2500; [1006K]&nbsp;train.tsv&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;/* train data */<br>&nbsp;&nbsp;
	&#x251C;&#x2500;&#x2500; t1<br>&nbsp;&nbsp;
	&nbsp;&nbsp;&nbsp;&nbsp;&#x251C;&#x2500;&#x2500; [5.9M]&nbsp;&nbsp;test_run.tsv&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;/* list of test items to be reranked */<br>&nbsp;&nbsp;
	&nbsp;&nbsp;&nbsp;&nbsp;&#x251C;&#x2500;&#x2500; [1.7M]&nbsp;&nbsp;train.tsv&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;/* train data */<br>&nbsp;&nbsp;
	&nbsp;&nbsp;&nbsp;&nbsp;&#x251C;&#x2500;&#x2500; [236K]&nbsp;&nbsp;valid_qrel.tsv&nbsp;&nbsp;&nbsp;/* validation positive samples */<br>&nbsp;&nbsp;
	&nbsp;&nbsp;&nbsp;&nbsp;&#x2514;&#x2500;&#x2500; [5.9M]&nbsp;&nbsp;valid_run.tsv&nbsp;&nbsp;&nbsp;&nbsp;/* list of validation items to be reranked */<br>&nbsp;&nbsp;
	&#x2514;&#x2500;&#x2500; t2<br>&nbsp;&nbsp;
	&nbsp;&nbsp;&nbsp; &#x251C;&#x2500;&#x2500; [2.9M]&nbsp;&nbsp;test_run.tsv&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;/* list of test items to be reranked */<br>&nbsp;&nbsp;
	&nbsp;&nbsp;&nbsp; &#x251C;&#x2500;&#x2500; [845K]&nbsp;&nbsp;train.tsv&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;/* train data */<br>&nbsp;&nbsp;
	&nbsp;&nbsp;&nbsp; &#x251C;&#x2500;&#x2500; [116K]&nbsp;&nbsp;valid_qrel.tsv&nbsp;&nbsp;&nbsp;/* validation positive samples */<br>&nbsp;&nbsp;
	&nbsp;&nbsp;&nbsp; &#x2514;&#x2500;&#x2500; [2.9M]&nbsp;&nbsp;valid_run.tsv&nbsp;&nbsp;&nbsp;&nbsp;/* list of validation items to be reranked */<br>&nbsp;&nbsp;
	<br>&nbsp;&nbsp;<br>
</code>

- There are three folders 
        *```s1```*,
        *```s2```*, and 
        *```s3```*;
        containing the data of the source markets. Inside each, the following file can be found:
    - *```train.tsv```*: A tab separated file, containing the **Training** data with the following format:
                ```
                    userId	itemId	rating
                ```
                where the first column ```userId``` is the user unique id, the second column ```itemId``` is the item unique id, and the third column ```rating``` is the rating (an integer ranging from 1 to 5).
                This means that our training data only contains the **positive** samples. All the other (user, item) pairs are unknown and can be considered **negative** during training.

- There are two folders
        *```t1```*, and 
        *```t2```*; containing the data of the target markets. Inside each, the following files can be found:
    - *```train.tsv```*: 
                A tab separated file, containing the **Training** data with ```userId```, ```itemId```, and ```rating``` fields the same as above.
    - *```valid_qrel.tsv```*:
                The **Validation positive** samples, with a structure similar to the *```train.tsv```*.
                Note that the validation set only has one positive sample per user.
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
                Note that only one item from this list of 100 items is a positive sample and the rest 99 items are negative samples.

## Getting started
[This repository](https://github.com/hamedrab/wsdm22_cup_xmrec) provides a sample code for training a simple Generalized Matrix Factorization (GMF) model over several markets. We provide loading data from zero to a few source markets to augment the target market data, which can help the recommendation performance in the target market.

We highly recommend following the structure of our sample code for your own model design, as we ask every team to submit their code along with their submission and share the implementation with the organizers. In the case we are not able to reproduce your results, your submission will be removed from our leaderboard. Please reach out to us if you encounter any problem with using this code or any other questions / feedback.

### Requirements
We use conda for our experimentations. You can use ```environment.yml``` to create the environment (use ```conda env create -f environment.yml```) or install the below list of requirements on your own environment.

<code>
    &nbsp;&nbsp;python 3.7<br>
    &nbsp;&nbsp;pandas-1.3.3<br>
    &nbsp;&nbsp;numpy-1.21.2<br>
    &nbsp;&nbsp;torch==1.9.1<br>
    &nbsp;&nbsp;pytrec_eval<br>
</code>

### Train the baseline GMF++ model
```train_baseline.py``` is the script for training our simple GMF++ model that is taking one target market and zero to a few source markets (separated by dash "-") for augmenting with the target market. We implemented our dataloader such that it loads all the data and samples equally from each market in the training phase. You can use ```ConcatDataset``` from ```torch.utils.data``` to concatenate your ```torch``` datasets.


Here is a sample train script using zero source market (only train on the target data):

```python
python train_baseline.py --tgt_market t1 --src_markets none --tgt_market_valid DATA/t1/valid_run.tsv --tgt_market_test DATA/t1/test_run.tsv --exp_name toytest --num_epoch 5 --cuda
```

Here is a sample train script using two source markets:

```python
python train_baseline.py --tgt_market t1 --src_markets s1-s2 --tgt_market_valid DATA/t1/valid_run.tsv --tgt_market_test DATA/t1/test_run.tsv --exp_name toytest --num_epoch 5 --cuda
```

After training your model, the scripts prints the directories of model and index checkpoints as well as the run files for the validation and test data as below. You can load the model for other usage and evaluate the validation run file. See the notebook ```tutorial.ipynb``` for a sample code on these.


For example, this the output of the above train script:
```
Model is trained! and saved at:
--model: checkpoints/t1_s1-s2_toytest.model
--id_bank: checkpoints/t1_s1-s2_toytest.pickle
Run output files:
--validation: valid_t1_s1-s2_toytest.tsv
--test: test_t1_s1-s2_toytest.tsv
```
You can test your validation performance using the following code:

```python
from utils import read_qrel_file,get_evaluations_final

valid_run_mf = mymodel.predict(tgt_valid_dataloader)
tgt_valid_qrel = read_qrel_file('DATA/t1/valid_qrel.tsv')
task_ov, task_ind = get_evaluations_final(valid_run_mf, tgt_valid_qrel)
```

You will need to upload the test run output file (.tsv file format) for both target markets to Codalab for our evaluation and leaderboard entry. This output file contains ranked items for each user with their score. Our final evaluation metric is based on nDCG@10 on both target markets.


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

