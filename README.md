### This goal behind this project is to create WordCloud of the most occuring words from the list car reviews that we are going to scrap from mouthshut.com


First we need the start- url's for scrapping. 
```python
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
```python
scrapy shell 'https://www.mouthshut.com/cars-suvs/Nissan-Terrano-XL-Plus-ICC-WT20-SE-reviews-925815477-srch'
```
### Step 2:
The next step would be to find out the xpath for the review text that lies in the website 

![inspectelement](https://user-images.githubusercontent.com/33875100/45506121-facf9980-b7ab-11e8-86d8-e8140e13bc3a.PNG)

the xpath for our set of reviews is 

```python
response.xpath('//*[@id="totcont"]/*[@class="container box-module ofinherit"]/*[@class="custom-body"]/*[@class="wrapper"]/*[@class="left-panel"]/*[@class="read-review-holder"]/*[@class="row review-article"]/*[@class="row"]/*[@class="col-10 review"]/*[@class="more reviewdata"]//text()').extract()
```

The output for this would be a list of string with all the 20 reviews in the page along with a few dummy text.Before writing  this onto a CSV file we have to handle all the unwanted texts in the list. 

![xpathresponse](https://user-images.githubusercontent.com/33875100/45506682-a62d1e00-b7ad-11e8-8046-9d81d1db6537.PNG)

### Step 3:
Handling the list includes removing the 'Read More' elements along with any other unwanted strings present. 
This can be done by :
```python
t = []
for  i in response.xpath('//*[@id="totcont"]/*[@class="container box-module ofinherit"]/*[@class="custom-body"]/*[@class="wrapper"]/*[@class="left-panel"]/*[@class="read-review-holder"]/*[@class="row revew-article"]/*[@class="row"]/*[@class="col-10 review"]/*[@class="more reviewdata"]//text()'):
    t.append(i.extract())

for i in range(len(t)):
    t.remove('Read More')
```

### Step 4:
The next step would be to fetch the corresponding rating in stars that they have given for their review. 
```python
response.xpath('//*[@id="totcont"]/*[@class="container box-module ofinherit"]/*[@class="custom-body"]/*[@class="wrapper"]/*[@class="left-panel"]/*[@class="read-review-holder"]/*[@class="row review-article"]/*[@class="row"]/*[@class="col-10 review"]/*[@class="rating"]//span//i[@class="icon-rating unrated-star"]|//i[@class="icon-rating rated-star"]').extract()
```
Each star is counted as an entity so there would be 100 rows for 20 reviews. 

![lenofstars](https://user-images.githubusercontent.com/33875100/45507155-e771fd80-b7ae-11e8-881b-89528c3b6359.PNG)

We need to convert this list onto a readable list of integers indicating the rating of each review 

![ratings](https://user-images.githubusercontent.com/33875100/45507223-18523280-b7af-11e8-8d86-a9b96babd6e2.PNG)

Code :
```python
t =[]
for  i in range(0,len(q),5):
    t.append(''.join(q[i:i+5]))

for i in range(0,20):
    t[i] = int(5-t[i].count('unrated'))
```

### Step 5:
Write it to a dataframe and sort it to fetch the reviews that fall in the low end of the spectrum (rating < 3 )

load those reviews to a list 
![loadedstring](https://user-images.githubusercontent.com/33875100/45507545-06bd5a80-b7b0-11e8-8756-e8441596eeeb.PNG)

Preprocess it which means getting rid of common and stopwords and tokenizing them to find out the most occuring words 
![stopwords](https://user-images.githubusercontent.com/33875100/45507712-82b7a280-b7b0-11e8-93f5-803b2d9e2b45.PNG)

Now with the set of these we can go ahead and create the WordCloud for Nissan-Terrano's Reviews(the low ratings). 

```python
mask = np.array(Image.open('D:/imafe.png'))
wc = WordCloud(background_color="white", max_words=1000, mask=mask,
               max_font_size=90, random_state=42)
wc.generate(string)

image_colors = ImageColorGenerator(mask)
plt.figure(figsize=[7,7])
plt.imshow(wc.recolor(color_func=image_colors), interpolation="bilinear")
plt.axis("off")
plt.show()
```

which will produce the WordCloud 

![hondacity-lowreviews](https://user-images.githubusercontent.com/33875100/45507921-feb1ea80-b7b0-11e8-9044-b0e879076011.PNG)
