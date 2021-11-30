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


1. **Ratings**


Provide a simple file format listing as `userId itemId rating date`. For this purpose, you can easily read each of these ratings with the following code. 


```python
import pandas as pd
cur_ratings = 'ratings_uk_Books.txt.gz' # specify the url to the file 
df = pd.read_csv(cur_ratings, compression='gzip', header=None, sep=' ', names=["userId", "itemId", "rate", "date"] )
```

After reading, you can see a dataframe similar to below (taken from uk market). 

```
+------------------------------+------------+--------+------------+
| userId                       |     itemId |   rate | date       |
|------------------------------+------------+--------+------------|
| AFXKARXBL3X3LO5GNN4XAXC7XMPQ | 0061806765 |      3 | 2012-08-20 |
| AH3Z5FFVAQYVQNDBUG2M23ZZBCGA | 0007273754 |      5 | 2013-11-24 |
| AHDFCI7XXY5J7GIDWWE2VSLPQMCQ | 1682307530 |      4 | 2016-06-29 |
| AE5KNHUMUIHXU3FCRBDMTTZT7UYA | 1611097843 |      5 | 2014-05-29 |
| AEEHVF74E2QB7UP7VG4LFSUNXVJA | 0142410586 |      4 | 2020-04-13 |
+------------------------------+------------+--------+------------+
```

where
- userId - ID of the reviewer  
- itemId - asin or the ID of the product, e.g. [B014RDFCFI](https://www.amazon.co.uk/dp/B014RDFCFI) 
- rate - rating of the product in the range of 1-5 
- date - date of the review in the format of YYYY-MM-DD


2. **Reviews**


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

```
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


3. **Metadata**


Metadata includes product descriptions, price, sales-rank, brand info, and co-purchasing links. For reading, similar to reviews, use the below python code snippet.   

```python
import gzip
example_met_file = 'metadata_us_Electronics.json.gz'
meta_lines = []
with gzip.open(example_met_file, 'rt', encoding='utf8') as f:
    meta_lines = f.readlines()

print( eval(meta_lines[80].strip()) )
```

Below is the output of the sample line of the review file we read above. 

```
 {'asin': 'B000023VW2',
 'title': 'Sangean ANT-60 Short Wave Antenna',
 'averageRating': '4.3',
 'ratingCount': '698 global ratings',
 'amazon_badge': 'Amazon\'s \nChoice for "shortwave antenna"',
 'ratingDist': {'5': '67%', '4': '14%', '3': '10%', '2': '3%', '1': '6%'},
 'ratingByFeature': {},
 'price': '$12.86',
 'imgUrl': '{"https://images-na.ssl-images-amazon.com/images/I/71-XmouTyhL._AC_SX569_.jpg":[369,569],"https://images-na.ssl-images-amazon.com/images/I/71-XmouTyhL._AC_SX355_.jpg":[230,355],"https://images-na.ssl-images-amazon.com/images/I/71-XmouTyhL._AC_SX425_.jpg":[276,425],"https://images-na.ssl-images-amazon.com/images/I/71-XmouTyhL._AC_SX450_.jpg":[292,450],"https://images-na.ssl-images-amazon.com/images/I/71-XmouTyhL._AC_SX466_.jpg":[303,466],"https://images-na.ssl-images-amazon.com/images/I/71-XmouTyhL._AC_SX522_.jpg":[339,522],"https://images-na.ssl-images-amazon.com/images/I/71-XmouTyhL._AC_SX679_.jpg":[441,679]}',
 'related': {
	 'sponsored': ['B000NOSCN0', 'B07NRSWB6Q', 'B00008N9M7', 'B0823N6DWV', 'B07NRNVCTV', 'B00QXJMZRS', 'B07SXF87LJ', 'B07P8BW3P2', 'B07YN62TM1', 'B07CNMDTY8', 'B000NOSCN0', 'B00R6VXD6Y', 'B074QLLVC6', 'B00008N9M7', 'B00QXJMZRS', 'B07R284TN5','B01N3POQ92', 'B07P8BW3P2', 'B07R3QG1CR','B000O8SQNG'],
	  'alsoBought': [],
	  'alsoViewed': ['B0104MT5HW', 'B00GJ51NVA', 'B08464KCG1', 'B0055Q5FIQ', 'B082B4C7CC', 'B001KC579Q', 'B0141X3B5W', 'B00IDM4N5K', 'B07T6LK1X2', 'B004H912FC', 'B08HCYYW88', 'B07T2FQH6N', 'B0104J57GS', 'B0014T7W8Y', 'B001HX4D84', 'B004QJKO52', 'B01ARN28SQ', 'B07T1F6LDY', '1999830024', 'B00X15M5MC', 'B08BLF7QDV', 'B005OEA88Q', 'B0035X1EC2', 'B01MSMRASH', 'B078JKXCYH', 'B085ZSXF8Z', 'B009ENG6TI','B00BLW627G', 'B001P4LTAU'],
	  'boughtTogether': ['B000023VW2', 'B005KVJYTW', 'B004H912FC'],
	  'compared': ['B000023VW2', 'B01I27ZOQM','B082B4C7CC', 'B001PNNXGO', 'B001KC579Q', 'B0055Q5FIQ']
  },
 'productDetails': {
      'Product Dimensions': '5.98 x 8.27 x 0.94 inches',
      'Item Weight': '1.41 ounces',
      'Manufacturer': 'Sangean',
      'ASIN': 'B000023VW2',
      'Item model number': 'ANT-60',
      'Customer Reviews': ['/*',
	   '* Fix for UDP-1061. Average customer reviews has a small extra line on hover',
	   '* https://omni-grok.amazon.com/xref/src/appgroup/websiteTemplates/retail/SoftlinesDetailPageAssets/udp-intl-lock/src/legacy.css?indexName=WebsiteTemplates#40'],
  	'Best Sellers Rank': '#13 in Radio Antennas',
  	'Is Discontinued By Manufacturer': 'No',
  	'Date First Available': 'October 15, 1999'},
	 'sellerPage': '/stores/SANGEAN/page/E550CD6C-93E0-447F-B3F0-A06E71DF322F?ref_=ast_bln',
	 'categories': ['Electronics',
	  'Accessories & Supplies',
	  'Audio & Video Accessories',
	  'Antennas',
	  'Radio Antennas'],
 	'description': "From the Manufacturer\nShortwave radios brought the world together long before the Internet, and now the Sangean ANT-60 antenna helps maximize your shortwave experience. Ideally suited for today's compact shortwave receiver and fully portable inside its convenient, compact carrying case, the ANT-60 extends to a full 23 feet and enhances the performance and reception of your shortwave radio beyond that of a typical built-in telescoping antenna. An adapter plug is provided for those receivers lacking a standard 1/8-inch (3.5mm) miniplug.",
 	'overviewFeatures': {},
 	'features': ['Improves the performance and reception of your shortwave radio',
	  'Extends to 23 feet and can be easily rewound into its compact case',
	  'Has 3.5-millimeter mini plug',
	  'Fits any 3.5-millimeter external antenna jack',
	  'Includes adapter plug'],
	'reviewFilters': []}
```

Where

- asin - ID of the product, e.g. see [B000023VW2](https://www.amazon.com/dp/B000023VW2)
- title - name of the product
- averageRating - the average rate of the product in the time of obtaining the data, float number \[1-5\]
- ratingCount - how many users rate this product
- amazon_badge - if there is any badge associated with this product
- ratingDist - the distribution of each rating value 
- related - related products (also bought, also viewed, bought together, compared, and sponsored) are listed
- productDetails - a variety of information related to the product, including brand, category, descriptions, features, etc. are all provided in this dictionary. See the product page for further information on each of these fields provided with our data. 




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
FOREC's data cleaning and code are provided in [this repository](https://github.com/hamedrab/FOREC).
