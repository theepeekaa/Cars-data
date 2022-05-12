OMIS-665 Big Data Analytics for Business
Group 5 - Part 1 and Part 2
May 08,2022
Biagio Palese , Professor
                          
                         




   Submitted by:                           
                            Theepeekaa Ramakrishnan Shanthi - Z1911800



—Cheesy Cars part 1 and part-2 —
08 May, 2022
Introduction
DataSource: https://www.kaggle.com/ananaymital/us-used-cars-dataset
From the Kaggle website, we took a data set that has the data regarding used and new cars with their details from all over the USA. This data set contains the details of car specifications along with the price and where they sold it. From the 3 million rows, we took a sample of 500,000 rows to identify a research question and use the interpretations.We choose this dataset because of our fantasy towards cars. Our team is considering to open a car dealership. Using RStudio and sparkly to filter the dataset and understand what determines the price of a car?. Will it help while opening a Dealer service?Are those linked with the price? Who is the leading company, and who are their competitors? We have done a few manipulations on the dataset, like adding new variables with numerical values, scaling the visualization, and using geom repel to make visualizations better.
Checking what brands have sold more cars
car_small %>% filter(!is.na(highway_fuel_economy),!is.na(make_name),!is.na(body_type),!is.na(city_fuel_economy),!is.na(dealer_zip)) %>% group_by(make_name) %>% summarize(number_of_cars_sold = n()) %>% arrange(desc(number_of_cars_sold))
## # A tibble: 58 x 2
##    make_name number_of_cars_sold
##    <chr>                   <int>
##  1 Ford                    64029
##  2 Chevrolet               47579
##  3 Toyota                  35039
##  4 Honda                   34061
##  5 Nissan                  33703
##  6 Jeep                    25039
##  7 Hyundai                 18679
##  8 Kia                     14936
##  9 Dodge                   14015
## 10 GMC                     12726
## # ... with 48 more rows
From the observation we picked the top 7 brands to check why they have more sales than the other companies. The top 7 brands are Ford, Chevrolet, Toyota, Honda, Nissan, Jeep, and Hyundai. Ford occupies the first place by selling 64029 cars compared to other car makes. So we are checking whether those brands have more sales every year or not?
car_small %>% filter(!is.na(highway_fuel_economy),!is.na(make_name),!is.na(body_type),!is.na(city_fuel_economy),!is.na(dealer_zip), make_name == "Ford"| make_name == "Chevrolet"| make_name == "Toyota" | make_name == "Honda" | make_name == "Nissan"| make_name == "Jeep"|make_name == "Hyundai", year >2015 & year <2021) %>%group_by(make_name,year) %>% summarise(number_of_cars_sold = n())
## # A tibble: 35 x 3
## # Groups:   make_name [7]
##    make_name  year number_of_cars_sold
##    <chr>     <dbl>               <int>
##  1 Chevrolet  2016                1961
##  2 Chevrolet  2017                5201
##  3 Chevrolet  2018                3439
##  4 Chevrolet  2019                3437
##  5 Chevrolet  2020               22797
##  6 Ford       2016                2515
##  7 Ford       2017                6959
##  8 Ford       2018                4248
##  9 Ford       2019                5579
## 10 Ford       2020               35853
## # ... with 25 more rows
The observation is kept to the particular brands and finding the sales range from the year 2016 to 2020. And we saw that For the Ford company, we can see that there is a significant change in sales in the year 2020. It has raised around 640%. For the Chevrolet company, we can see that it has a change in their sales in the year 2020. It has raised 660%. For the Toyota company, we can see a huge change in their sales in the year 2020. It has risen to 380%. For the Honda company, we can see a change in sales in the year 2020. It has a huge raise of 900% For the Nissan company, we can see that there is also a change in sales year 2020, and that is of 300% For the Jeep company, we can see that there is a change in sales in the year 2020, and the rise is of 540% For the Hyundai company, we can see that in sales in the year 2020, It has a huge rise of 400% We can say that for all the top brand companies, there is a change in the particular year 2020. ( From google research, there are many changes in transportation because of a pandemic, to maintain a social distancing, the rise of cars has increased.) So we are checking whether these sales are because of new cars/old cars.
car_small %>% filter(!is.na(highway_fuel_economy),!is.na(make_name),!is.na(body_type),!is.na(city_fuel_economy),!is.na(dealer_zip), make_name == "Ford"| make_name == "Chevrolet"| make_name == "Toyota" | make_name == "Honda" | make_name == "Nissan"| make_name == "Jeep"|make_name == "Hyundai", year >2015 & year <2021) %>%group_by(make_name,is_new) %>% summarise(number_of_cars_sold = n()) %>%  arrange(desc(number_of_cars_sold))
## # A tibble: 14 x 3
## # Groups:   make_name [7]
##    make_name is_new number_of_cars_sold
##    <chr>     <lgl>                <int>
##  1 Ford      TRUE                 35180
##  2 Chevrolet TRUE                 21361
##  3 Ford      FALSE                19974
##  4 Honda     TRUE                 16689
##  5 Nissan    TRUE                 15634
##  6 Chevrolet FALSE                15474
##  7 Nissan    FALSE                13785
##  8 Toyota    FALSE                13255
##  9 Toyota    TRUE                 13072
## 10 Jeep      TRUE                 12290
## 11 Honda     FALSE                 9238
## 12 Jeep      FALSE                 8632
## 13 Hyundai   TRUE                  7557
## 14 Hyundai   FALSE                 6378
From the observation, we can say that each car has more sales in New cars than the Used cars. Only the Toyota cars have more sales in used cars than new ones. Hence, We can say that Toyota is a more reliable company and can handle more miles than the others. As we are in Illinois state we are filtering the data to Illinois and checking what kind of fuel type are sold more
ggplot(data = (car_small %>% filter(dealer_zip<=70000&dealer_zip>=60000,!is.na(body_type), !is.na(fuel_type), make_name == "Ford"| make_name == "Chevrolet"| make_name == "Toyota" | make_name == "Honda" | make_name == "Nissan"| make_name == "Jeep"|make_name == "Hyundai")))+ geom_bar(mapping=aes(x=body_type,color = fuel_type, fill = fuel_type)) 
 
 We can see that the most sold cars are SUV / Crossovers and Sedan. Also, we can see that they both have the same fuel type - Gasoline.
checking to see what kind of cars are more sold based in their body type
ggplot(data = (car_small %>%filter(dealer_zip<=70000&dealer_zip>=60000,!is.na(body_type), !is.na(fuel_type),make_name == "Ford"| make_name == "Chevrolet"| make_name == "Toyota" | make_name == "Honda" | make_name == "Nissan"| make_name == "Jeep"|make_name == "Hyundai",body_type == "Sedan" | body_type == "SUV / Crossover", fuel_type == "Gasoline"))) + geom_bar(mapping = aes(x=make_name, color = body_type, fill = body_type))+facet_wrap(~body_type) 
  All the leading companies have their most sales in body type SUV / Crossover than the sedan. From the observation, Ford and Chevrolet are the top 2 brands, and they have maintained their position in the market, but Toyota has fewer sales in SUV / crossover than other companies. From the internet search, Honda has an excellent build quality in sedans, but they have very few sales in sedans compared to the other competitors. We can say that Jeep cars are only interested in Suv / Crossovers.
Now we are observing which brands offers more fuel economy
ggplot(data = (car_small %>% filter(dealer_zip<=70000&dealer_zip>=60000,!is.na(body_type), !is.na(fuel_type),make_name == "Ford"| make_name == "Chevrolet"| make_name == "Toyota" | make_name == "Honda" | make_name == "Nissan"| make_name == "Jeep"|make_name == "Hyundai",body_type == "Sedan" | body_type == "SUV / Crossover", fuel_type == "Gasoline") %>% mutate(avg_total_fuel_economy =(city_fuel_economy+highway_fuel_economy)/2)%>% group_by(make_name,body_type)%>% summarise(avg_mileage=mean(avg_total_fuel_economy,na.rm = T)) %>% arrange(desc(avg_mileage)))) + geom_col(mapping = aes(x= make_name,y = avg_mileage, fill = make_name)) + facet_wrap(~ body_type)
 
 From the observation, Honda, Hyundai, and Nissan cars have more mileage than others, but Honda cars still lack sales. Ford is the top sales car and still lacks mileage than other companies. Jeep is only interested in Suv / Crossovers, and they still lack mileage compared to the other competitors. Honda and Nissan are leading the way on average millage capacity cars. From these, we can say Sedan cars give a lot more mileage than the Suv/Crossovers. But comping both Suv/Crossovers have more sales. (any other parameters effecting the sales?)
From here we can check whether engine displacement is affecting the price or not.
ggplot(data= (car_small %>% filter(dealer_zip<=70000&dealer_zip>=60000, ,!is.na(body_type),!is.na(fuel_type),make_name == "Ford"| make_name == "Chevrolet"| make_name == "Toyota" | make_name == "Honda" | make_name == "Nissan"| make_name == "Jeep"|make_name == "Hyundai",body_type == "Sedan" | body_type == "SUV / Crossover", fuel_type == "Gasoline") %>% group_by(make_name,body_type) %>% summarise(avg_engine_displacement=mean(engine_displacement,na.rm=T)) %>% arrange(desc(avg_engine_displacement)))) + geom_col(mapping = aes(x=make_name,y=avg_engine_displacement, fill = make_name)) + facet_wrap(~body_type)
 
From this, we can say that every brand has more engine displacement( CC) than their sedan cars. It seems like Engine displacement matters from the sales and customer’s perspective. Jeep, being interested in Suv/Crossovers, are giving tough competition to the other top powerful car Toyota
As we can see that engine displacement is effecting the price and now checking whether engine cylinder is affecting the price or not.
ggplot(data= (car_small %>% filter(dealer_zip<=70000&dealer_zip>=60000, ,!is.na(body_type),!is.na(fuel_type),!is.na(engine_cylinders),make_name == "Ford"| make_name == "Chevrolet"| make_name == "Toyota" | make_name == "Honda" | make_name == "Nissan"| make_name == "Jeep"|make_name == "Hyundai",body_type == "Sedan" | body_type == "SUV / Crossover", fuel_type == "Gasoline") %>% group_by(make_name,engine_cylinders,body_type) %>% summarise(number_of_cars = n()) %>% arrange(desc(number_of_cars)))) + geom_col(mapping = aes(x=make_name,y=number_of_cars, fill = engine_cylinders)) +facet_wrap(~body_type)
 
We can see that the I4 engine is pretty common in sedans and Suv/Crossovers. But the engines V6, V8, and V10 are more powerful than I4. Suv/Crossovers are coming in all kinds of Engine sizes. V6 are the second-highest cylinders in Suv/Crossovers, and customers have a keen interest in powerful engines. It is also known that smaller engine cylinder vehicles give more mileage than the ones with Higher engine cylinders. From both graphs, it is clear that sales of Suv/Crossovers are high because of engine displacement and engine cylinders.
Keeping all these observations we are checking if suv have have more price than sedans
ggplot(data=(car_small %>% filter(dealer_zip<=70000&dealer_zip>=60000, ,!is.na(body_type),!is.na(fuel_type),make_name == "Ford"| make_name == "Chevrolet"| make_name == "Toyota" | make_name == "Honda" | make_name == "Nissan"| make_name == "Jeep"|make_name == "Hyundai",body_type == "Sedan" | body_type == "SUV / Crossover", fuel_type == "Gasoline") %>% group_by(make_name, body_type) %>% summarise(avg_price= mean(price, na.rm=TRUE))))+geom_col(mapping =aes(x=make_name, y=avg_price,color=make_name, fill=make_name))+facet_wrap(~body_type) 
 
As we can see, Ford and Toyota have the top prices in Suv/Crossovers. But, Honda is a bit more pricy in sedans than Ford and Toyota. From the graph, we can say that the price of Suv/Crossover cars is high than the Sedan cars. Engine cylinder and Engine displacement will make the car powerful. These powerful cars come at a high price compared to sedans.
Conclusion:
In the top 7 brands, we can see that the price factor varies for a few parameters. Among them, engine displacement and engine cylinder play a crucial role. Although Suv/Crossovers have high sales with powerful engines, they lack mileage capacity. Sedan cars have a lot of I4 engine cars compared to the other engines. And they have a good fuel economy compared to the Suv/Crossovers. Most of the cars sold in the market have Gasoline as their fuel type, which gives more options on engine power. Ford company stays on the top of the market - they have all body type cars and have good sales in both Suv/Crossovers and Sedan. Having all these, they have a high price in Suv/Crossovers.
Hyundai is one of the top brands, giving a high mileage on both sedan and Suv/Crossovers. They have less engine displacement compared to others in Suv/Crossovers and fewer options in the Engine cylinder. Overall the price of the car is less, and the sales are less compared to the others. By providing with all kinds of engine cylinders and powerful engine displacement, body type as Suv/Crossover will give more sales. From the customer’s perspective, a Sedan car gives more mileage than Suv/Crossovers.




                    







—Cheesy Cars-Part2–
08 May, 2022
Introduction
DataSource: https://www.kaggle.com/ananaymital/us-used-cars-dataset
From the Kaggle website, we took a data set that has the data regarding used and new cars with their details from all over the USA. This data set contains the details of car specifications along with the price and where they sold it. From the 3 million rows, we took a sample of 500,000 rows to identify a research question and use the interpretations.We choose this dataset because of our fantasy towards cars. Our team is considering to open a car dealership. Using RStudio and sparkly to filter the dataset and understand what determines the price of a car?. Will it help while opening a Dealer service? 
We have done a few manipulations on the dataset, like adding new variables with numerical values, scaling the visualization, and using geom repel to make visualizations better.
car_small %>% filter(!is.na(body_type),!is.na(fuel_type)) %>% group_by(make_name) %>% summarise(avg_price= mean(price, na.rm=TRUE)) %>%  arrange(desc(avg_price))
## # A tibble: 61 x 2
##    make_name    avg_price
##    <chr>            <dbl>
##  1 Ferrari        249501.
##  2 McLaren        238331.
##  3 Lamborghini    224014.
##  4 Rolls-Royce    214070.
##  5 Aston Martin   180866.
##  6 Karma          134798.
##  7 Bentley        122456.
##  8 SRT            102152 
##  9 Porsche         80352.
## 10 Lotus           79567.
## # ... with 51 more rows
As we can see, the top 5 cars are the supercars and have a price of more than 100K, so what are the variables affecting the price?. To check the similarities and distances between brands will use clustering techniques
fviz_dist(distance, gradient = list(low = "#00AFBB", mid = "white", high = "#FC4E07"))
 
We have manipulated the data set to remove null values and check the distances between them. These observations help to keep them in similar groups through clustering techniques. To make clustering groups, we need to scale the data and find the optimal clusters. So, checking the optimal cluster by three techniques would help us to identify best number of clusters.
set.seed(1234)
cars_cluster <- cars_cluster %>%select(engine_displacement,horsepower,price,engine_type_new,fuel_economy,fuel_tank_volume_new,maximum_seating_new) %>% scale()
fviz_nbclust(cars_cluster, kmeans, method = "wss")
 
fviz_nbclust(cars_cluster, kmeans, method = "silhouette")
 
gap_stat <- clusGap(cars_cluster, FUN = kmeans, nstart = 25,K.max = 10, B = 50)
fviz_gap_stat(gap_stat)
  Observing above graphs we can see optimal clusters for Elbow method (WSS) is 7, the silhouette is two, and the gap statistic is 3. Because there are three different optimal clusters, by checking the grid of all these clusters will help us find out which ones are close to the centroid and helps our reasearch.
grid.arrange(p1,p2,p6, nrow = 2)
  From the above grid, we can see in the graph where k = 2 and K = 3 that cluster 1’s group is so far from each other, and from the with-in sum of squares technique, we can say that k = 7 is optimal and the gaps between the brand are closer. from this graph, we can say k = 7 shows us the optimal clustered groups Using a repel geom we can see the brand names better than usual plot. 
K-Means Clustering
fviz_cluster(k7, data = cars_cluster,geom = "text", grom = "repel")
  From the above graph where k = 7, it’s clear that all the supercars, luxury cars, sports cars, and racing cars are from clusters 1 to 5. From a google search, we can say that they are pricy based on the horsepower and engine displacement. To support this statement, we will check the correlation between all the variables. From clusters 6 and 7, we can say that SUV cars with a large engine displacement are in cluster 7, and the sedan cars are in cluster 6. From this output, we know what brands belong to the same group. This output will help our research question in a way that it shows the brands belongs to the same cluster. We can use this output to choose the inventory. For example, placing a dealer service in a location where the community is high profile and has more spending limit than usual, then we choose the cars from luxury cluster where the cars are priced higher. And choosing a location near to colleges we will choose a cluster 6 or 7, which has both sedans and SUVs (Majority of students will look for a economy car when he visits the dealer service).  
Correlation
cars_corr <- carsmain_to_cluster %>% select(horsepower, engine_displacement, mileage, price,fuel_economy,engine_type_new,fuel_tank_volume_new,maximum_seating_new)%>% na.omit()
cars_corr %>% 
correlate( use = "pairwise.complete.obs", method = "pearson") %>% 
  rplot()
 
As seen in the distance matrix graph, the relationship between variables, but here we can see the correlation between the variables. This correlation helps us get a better understanding of what variables are correlated to each other and helps in understanding the strength of the relationship between each variable. The graph shows that the price is positively correlated with horsepower, engine type(cylinder), and engine displacement. We can also see that price is negatively correlated with mileage, fuel economy, and seating capacity. In the positive correlation, horsepower has a stronger bond with the price. Keeping these observations, checking how a cluster how a price and horsepower is related.
predicted1 %>%  group_by(prediction) %>%select(make_name,prediction) %>%  arrange(prediction)
## # A tibble: 57 x 2
## # Groups:   prediction [7]
##    make_name prediction
##    <chr>          <int>
##  1 Chrysler           0
##  2 Dodge              0
##  3 Eagle              0
##  4 FIAT               0
##  5 Honda              0
##  6 Hummer             0
##  7 Hyundai            0
##  8 Isuzu              0
##  9 Kia                0
## 10 Mazda              0
## # ... with 47 more rows
Using spark methods to predict the cluster, we can see that the cars from clusters 6 and 7 from the “k = 7 graph” have been placed in prediction number 0. To visualize them, we will use geom point
   
Now observing the above graph, it is clear that horse price does affect the price of the car, To support this prediction and to check the other variables that are affecting the price, we are applying Multiple regression techniques. #Multiple regression Now by performing Multiple regression to predict which independent variables predicts the price of the car.
Multiple Regression
set.seed(123)
model1 <- lm(price ~  horsepower+ engine_type_new + engine_displacement+ fuel_tank_volume_new+ maximum_seating_new+ fuel_economy , data = train)
summary(model1)
## 
## Call:
## lm(formula = price ~ horsepower + engine_type_new + engine_displacement + 
##     fuel_tank_volume_new + maximum_seating_new + fuel_economy, 
##     data = train)
## 
## Residuals:
##     Min      1Q  Median      3Q     Max 
##  -63822   -7091     179    6377 2161810 
## 
## Coefficients:
##                          Estimate   Std. Error t value             Pr(>|t|)    
## (Intercept)          -18308.63832    394.26025 -46.438 < 0.0000000000000002 ***
## horsepower              210.16846      0.69096 304.169 < 0.0000000000000002 ***
## engine_type_new         156.92904     69.36394   2.262               0.0237 *  
## engine_displacement      -6.13796      0.07902 -77.679 < 0.0000000000000002 ***
## fuel_tank_volume_new     69.63767     12.62788   5.515          0.000000035 ***
## maximum_seating_new      65.30609     29.69181   2.199               0.0278 *  
## fuel_economy            427.24819      7.61621  56.097 < 0.0000000000000002 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 14200 on 233949 degrees of freedom
## Multiple R-squared:  0.4111, Adjusted R-squared:  0.411 
## F-statistic: 2.721e+04 on 6 and 233949 DF,  p-value: < 0.00000000000000022
Running model 1 in R studio with the dependent variable as price and independent variables as horsepower, engine type, engine displacement, fuel tank volume, maximum seating, and fuel economy. We can see that the p-value of all independent variables is most significant compared to seating capacity and engine type. The estimates of each independent variable say, Every increase in horsepower, the mean price will be increased by 210.16 USD, and for every decrease in engine displacement, the mean price will be decreased by 6.13 USD. The RSE value of 14,200 indicates that the car’s actual price will deviate from the true regression line by 14,200 units. This model with six independent variables explains that the R square value is 41.11% of the variability in our car’s data. We can see that F- statistics is greater than one. Interpreting all these factors, we can say that it is a significant model.
model2 <- lm(price ~  horsepower * engine_displacement , data = train)
summary(model2)
## 
## Call:
## lm(formula = price ~ horsepower * engine_displacement, data = train)
## 
## Residuals:
##     Min      1Q  Median      3Q     Max 
##  -81558   -7055    -128    6235 2159907 
## 
## Coefficients:
##                                     Estimate    Std. Error t value
## (Intercept)                    16262.5565638   225.2833391   72.19
## horsepower                       139.9260465     0.9374348  149.26
## engine_displacement              -13.5091875     0.0935954 -144.34
## horsepower:engine_displacement     0.0217000     0.0002625   82.66
##                                           Pr(>|t|)    
## (Intercept)                    <0.0000000000000002 ***
## horsepower                     <0.0000000000000002 ***
## engine_displacement            <0.0000000000000002 ***
## horsepower:engine_displacement <0.0000000000000002 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 14100 on 233952 degrees of freedom
## Multiple R-squared:  0.4194, Adjusted R-squared:  0.4194 
## F-statistic: 5.633e+04 on 3 and 233952 DF,  p-value: < 0.00000000000000022
We ran model 2 in R studio with the dependent variable as price and independent variables as horsepower and engine displacement to check the Moderation analysis. ## Model 2: We can see that all the variables are significant. With an increase in horsepower, the mean price will increase by 139.92 USD, and for every decrease in engine displacement, the mean will be decreased by 13.50 USD. The RSE values are a bit low than our previous model. Checking different models with Spark
cars_model3 <- train_cars %>%
  ml_linear_regression(response = "price", features = c("horsepower","mileage","engine_type_new","fuel_tank_volume_new","maximum_seating_new"))
summary(cars_model3)#give less rich output but contain info about R2 and RMSE
## Deviance Residuals (approximate):
##      Min       1Q   Median       3Q      Max 
## -44242.7  -4997.1   -749.3   3605.3 540484.4 
## 
## Coefficients:
##          (Intercept)           horsepower              mileage 
##         8395.6553414          143.3422311           -0.1835839 
##      engine_type_new fuel_tank_volume_new  maximum_seating_new 
##        -1725.2457268           79.8898926         -332.4836811 
## 
## R-Squared: 0.5324
## Root Mean Squared Error: 12770
cars_model3$summary$r2
## [1] 0.5324408
cars_model3$summary$r2adj
## [1] 0.5324308
From the model 3 tidy version, we can see that all of them are significant, and with every increase in 1 unit of horsepower, there is an increase of 143 USD in the mean price of the cars. Similarly, when mileage changes by 1 mile, it affects the price negatively, and it decreases by 18 USD. each lower engine type decreases the average price by 1725 USD. For every 1 gallon increase in the fuel tank capacity, the average price increased by 79.9 USD. from r-squared, we can specify that it has 53.24 percent efficiency.
cars_model4 <- train_cars %>%
  ml_linear_regression(response = "price", features = c("horsepower","engine_displacement","fuel_economy","mileage","engine_type_new","fuel_tank_volume_new"))
summary(cars_model4)#give less rich output but contain info about R2 and RMSE
## Deviance Residuals (approximate):
##      Min       1Q   Median       3Q      Max 
## -42930.0  -5029.8   -659.6   3887.4 541692.7 
## 
## Coefficients:
##          (Intercept)           horsepower  engine_displacement 
##        -5795.4618792          149.5512988           -5.4088817 
##         fuel_economy              mileage      engine_type_new 
##          127.0185425           -0.1789524         2035.6673229 
## fuel_tank_volume_new 
##          297.5767170 
## 
## R-Squared: 0.5445
## Root Mean Squared Error: 12610
cars_model4$summary$r2
## [1] 0.5445234
cars_model4$summary$r2adj
## [1] 0.5445117
From model 4, with every increase in 1 unit of horsepower, there is an increase of 149.5 USD in the mean price of the cars. Similarly, when mileage changes by 1 mile, it affects the price negatively, and it decreases by 17.8 USD. from r-square, we can specify that it has 54.45 percent efficiency.
To find out which model is better predictable, we are checking MSE values.
For all 4 models we are plotting a graph
MSE_Values <- c(201549648,198697357,163144697,158928706)
MSE_Model_number <- c("1","2","3","4")
values <- data.frame(MSE_Model_number,MSE_Values)
ggplot(values)+
  geom_col(mapping = aes(x=MSE_Model_number,y=MSE_Values),fill=MSE_Model_number)    
  
From the above graph, model number 4 has the lowest MSE value compared to the other MSE Models. So, from this we can say that Price is affected by the variblles “horsepower”,“Engine_displacement”,“Fuel_economy,”mileage”,“Engine_type(Cylinder)”,and “Fuel_tank_volume”
Conclusion
From the information we got from the cluster and regression, we can say what variables affect the price. We can conclude our research question, and also, for dealer services, we can use the cluster value to make an Inventory based on the city. The graph where clusters k = 7 shows us that all the brands from one cluster make a similar inventory. For a dealer to sell the new cars or second-hand cars, we can build metrics like Kelly-Blue-Book based on the M-R Values. These metrics can also be used as a sales pitch while dealing with the customers. And also, using the prediction variables, we can set the car’s price.
