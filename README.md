
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
setwd("C:/Users/aashi/Downloads/uWindsor/Sem 3/Intro to Data Analytics - Brent/Assignment3")
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
