
# Market Basket Analysis 

Market basket analysis can provide several benefits for a business analyst, including:

* 	**Identifying cross-selling opportunities:** By analyzing the products frequently purchased together, business analysts can identify opportunities for cross-selling and recommend complementary products to customers.

* 	**Optimizing product placement:** By understanding the products that are frequently purchased together, business analysts can recommend optimal product placement in physical or online stores.

* 	**Understanding customer behavior:** Market basket analysis can provide insights into customer behavior, preferences, and trends. This information can be used to develop targeted marketing campaigns, optimize pricing, and improve customer experience.

* 	**Improving inventory management:** By understanding which products are frequently purchased together, businesses can better manage their inventory, reduce waste, and ensure that they always have the most popular products in stock.

Below is the code for the market basket analysis performed:



## Code
**Setting home directory**
```rscript
setwd("C:/Users/aashi/Downloads")
getwd()
```
**Importing libraries**
```rscript
library(tidyverse)
library(lubridate) # library to change date from field to class 
library(arules)	# library for apriori for market basket analysis 
library(arulesViz) # library to plot apriori

```
**Load dataset**
```rscript
grocery_data <- read.csv("Groceries_dataset.csv")
```
**Renaming columns and making the variables as a factor**
```rscript
names(grocery_data) <- c("member_id", "date", "item") grocery_data$member_id <- as.factor(grocery_data$member_id) grocery_data$date <- as.factor(dmy(grocery_data$date)) grocery_data$item <- as.factor(grocery_data$item)
```
**Remove null values**
```rscript
grocery_data <- na.exclude(grocery_data)
```
**Sorting the orders**
```rscript
grocery_data <- grocery_data[order(grocery_data$member_id),]
str(grocery_data)
```
**Grouping items based on customer buying on the same date**
```rscript
grocery_list <- ddply(grocery_data, c("member_id","date"),
function(df1)paste(df1$item,collapse = ","))
```
**Removing memberid and date from the list**
```rscript
grocery_list$member_id <- NULL grocery_list$date <- NULL colnames(grocery_list) <- c("item_list")
```
**Rewriting the data into new file**
```rscript
write.csv (grocery_list,"Grocery_List.csv", quote = FALSE, row.names = TRUE) head(grocery_list)
basket_list = read.transactions(file="Grocery_List.csv",
rm.duplicates= TRUE, format="basket",sep=",",cols=1);
print(basket_list)
```
**Preprocessing removing punctuation marks**
```rscript
basket_list@itemInfo$labels <- gsub("\"","",basket_list@itemInfo$labels)
basket_item <- apriori(basket_list, parameter = list(minlen=2,
sup = 0.001, conf = 0.05, target="rules"))
summary(basket_item)
```
**Top10 items sets with highest support, highest confidence and highest lift**
```rscript
inspect(head(sort(basket_item, by = "support"), 10)) inspect(head(sort(basket_item, by = "confidence"), 10)) inspect(head(sort(basket_item, by = "lift"), 10))
```
**Selection criteria**
```rscript
topsupport <- head(sort(basket_item, by = "support"), 100) topconfident <- head(sort(basket_item, by = "confidence"), 100) toplift <- head(sort(basket_item, by = "lift"), 100)
inspect(topsupport)
```
**Graph for highest support**
```rscript
plot(topsupport, jitter = 0) plot(topsupport[1:20], method="graph") plot(topsupport[1:20], method="matrix") plot(topsupport[1:20], method="paracoord") plot(topsupport[1:20], method="scatterplot")
plot(topsupport[1:20], method="grouped matrix")
```
**Graph for highest confidence**
```rscript
plot(topconfident, jitter = 0) plot(topconfident[1:20], method="graph") plot(topconfident[1:20], method="matrix") plot(topconfident[1:20], method="paracoord") plot(topconfident[1:20], method="scatterplot")
plot(topconfident[1:20], method="grouped matrix")
```
## Output
![WhatsApp Image 2023-02-13 at 12 55 21 PM](https://user-images.githubusercontent.com/85166438/218536953-87b36ecd-c691-44d3-8cf8-2aefc381aa22.jpeg)
![WhatsApp Image 2023-02-13 at 12 55 39 PM](https://user-images.githubusercontent.com/85166438/218536976-524b400a-6110-4b12-9675-de86eade88d2.jpeg)
![WhatsApp Image 2023-02-13 at 12 56 03 PM](https://user-images.githubusercontent.com/85166438/218536983-a4024f8a-f429-4347-ac7e-5711215eb92d.jpeg)
![WhatsApp Image 2023-02-13 at 12 44 51 PM](https://user-images.githubusercontent.com/85166438/218537300-6184d73c-c8c5-4b28-912f-50c1e2319dda.jpeg)
![WhatsApp Image 2023-02-13 at 12 57 08 PM](https://user-images.githubusercontent.com/85166438/218536992-85bd1ddf-f04d-49d1-9fba-11ffbfe7065a.jpeg)
![WhatsApp Image 2023-02-13 at 12 57 28 PM](https://user-images.githubusercontent.com/85166438/218537011-fdb51373-1752-407d-ac6a-ddccb0a790e0.jpeg)
![WhatsApp Image 2023-02-13 at 12 57 49 PM](https://user-images.githubusercontent.com/85166438/218537023-4bc107d8-c3b3-485e-a624-50cd116c7c37.jpeg)
![WhatsApp Image 2023-02-13 at 12 58 14 PM](https://user-images.githubusercontent.com/85166438/218537031-6a750bc5-2e78-4a36-b55c-be1ff216c670.jpeg)
![WhatsApp Image 2023-02-13 at 12 59 00 PM](https://user-images.githubusercontent.com/85166438/218537043-07d6ea72-2022-44fe-b436-2bd4e4c649b9.jpeg)
![WhatsApp Image 2023-02-13 at 12 59 52 PM](https://user-images.githubusercontent.com/85166438/218537054-0399f6a1-1bc9-4a7f-a5d5-d58e442e84a2.jpeg)

## Insights

By analyzing the data with consideration to both Support and Confidence, it was possible to gain valuable insights into the relationships among various items, and we were able to pinpoint multiple itemsets that are frequently sold together. Our examination revealed that in the itemset with high support, combinations such as Whole milk, other vegetables, rolls/buns, and soda were commonly purchased. Similarly, in the itemset with high confidence, Whole milk was typically sold together with items such as sausages, yogurts, rolls/buns, and semi-finished bread.

## Recommendations

To potentially increase sales, profits, and revenue, it is recommended that the grocery store group frequently purchased items together in close proximity to each other. Based on the Apriori algorithm model, placing whole milk alongside vegetables, rolls/buns, and soda could lead to increased sales. Furthermore, according to the high confidence model, placing whole milk next to sausages, yogurts, rolls/buns, and semi-finished bread could result in a higher profit margin and increased revenue.
