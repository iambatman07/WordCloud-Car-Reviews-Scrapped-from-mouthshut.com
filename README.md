### This goal behind this project is to create WordCloud of the most occuring words from the list car reviews that we are going to scrap from mouthshut.com


First we need the start- url's for scrapping. 
```
path = ['https://www.mouthshut.com/cars-suvs/Nissan-Terrano-XL-Plus-ICC-WT20-SE-reviews-925815477-srch']
for i in range (2,7):
    st= 'https://www.mouthshut.com/cars-suvs/Nissan-Terrano-XL-Plus-ICC-WT20-SE-reviews-925815477-page-'+str(i)
    path.append(st)
    print (st)
```
https://www.mouthshut.com/cars-suvs/Nissan-Terrano-XL-Plus-ICC-WT20-SE-reviews-925815477-srch
https://www.mouthshut.com/cars-suvs/Nissan-Terrano-XL-Plus-ICC-WT20-SE-reviews-925815477-page-2
https://www.mouthshut.com/cars-suvs/Nissan-Terrano-XL-Plus-ICC-WT20-SE-reviews-925815477-page-3
https://www.mouthshut.com/cars-suvs/Nissan-Terrano-XL-Plus-ICC-WT20-SE-reviews-925815477-page-4
https://www.mouthshut.com/cars-suvs/Nissan-Terrano-XL-Plus-ICC-WT20-SE-reviews-925815477-page-5
https://www.mouthshut.com/cars-suvs/Nissan-Terrano-XL-Plus-ICC-WT20-SE-reviews-925815477-page-6

are the start-url's for NISSAN TERRANO reviews 

### Step 1:
Start your Scrapy shell by
```
scrapy shell 'https://www.mouthshut.com/cars-suvs/Nissan-Terrano-XL-Plus-ICC-WT20-SE-reviews-925815477-srch'
```
### Step 2:
The next step would be to find out the xpath for the review text that lies in the website 

![inspectelement](https://user-images.githubusercontent.com/33875100/45506121-facf9980-b7ab-11e8-86d8-e8140e13bc3a.PNG)

the xpath for our set of reviews is 

```
response.xpath('//*[@id="totcont"]/*[@class="container box-module ofinherit"]/*[@class="custom-body"]/*[@class="wrapper"]/*[@class="left-panel"]/*[@class="read-review-holder"]/*[@class="row review-article"]/*[@class="row"]/*[@class="col-10 review"]/*[@class="more reviewdata"]//text()').extract()
```

The output for this would be a list of string with all the 20 reviews in the page along with a few dummy text.Before writing  this onto a CSV file we have to handle all the unwanted texts in the list. 

![xpathresponse](https://user-images.githubusercontent.com/33875100/45506682-a62d1e00-b7ad-11e8-8046-9d81d1db6537.PNG)

### Step 3:
Handling the list includes removing the 'Read More' elements along with any other unwanted strings present. 
This can be done by :
```
t = []
for  i in response.xpath('//*[@id="totcont"]/*[@class="container box-module ofinherit"]/*[@class="custom-body"]/*[@class="wrapper"]/*[@class="left-panel"]/*[@class="read-review-holder"]/*[@class="row revew-article"]/*[@class="row"]/*[@class="col-10 review"]/*[@class="more reviewdata"]//text()'):
    t.append(i.extract())

for i in range(len(t)):
    t.remove('Read More')
```

### Step 4:
The next step would be to fetch the corresponding rating in stars that they have given for their review. 
```
response.xpath('//*[@id="totcont"]/*[@class="container box-module ofinherit"]/*[@class="custom-body"]/*[@class="wrapper"]/*[@class="left-panel"]/*[@class="read-review-holder"]/*[@class="row review-article"]/*[@class="row"]/*[@class="col-10 review"]/*[@class="rating"]//span//i[@class="icon-rating unrated-star"]|//i[@class="icon-rating rated-star"]').extract()
```
Each star is counted as an entity so there would be 100 rows for 20 reviews. 

![lenofstars](https://user-images.githubusercontent.com/33875100/45507155-e771fd80-b7ae-11e8-881b-89528c3b6359.PNG)

We need to convert this list onto a readable list of integers indicating the rating of each review 

![ratings](https://user-images.githubusercontent.com/33875100/45507223-18523280-b7af-11e8-8d86-a9b96babd6e2.PNG)
