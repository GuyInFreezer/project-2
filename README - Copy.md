# Project-2

https://github.com/GuyInFreezer/project-2/assets/101156624/c04affd4-677f-4567-85f9-c5079dae670d

## Collaborators

* Yeongjin Nam
* Jana Avery
* Nee Buntoum
* Mat Nicholas
* Jay Requarth
* Dr. Dick Davis II
* John Andrews

## Links of Interest

[Google Doc for Project Notes](https://docs.google.com/document/d/10dsMXYUykWCubv5qjsCGWZZ3ls30fQex773Dv2qZ874/edit?usp=sharing)

README:

This program is intricately crafted to enhance the efficacy of a covered call trading strategy.

When an individual who owns stock opts to sell a covered call (CC), they are effectively granting the purchaser of the CC full access to reap all potential gains from that stock throughout the duration of the CC's activation.

We are concentrating on “writing” “Day 0” CCs. This means that we will (theoretically) sell the CCs at 9:40 AM and buy them back (if necessary) at the end of the day 3:50 PM.

We are using the S&P500 Index (symbol SPY) because it usually has options that expire daily. Currently, the SPY trades for approximately $520/share.

An option contract is 100 shares; thus, the investment of the seller is ~$52,000.

We are ignoring the fee to buy and sell the contract.

We are selling At-the-Money CCs; thus, if SPY=$520.25, we will sell the option contract at the $521 strike price (~$1.00). It doesn’t seem like much, but if you sell the CC every day, it adds up.


The person who is selling the CCs never loses money on the CC. Since he/she still owns the stock, he/she will lose less on a down-market day and make slightly less on an up-market day. Basically, the owner of the stock is hoping for a dull day in the market.

The data was obtained from Polygon.io.

We scraped the opening and closing price of the SPY, QQQ, VIX, DIA, and the opening and closing call price on SPY for the last 2 years. The maximum that Polygon.io allows.

We calculated the return on investment (ROI) for holding the SPY and for selling CCs for the last 2 years.

Our goal is to develop a machine learning model to help us decide when to sell the calls (don’t sell on up-days and sell on down-days).

The data was adjusted so that every row included the following data:
Date
SPY opening
QQQ opening
VXX opening
DIA opening
SPY closing – the previous day
QQQ closing – the previous day
VXX closing – the previous day
DIA closing – the previous day
SPY – 5 day change
SPY – 3 day change
SPY – 1 day change
QQQ – 5 day change
QQQ – 3 day change
QQQ – 1 day change
VXX – 5 day change
VXX – 3 day change
VXX – 1 day change
DIA – 5 day change
DIA – 3 day change
DIA – 1 day change
SPY strike price
SPY Call opening price
SPY Call closing price
trade

The SPY Call closing price occasionally needed to be adjusted because if the SPY fell, the call price was artificially inflated because no one was willing to buy the option.

We meticulously engineered a target variable named 'trade', aimed at discerning opportune moments for trading. Our objective was to execute trades when SPY exhibited a downward trend, while refraining from trading when SPY indicated an upward trajectory. Ideally, the column would have been aptly titled 'trade decision' to reflect this nuanced consideration.

The 5-day, 3-day, and 1-day change columns were created with the rolling(window=?) method.

We shifted the SPY Closing Price by 1 day so that we would not bias the models. We didn’t want to tell the models the SPY closing price. That would be cheating. Our models were trained on the previous business day’s SPY closing price. This was obtained with the shift method. Because the dollar amounts were significantly different, we scaled our X_train and X_test data with StandardScaler().

When we checked our value counts, the data was nearly evenly divided between sell the CC and don’t sell the covered call. Thus, we did not artificially inflate or deflate the number of data points in each set.

The models and results are included here:
We optimized the Random Forest Classifier to a max depth of 4.

We optimized the KNN n_neighbors to 13.

Finally, we used the X_test and y_test data to determine the ROI if we used the Random Forest (max_depth =4) model. To do that we had to reconstruct the test data frame by adding the target variable ‘trade’. Then we shifted the SPY close data back to its original position so that we could calculate the change in SPY over the day. Because of the shift one data row was sacrificed.

<img width="466" alt="Screenshot 2024-04-04 at 6 09 38 PM" src="https://github.com/GuyInFreezer/project-2/assets/101156624/e6c4b17e-2cd9-4694-801b-ee061ee61e13">