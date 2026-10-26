# Finding the Best Markets to Advertise In

An e-learning company selling $59/month programming courses wants to know which two countries to advertise in. Data: the 2017 freeCodeCamp New Coders Survey, 18,175 responses across 136 columns.

Dataquest guided project, Data Scientist in Python path.

## The recommendation

**Advertise in the United States. Do not pick a second market from this data; run a paid test instead.**

That is narrower than the brief asked for, and it is what the data supports.

| Country | n | Mean | Mean 95% CI | Median | Share spending $59+ | Share CI |
|---|---|---|---|---|---|---|
| **US** | 2,918 | $138.98 | [121.7, 157.0] | **$3.33** | **22.2%** | [20.7, 23.7] |
| India | 462 | $113.75 | [70.6, 164.9] | **$0.00** | 16.0% | [12.8, 19.5] |
| Canada | 240 | $113.51 | [64.9, 176.1] | **$0.00** | 16.7% | [12.1, 21.3] |
| UK | 279 | $45.53 | [28.3, 66.3] | **$0.00** | 15.4% | [11.5, 19.7] |

## Three findings the point-estimate version cannot see

### 1. The median monthly spend is $0 in three of the four countries

The $113 mean for India describes almost nobody: **56.7% of Indian respondents spent exactly $0**, along with 52.5% in Canada, 50.9% in the UK and 45.0% in the US. The means are carried entirely by a paying minority, and calling them "average spend" invites a reader to picture a typical customer who does not exist.

For a $59 subscription the honest metric is the share already spending at least that much per month: a proportion, far more stable on a skewed variable, and what the recommendation rests on.

### 2. The ranking is not statistically distinguishable

Bootstrapped differences in mean monthly spend, 10,000 resamples:

| Comparison | Difference | 95% CI | Excludes zero? | P(first > second) |
|---|---|---|---|---|
| US vs India | $25.10 | [-28.5, 72.6] | **no** | 0.838 |
| US vs UK | $93.42 | [66.4, 119.2] | yes | 1.000 |
| US vs Canada | $25.38 | [-38.8, 78.0] | **no** | 0.802 |
| India vs UK | $68.32 | [20.5, 121.4] | yes | 0.998 |
| **India vs Canada** | **$0.28** | **[-75.8, 71.1]** | **no** | **0.514** |
| UK vs Canada | -$68.04 | [-132.5, -14.5] | yes | 0.005 |

Only 3 of 6 comparisons are distinguishable, and India versus Canada is a **coin flip**: 28 cents of difference with P = 0.514.

This matters because the conventional answer to this project is "the US and India" or "the US and Canada", decided on exactly that difference. On the metric that matters, the share spending $59+, the US interval [20.7, 23.7] sits clear of India [12.8, 19.5] and the UK [11.5, 19.7] and overlaps only slightly with Canada [12.1, 21.3]. The US is defensible; second place is not.

### 3. The outlier cutoff decides the answer

| Cutoff | Dropped | US | India | UK | Canada | Top two by mean |
|---|---|---|---|---|---|---|
| none | 0 | $228.00 | $135.10 | $45.53 | $113.51 | US, India |
| <= $10,000 | 8 | $155.46 | $135.10 | $45.53 | $113.51 | US, India |
| <= $5,000 | 16 | $138.98 | $113.75 | $45.53 | $113.51 | US, India |
| <= $2,000 | 69 | $89.52 | $65.76 | $45.53 | $83.64 | **US, Canada** |
| <= $1,000 | 141 | $59.39 | $47.87 | $32.08 | $46.93 | US, India |
| <= $500 | 239 | $38.82 | $25.82 | $25.25 | $27.85 | **US, Canada** |
| <= $200 | 419 | $20.54 | $15.61 | $18.54 | $12.85 | **US, UK** |

**Three different second-place countries appear**, depending only on where the cutoff is drawn. The US is first at every cutoff, which is why that part is safe; the second slot is a cleaning decision, not a market finding.

The extreme values explain themselves: the largest figures pair a big total with `MonthsProgramming` of 1, the signature of a bootcamp fee or degree tuition divided by a small number, not a monthly subscriber.

## The sample is not the market

| | Value |
|---|---|
| Stated a role interest | 6,992 of 18,175 (38.5%) |
| Mentioning web or mobile development | **86.3%** |
| Selecting only one role | 31.7% |

The survey was run by freeCodeCamp, which teaches web development for free, so the 86.3% is a fact about the sampling frame rather than about global course demand. Worse, **61.5% never answered the question that defines the target market**, and only 31.7% named a single role, so most of the sample is still exploring rather than committed enough to pay monthly.

The analysis is still worth doing, because it is the data that exists. Its output is a hypothesis to test, not a market estimate.

## Market context behind the averages

| Country | n | Median age | Attended bootcamp | Median months coding | Spent $0 | Spend $59+ |
|---|---|---|---|---|---|---|
| US | 2,918 | 28 | 7.9% | 6 | 45.0% | 22.2% |
| India | 462 | 21 | 1.5% | 9 | 56.7% | 16.0% |
| UK | 279 | 27 | 2.5% | 6 | 50.9% | 15.4% |
| Canada | 240 | 26 | 3.8% | 6 | 52.5% | 16.7% |

The US advantage is not only spend: its respondents are older, five times more likely to have paid for a bootcamp, and least likely to have spent nothing. That is a consistent picture of established willingness to pay, and it is the part of the finding least dependent on which statistic is chosen.

India is the informative opposite: the youngest cohort, the **longest** median time programming, and the highest share spending nothing. A large, engaged audience that does not currently pay, which makes the $59 price the thing to question there rather than the audience.

## What to do instead of naming a second market

1. **Commit most of the budget to the US.** It leads on every statistic at every cutoff, and its lead on the business metric is outside the interval of two of three alternatives.
2. **Split a small test budget across India, Canada and the UK** and measure real conversion. The question the survey cannot answer is cheap to answer with an experiment that measures purchases rather than recalled spending.
3. **Test a lower price point in India.** A large engaged audience with the longest median time coding and 56.7% spending nothing suggests a price objection, not an interest objection.

## Method

1. Assess representativeness first, since nothing downstream is reliable if the sample is wrong for the market.
2. Role-interest frequency, including how many respondents are still exploring.
3. Country counts, noting that sample size rather than business reasoning fixes the candidate set.
4. Spend per month, handling the `MonthsProgramming == 0` division explicitly.
5. Four summary statistics per country, showing the rankings disagree.
6. Outlier sensitivity across eight cutoffs.
7. Bootstrap confidence intervals and pairwise differences, 10,000 resamples.
8. Cross-check spend against reported income as a validity test on the spend field.

## Limitations

- Self-reported, recalled, unverified spending. The weak correlation with reported income is consistent with a noisy measure.
- `MonthsProgramming == 0` replaced with 1, inflating per-month figures for the newest learners, the group most likely to have made a single lump-sum purchase. That choice was not sensitivity-tested the way the outlier cutoff was.
- Past spend is not willingness to pay for this product. Someone who paid for a bootcamp may be done buying.
- Country of residence is a crude market definition: it ignores purchasing power, local competition, ad cost per impression and language. A market where customers spend less but ads cost a tenth as much could be the better investment, and no cost data was available.
- 2017 data, in a market that changed substantially afterwards.
- The bootstrap quantifies sampling variability, which is real and large, but cannot correct the selection bias. A wide interval and a biased sample are different problems and it addresses only the first.

## Stack

Python, pandas, NumPy, matplotlib. The bootstrap is hand-written; no SciPy or scikit-learn.

## Files

- `notebook.ipynb`: the full analysis with executed outputs and figures.