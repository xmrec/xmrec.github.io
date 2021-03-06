# WSDM Cup on Cross-Market Recommendation Competition

## Overview
E-commerce companies often operate across markets; for instance [Amazon](https://www.amazon.com)
has expanded their operations and sales to 18 markets (i.e. countries) around the globe. 
The cross-market recommendation concerns the problem of recommending relevant products to users in a target market (e.g., a resource-scarce market) by leveraging data from similar high-resource markets, e.g. using data from the U.S. market to improve recommendations in a target market. 
The key challenge, however, is that data, such as user interaction data with products (clicks, purchases, reviews), convey certain biases of the individual markets. 
Therefore, the algorithms trained on a source market are not necessarily effective in a different target market.
Despite its significance, small progress has been made in cross-market recommendation, mainly due to a lack of experimental data for the researchers. In this WSDM Cup challenge, we provide user purchase and rating data on various markets with a considerable number of shared item subsets. The goal is to improve individual recommendation systems in these target markets by leveraging data from similar auxiliary markets.


The competition was held on [Codalab](https://competitions.codalab.org/competitions/36050).

[Starter kit repository](https://github.com/hamedrab/wsdm22_cup_xmrec)

## Leaderboard


|#|Corresponding user|Team|T1+T2 nDCG@10|T1+T2 HR@10|T1 nDCG@10|T2 nDCG@10|
|--- |--- |--- |--- |--- |--- |--- |
|1|Zhang Qi|young_simple|0.6773 (1)|0.7869 (1)|0.7384 (3)|0.6472 (1)|
|2|Zeyuan Chen|fish_|0.6765 (2)|0.7860 (2)|0.7393 (2)|0.6457 (2)|
|3|Peng Zhang|biubiubiu|0.6746 (3)|0.7800 (4)|0.7395 (1)|0.6427 (4)|
|4|Cesare Bernardis|G13Destroyer|0.6735 (4)|0.7841 (3)|0.7354 (4)|0.6430 (3)|


## Task
### Problem Definition

Given a global set of items I, 
we define a market M as the collection of its users U(M) together with their interactions (i.e. ratings) with items from I.
Generally, a user can interact with different markets, but for simplicity, we assume that the set of users in each market are mutually disjoint with any other parallel market.
In this competition, we have three source markets, s1, s2, and s3, and two target markets, t1 and t2. 

The goal is to have the best possible recommender system in terms of nDCG@10 on the target markets t1 and t2.
For that, you can use the data on these markets and also get help from the data available from the source markets s1, s2, and s3.


### Train, Validation, and Test Split
The source markets are used for training, and therefore they are provided in a single **Train** split.
For the target markets, we leave one interaction of each user out for the **Test** split, and one other interaction for the **Validation** split. All the remaining interactions are given as the **Train** split. As usual, you are given only the Train and Validation splits.


### Submissions

Your submissions should be a zip file, containing two folders *```t1```* and *```t2```*.
In each of these two target folders, there should be two tab separated files *```test_pred.tsv```* (the test scores, i.e. reranked items from *```test_run.tsv```*) and *```valid_pred.tsv```* (the validation scores, i.e. reranked items from *```valid_run.tsv```*) with each line containing three columns as follows:
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


&nbsp;submission.zip<br>&nbsp;&nbsp;
&#x251C;&#x2500;&#x2500; t1<br>&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&#x251C;&#x2500;&#x2500; test_pred.tsv&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;/\* scores of test items \*/<br>&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&#x2514;&#x2500;&#x2500; valid_pred.tsv&nbsp;&nbsp;&nbsp;&nbsp;/\* scores of validation items \*/<br>&nbsp;&nbsp;	
&#x251C;&#x2500;&#x2500; t2<br>&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&#x251C;&#x2500;&#x2500; test_pred.tsv&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;/\* scores of test items \*/<br>&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&#x2514;&#x2500;&#x2500; valid_pred.tsv&nbsp;&nbsp;&nbsp;&nbsp;/\* scores of validation items \*/<br>&nbsp;&nbsp;

We provide a validation script ```validate_submission.py``` that can be used to check your zip file before submission.
Simply run
```python
validate_submission.py path/to/submission.zip
```
on your final zip file to make sure the structure of your submission is OK.
If so you will get the following message:
```python
File structure validation successfully passed
```
## Evaluation

We evaluate the submissions based on their average nDCG@10.
As discussed in submission guidelines, the scores of items are sorted for each user and the top 10 items are considered for evaluation.
For the total evaluation, we concatenate the users of the target markets (*```t1/scores.tsv```* and *```t2/scores.tsv```*) and compute the nDCG@10 on the resulting list. The teams are ranked based on this metric.
<br>
For information purposes we also report separate nDCG@10 and HR@10 for each target market, too.

## Data
The training and validation as well as the test run are provided in the [starter kit repository](https://github.com/hamedrab/wsdm22_cup_xmrec).
The data is structured as follows:

&nbsp;DATA<br>&nbsp;&nbsp;
&#x251C;&#x2500;&#x2500; s1<br>&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&#x2514;&#x2500;&#x2500; [1.8M]&nbsp;&nbsp;train_5core.tsv&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;/\* train data \*/<br>&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&#x2514;&#x2500;&#x2500; [19.2M]&nbsp;&nbsp;train.tsv&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;/\* full train data \*/<br>&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&#x2514;&#x2500;&#x2500; [139K]&nbsp;&nbsp;valid_qrel.tsv&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;/\* validation positive samples \*/<br>&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&#x2514;&#x2500;&#x2500; [5.6M]&nbsp;&nbsp;valid_run.tsv&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;/\* list of validation items to be reranked \*/<br>&nbsp;&nbsp;
&#x251C;&#x2500;&#x2500; s2<br>&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&#x2514;&#x2500;&#x2500; [1.1M]&nbsp;&nbsp;train_5core.tsv&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;/\* train data \*/<br>&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&#x2514;&#x2500;&#x2500; [2.6M]&nbsp;&nbsp;train.tsv&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;/\* full train data \*/<br>&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&#x2514;&#x2500;&#x2500; [153K]&nbsp;&nbsp;valid_qrel.tsv&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;/\* validation positive samples \*/<br>&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&#x2514;&#x2500;&#x2500; [6.2M]&nbsp;&nbsp;valid_run.tsv&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;/\* list of validation items to be reranked \*/<br>&nbsp;&nbsp;
&#x251C;&#x2500;&#x2500; s3<br>&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&#x2514;&#x2500;&#x2500; [548K]&nbsp;&nbsp;train_5core.tsv&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;/\* train data \*/<br>&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&#x2514;&#x2500;&#x2500; [1.2M]&nbsp;&nbsp;train.tsv&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;/\* full train data \*/<br>&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&#x2514;&#x2500;&#x2500; [71K]&nbsp;&nbsp;valid_qrel.tsv&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;/\* validation positive samples \*/<br>&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&#x2514;&#x2500;&#x2500; [2.9M]&nbsp;&nbsp;valid_run.tsv&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;/\* list of validation items to be reranked \*/<br>&nbsp;&nbsp;
&#x251C;&#x2500;&#x2500; t1<br>&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&#x251C;&#x2500;&#x2500; [2.3M]&nbsp;&nbsp;test_run.tsv&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;/\* list of test items to be reranked \*/<br>&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&#x251C;&#x2500;&#x2500; [1.4M]&nbsp;&nbsp;train.tsv&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;/\* full train data \*/<br>&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&#x251C;&#x2500;&#x2500; [457K]&nbsp;&nbsp;train_5core.tsv&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;/\* train data \*/<br>&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&#x251C;&#x2500;&#x2500; [58K]&nbsp;&nbsp;valid_qrel.tsv&nbsp;&nbsp;&nbsp;/\* validation positive samples \*/<br>&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&#x2514;&#x2500;&#x2500; [2.3M]&nbsp;&nbsp;valid_run.tsv&nbsp;&nbsp;&nbsp;&nbsp;/\* list of validation items to be reranked \*/<br>&nbsp;&nbsp;
&#x2514;&#x2500;&#x2500; t2<br>&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp; &#x251C;&#x2500;&#x2500; [4.8M]&nbsp;&nbsp;test_run.tsv&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;/\* list of test items to be reranked \*/<br>&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp; &#x251C;&#x2500;&#x2500; [2.8M]&nbsp;&nbsp;train.tsv&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;/\* full train data \*/<br>&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp; &#x251C;&#x2500;&#x2500; [966K]&nbsp;&nbsp;train_5core.tsv&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;/\* train data \*/<br>&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp; &#x251C;&#x2500;&#x2500; [118K]&nbsp;&nbsp;valid_qrel.tsv&nbsp;&nbsp;&nbsp;/\* validation positive samples \*/<br>&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp; &#x2514;&#x2500;&#x2500; [4.8M]&nbsp;&nbsp;valid_run.tsv&nbsp;&nbsp;&nbsp;&nbsp;/\* list of validation items to be reranked \*/<br>&nbsp;&nbsp;
<br>&nbsp;&nbsp;<br>


- There are three folders 
        *```s1```*,
        *```s2```*, and 
        *```s3```*;
        containing the data of the source markets. Inside each, the following files can be found:
    - *```train.tsv```* and *```train_5core.tsv```*: A tab separated file, containing the **Training** data with the following format:
                ```
                    userId	itemId	rating
                ```
                where the first column ```userId``` is the user unique id, the second column ```itemId``` is the item unique id, and the third column ```rating``` is the rating (an integer ranging from 1 to 5). We conduct some pre-processing steps and provide 5core versions for training data for further facilitate the data preprocessing burden. For this step, we first normalize ratings into 0 and 1 and filtered users with 5 ratings, and items with 5 ratings from their corresponding train.tsv file. Note that you might see some descripincies between these training files. It is due to a few other steps that is related to our data generation and we are not able to reveal those. 
                This means that our 5core training data only contains the **positive** samples. All the other (user, item) pairs are unknown and can be considered **negative** during training. We provide valid_qrel.tsv and valid_run.tsv for source markets for any solution that might need evalutions on source markets. 

- There are two folders
        *```t1```*, and 
        *```t2```*; containing the data of the target markets. Inside each, the following files can be found:
    - *```train.tsv```* and *```train_5core.tsv```*: 
                Similar to source markets, provide tab separated files, containing the **Training** data with ```userId```, ```itemId```, and ```rating``` fields.
    - *```valid_qrel.tsv```*:
                The **Validation positive** samples, with a structure similar to the *```train.tsv```*.
                Note that the validation set only has one positive sample per user.
    - *```valid_run.tsv```*:
                The **Validation** samples. For consistency of results between different teams, we provide you 99 negative samples for each unique ```userId``` as follows:
                ```
                    userId	itemId1,itemId2,...,itemId100
                ```
                where the two columns are separated by a tab and the list of items are separated by commas. There are 99 negative samples and 1 positive sample (identified in the *```valid_qrel.tsv```* file) in the list. Your model should rerank these 100 items per user.
    - *```test_run.tsv```*:
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


## Sponsors
Silver sponsors:

<img src="https://xmrec.github.io//wsdmcup//sponsor//Spotify_Logo_RGB_Green.png" width="30%" title="Spotify"/> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp; <img src="https://xmrec.github.io//wsdmcup//sponsor//new-google-logo-png.png" width="30%" title="Google"/> 



## Sponsorship
The XMRec WSDM Cup looks for a wide range of financial supports, ranging from the support of top teams award to computational power. Please check the [Call for Sponsorship](https://xmrec.github.io//wsdmcup//sponsor//sponsorship.pdf) for more information.
        
## Terms and Conditions
The XMRec dataset is free to download for research purposes.
Each team is allowed to submit two runs per day and a maximum of 100 runs in total.

Notice that we ask the top teams to provide the code to their models to run them locally after the end of the competition.

At the end of the challenge, each team is encouraged to open source the source code that was used to generate their final challenge solution under the MIT license. To be eligible for the leaderboard or prizes, winning teams are also required to submit papers describing their method to the WSDM Cup Workshop, and present their work at the workshop. Refer to the "Call for Papers" section on the WSDM Cup 2022 webpage for more details.

## Timeline

- October 18, 2021: Online registration is open. **Training** and **Validation** dataset released.
- December 20, 2021: Deadline for online registration.
- January 17, 2022: Deadline for results **submission**.
- February 1, 2022: **Announcement** of results.
- February 7, 2022: Deadline for supporting materials (codes, ...)
- February 14, 2022: Deadline for documents.
- February 21, 2022: Award and Presentation from the top 3 teams
 

## Prizes

- 1st-place: One team ($2000) 

- 2nd-place: One team ($1000) 

- 3rd-place: One team ($500)


## Organizers
 - [Mohammad Aliannejadi](http://aliannejadi.com), University of Amsterdam, The Netherlands
 - [Hamed Bonab](https://people.cs.umass.edu/~bonab/), University of Massachusetts Amherst, USA
 - Ali Vardasbi, University of Amsterdam, The Netherlands
 - [Evangelos Kanoulas](https://staff.fnwi.uva.nl/e.kanoulas/), University of Amsterdam, The Netherlands
 - [James Allan](http://ciir.cs.umass.edu/~allan/), University of Massachusetts Amherst, USA

## Contact
For questions and announcements follow us on [twitter](https://twitter.com/XMarketRec) and join our [google groups](https://groups.google.com/u/1/g/xmrec).

You can also contact us via [m.aliannejadi@uva.nl](mailto:m.aliannejadi@uva.nl).

