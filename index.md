# Cross-Market Recommendation (XMRec)

## Events

<span style="color:red">* Ongoing! * </span>
**[Cross-Market Recommendation Competition @ WSDM 2022](https://xmrec.github.io//wsdmcup)**

**[Cross-Market Product Recommendation @ CIKM 2021](https://dl.acm.org/doi/abs/10.1145/3459637.3482493)**

**[Workshop of Cross-Market Recommendation @ RecSys 2021](https://xmrec.github.io//recsys)**


## XMarket Dataset

### Description
Here, we release XMarket, a large dataset covering 18 local markets on 16 different product categories, featuring 52.5 million user-item interactions. For more information on the XMarket dataset, please refer to our [CIKM’21 paper](https://arxiv.org/pdf/2109.05929.pdf). 

### Files
Below is the list of our markets and their data. For every market below, you can click and see the list of categories as well as #user, #item, and #ratings we collected. For every market, you can download the `ratings`, `reviews`, and `metadata` associated with asins in this category. Please see below for how to read each type of these files and their file formats. Please reach out if you encounter any problem with these files provided below. The total size of gzipped data is ~7.5 GB.

- [United Arab Emirates (ae)](https://xmrec.github.io/data/ae)
- [Australia (au)](https://xmrec.github.io/data/au)
- [Brazil (br)](https://xmrec.github.io/data/br)
- [Canada (ca)](https://xmrec.github.io/data/ca)
- [China (cn)](https://xmrec.github.io/data/cn)
- [Germany (de)](https://xmrec.github.io/data/de)
- [Spain (es)](https://xmrec.github.io/data/es)
- [France (fr)](https://xmrec.github.io/data/fr)
- [India (in)](https://xmrec.github.io/data/in)
- [Italy (it)](https://xmrec.github.io/data/it)
- [Japan (jp)](https://xmrec.github.io/data/jp)
- [Mexico (mx)](https://xmrec.github.io/data/mx)
- [Netherlands (nl)](https://xmrec.github.io/data/nl)
- [Saudi Arabia (sa)](https://xmrec.github.io/data/sa)
- [Singapore (sg)](https://xmrec.github.io/data/sg)
- [Turkey (tr)](https://xmrec.github.io/data/tr)
- [United Kingdom (uk)](https://xmrec.github.io/data/uk)
- [United States (us)](https://xmrec.github.io/data/us)


### Data Samples (and python reading code examples)


1. ** Ratings **
Provide a simple file format listing as `userId itemId rating date`. For this purpose, you can easily read each of these ratings with the following code. 


```python
import pandas as pd
cur_ratings = 'ratings_uk_Books.txt.gz' # specify the url to the file 
df = pd.read_csv(cur_ratings, compression='gzip', header=None, sep=' ', quotechar='"', names=["userId", "itemId", "rate", "date"] )
```

After reading, you can see a dataframe similar to below (taken from uk market). 

+------------------------------+------------+--------+------------+
| userId                       |     itemId |   rate | date       |
|------------------------------+------------+--------+------------|
| AFXKARXBL3X3LO5GNN4XAXC7XMPQ | 0061806765 |      3 | 2012-08-20 |
| AH3Z5FFVAQYVQNDBUG2M23ZZBCGA | 0007273754 |      5 | 2013-11-24 |
| AHDFCI7XXY5J7GIDWWE2VSLPQMCQ | 1682307530 |      4 | 2016-06-29 |
| AE5KNHUMUIHXU3FCRBDMTTZT7UYA | 1611097843 |      5 | 2014-05-29 |
| AEEHVF74E2QB7UP7VG4LFSUNXVJA | 0142410586 |      4 | 2020-04-13 |
+------------------------------+------------+--------+------------+

where
- userId - ID of the reviewer  
- itemId - asin or the ID of the product, e.g. [B014RDFCFI](https://www.amazon.co.uk/dp/B014RDFCFI) 
- rate - rating of the product in the range of 1-5 
- date - date of the review in the format of YYYY-MM-DD


2. ** Reviews **
Review files provide a list of json objects, each providing a customer review for a given product. For reading these files you can read line by line and obtain the json dictionary of a specific review as below. 

```python
import gzip
example_rev_file = 'reviews_uk_Books.json.gz'
review_lines = []
with gzip.open(example_rev_file, 'rt', encoding='utf8') as f:
    review_lines = f.readlines()
    
print( eval(review_lines[1].strip())[0] )
```
Below is the output of the sample line of the review file we read above. 

```json
{
'reviewerID': 'AGIBLPPKMJ7NUQ2MCD3RW2OQXUAQ',
 'asin': '0001047868',
 'reviewerName': 'christine mcgill',
 'reviewText': 'I thourghly enjoyed this classic, read it when I was ten so was greatly surprised that it seemed even better than I remembered.',
 'overall': 5.0,
 'summary': 'I thourghly enjoyed this classic',
 'cleanReviewTime': '2015-09-29',
 'reviewTime': '29 September 2015'
}
```
where
- reviewerID - ID of the reviewer equivalent to userId of ratings 
- asin - ID of the product equivalent to the itemId of ratings, e.g. [B014RDFCFI](https://www.amazon.co.uk/dp/B014RDFCFI)  
- reviewerName - name of the reviewer
- reviewText - text of the review
- overall - rating of the product in the range of 1-5 
- summary - summary of the review
- cleanReviewTime - date of the review in the format of YYYY-MM-DD
- reviewTime - original review time posted along with the review in the local market calendar


3. ** Metadata **
Metadata includes product descriptions, price, sales-rank, brand info, and co-purchasing links.  

```python
import gzip
example_met_file = 'DATA_1/uk/Books/metadata_uk_Books.json.gz'
meta_lines = []
with gzip.open(example_met_file, 'rt', encoding='utf8') as f:
    meta_lines = f.readlines()

print( eval(meta_lines[0].strip()) )
```


```json
 {
 'asin': '0001050230',
 'title': 'Love’s Labours Lost: Performed by Derek Jacobi, Geraldine McEwan & Cast',
 'averageRating': '4.1',
 'ratingCount': '27 global ratings',
 'amazon_badge': '',
 'ratingDist': {'5': '59%', '4': '12%', '3': '14%', '2': '9%', '1': '6%'},
 'ratingByFeature': {},
 'price': '',
 'imgUrl': [],
 'related': {'sponsored': [],
  'alsoBought': ['1903436958', '0199536813',  '1904271014',  '1903436850', '1903436257', '9389193397'],
  'alsoViewed': ['0199536813',  '1903436958', '1853260304',  '1903436990',  '0199535906',  '1903436214',  '0199535914',  '0141396431',  '0198328729',  '1904271081', '9389193397', '147257754X'],
  'boughtTogether': [],
  'compared': []},
 'productDetails': {},
 'sellerPage': '',
 'categories': ['Fiction', 'Classics'],
 'description': '',
 'overviewFeatures': {},
 'features': [],
 'reviewFilters': []
 }
```

Where

- asin - ID of the product, e.g. 0000031852
- title - name of the product
- price - price in US dollars (at time of crawl)
- imUrl - url of the product image
- related - related products (also bought, also viewed, bought together, buy after viewing)
- salesRank - sales rank information
- brand - brand name
- categories - list of categories the product belongs to






### Citation
If you use this dataset, please refer to our [CIKM’21 paper](https://arxiv.org/pdf/2109.05929.pdf):
```
@inproceedings{bonab2021crossmarket,
	author = {Bonab, Hamed and Aliannejadi, Mohammad and Vardasbi, Ali and Kanoulas, Evangelos and Allan, James},
	booktitle = {Proceedings of the 30th ACM International Conference on Information \& Knowledge Management},
	publisher = {ACM},
	title = {Cross-Market Product Recommendation},
	year = {2021}}
```


### Code
