# Development of a Market-Making Strategy
### Joint work with [Gabriel Romon](https://github.com/gabsens)

The goal of this project is to create a market-making algorithm using an `order book dataset` and an associated `trade book dataset`.
A market-making strategy consists essentially of submitting bid-ask quotes $(p_{t}^b, p_{t}^a)$ at period $t$ in order to `capture the bid-ask spread` if the two orders are filled over time.


**The question is how to set the pair $(p_{t}^b, p_{t}^a)$ ?**
### Game Plan:
In this project, we will look into **two simple ideas**, or a **combination of the two**:

- **Strategy-1:** Setting the bid and ask quotes to be the same (or very close to) the previous one we saw in the order book:
$$\big(p_{t-1}^{\text{best bid}} - \mu, p_{t-1}^{\text{best ask}} + \mu \big)$$
with $\mu$ a parameter to be calibrated using `historical data`. Typically, we'll use $$\mu = \underbrace{\sigma\big( \text{mid price return} \big)}_{\text{volatility}} \sigma\big(spread  \big).$$

- **Strategy-2:** 
Setting the bid and ask quotes around the last transaction price (denoted $p_{t-1}$), which means establishing the bid-ask pair as:
$$\big(p_{t-1} - \delta/2, p_{t-1} + \delta/2 \big).$$ 
The fixed spread $\delta$ would be calculated, for example, by taking the mean (or median) of the previous historical spreads, $\delta = \text{mean}\big(spread \big)$.

**First remark:**\
Since this is a `high-frequency trading strategy`, we want to be `as close as possible to the real-time market`.\
So, depending on the most recent data available, we will alternate between the first and second ideas:
-  For example, if the most recent available data at time $t$ comes from the trade book (rather than the order book), 
we would use the second strategy idea, putting quotes around the last transaction price.
- On the contrary, if the most recent available data comes from the order book, we will employ the first strategy and set bid and asks quotes to be very close to the ones we observed in the recent past.

**Second remark:**\
Our strategy might be improved by taking into account short run price movement (price movement for the next period $t$). Indeed, we can put bid and ask quotes that try to follow the anticipated (short term) trend  (up trend or down trend):
- if we expect the (mid) price to go up in the next period, we could set a more aggressive bid quote and a less aggressive ask quotes, such as
$$\big(p_{t-1} - \delta/4, p_{t-1} + (3/4) \delta \big).$$
- if we anticipate that the (mid) price to go down in the next period, we can suggest the following quotes: 
$$\big(p_{t-1} - (3/4)\delta, p_{t-1} +  \delta/4 \big).$$
To do so we will examine various order book features such as `order book pressure` and the `order book imbalance`.\
Especially looking at the impact it might have on the (mid-price) return
$$ \text{mid price} = \frac{p^b_{\text{best}} + p^a_{\text{best}}}{2}.$$

Furthermore, if the market is `trending`, it may result in `hitting too frequently only one side` of the order, and resulting in an `unbalanced inventory`.
As a result, it is critical to consider short-term price movement.


**Third remark:**\
Since the spread may be influenced by volatility (of the mid price), it may be worthwhile to set the bid asks pair as a function of volatility.\
At the very least, we will investigate the relationship between the `spread` and the `volatility` (standard deviation of log mid price??returns).


## Articles


```BibTeX
@article{kanagal2017market,
  title={Market Making with Machine Learning Methods},
  author={Kanagal, Kapil and Wu, Yu and Chen, Kevin},
  journal={Available online here https://web. stanford. edu/class/msande448/2017/Final/Reports/gr4. pdf},
  year={2017}
}
```
```BibTeX
@article{das2003intelligent,
  title={Intelligent market-making in artificial financial markets},
  author={Das, Sanmay},
  year={2003}
}
```
* [Learn what a depth chart is and how to create it in Python](https://towardsdatascience.com/learn-what-a-depth-chart-is-and-how-to-create-it-in-python-323d065e6f86)
* [Python Tutorial: Explore and Visualize Crypto Order Book Snapshots with Kaiko???s API](https://blog.kaiko.com/python-tutorial-explore-and-visualize-crypto-order-book-snapshots-with-kaikos-api-a9a77ae90b65)



