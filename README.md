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

The next step would be to find out the xpath for the review text that lies in the website 
