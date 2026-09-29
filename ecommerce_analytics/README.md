# E-Commerce Marketing & Customer Analytics (Excel)

## The Overview

This project analyzes a 138,116 order e-commerce dataset spanning 2021 to 2025, using native Excel PivotTables and formulas, to answer a single strategic question for the business: how should it allocate its marketing budget and manage its customer relationships to maximize profitability. The dataset (sourced from Kaggle, synthetic but realistically structured) covers 24,911 customers across four sales channels, ten marketing channels, and seventeen named marketing campaigns.

Rather than treating each question as an isolated chart, this project follows a consistent investigative approach throughout: test the obvious explanation first, check for hidden interaction effects before accepting a flat result, and rule out plausible alternative causes before finalizing a finding. Two apparent leads (a regional profit margin gap and a very high CLV to CAC ratio) were investigated and found to be artifacts of how the dataset itself was built rather than genuine business signals, and are reported honestly as such rather than presented as real findings.

## The Questions

1. Does marketing channel drive different order economics, and does that hold across every customer segment?
2. How does customer segment relate to actual profitability, and what is driving the gap?
3. What does RFM segmentation and CLV to CAC reveal about customer value?
4. Which factors drive the highest return rates?
5. Does shipping speed erode profit margin, and is it priced appropriately?
6. Which marketing campaigns actually drove profitable orders?
7. Does profit margin vary by product category, and does that reveal a marketing allocation opportunity?

## Tools I Used

Microsoft Excel, specifically native PivotTables for all group level aggregation, percentile based formulas for RFM scoring, INDEX and MATCH for cross sheet lookups, conditional formatting, and PivotCharts.

## Data Preparation and Cleaning

The dataset arrived as five related files: a 138,116 row main transaction file, a 25,000 row customer master file, a 397,569 row order items file, a product catalog, and a pre computed summary statistics file. The first six questions in this project relied entirely on the main transaction file, which already contained everything needed (customer segment, region, sales channel, marketing channel, campaign name, return status, profit, and margin), so the order items and product catalog files were initially left out, since including roughly 400,000 additional rows would not have improved any of those six answers.

Question 7 was the exception, and it specifically needed product level detail the main transaction file did not contain, so the order items file was brought in and joined to the product catalog using XLOOKUP to pull in product category and product name, and to the main transaction file to pull in return status. This is also where an important technical lesson from this project applied directly: category level PivotTable groupings were kept on product_id, a field guaranteed to be unique per product, rather than on any simplified display label, after an earlier version of a related chart revealed that grouping by a shortened, non unique label could silently combine two different products into one row and inflate their combined total.

The main transaction file served as the source table for every other PivotTable in this project. The one other exception was customer acquisition cost, which lived only in the customer master file and was pulled in via INDEX and MATCH for the RFM analysis in Question 3.

A key technical decision shaped how Question 3 was built. At this scale (24,911 customers, 138,116 orders), calculating Recency, Frequency, and Monetary values with per row formulas scanning the full order table for each customer would have meant tens of thousands of slow lookups. Instead, a PivotTable was used to aggregate Recency, Frequency, and Monetary per customer first, since PivotTables are built specifically for fast group level aggregation, and the scoring and segmentation formulas were layered on top of that smaller, already aggregated table.

## The Analysis

### Question 1: Does marketing channel drive different order economics, and does that hold across every customer segment?

**Visualize Data**

A PivotTable comparing all ten marketing channels (Affiliate, Direct, Email Marketing, Facebook Ads, Google Ads, Instagram, Organic Search, Referral, TikTok, YouTube) on Order Count, Average Order Value, Average Profit per Order, Average Profit Margin, and Return Rate. A supporting cross tab further broke margin down by channel and customer segment together, to check whether the overall pattern was hiding something underneath.

**Results**

Every metric came back nearly flat across all ten channels. Average Order Value ranged from $1,273.11 to $1,289.12, a 1.3% spread. Average Profit Margin ranged from 44.89% to 45.35%. Return Rate ranged from 6.61% to 7.06%. The supporting cross tab confirmed this held true within every customer segment individually as well, Business and Consumer customers consistently showed roughly 46% margin and Premium and VIP customers consistently showed roughly 43% margin, regardless of which of the ten channels the order came through.

**Insights**

Marketing channel does not meaningfully predict order value, profitability, or return likelihood in this business, and this was checked, not assumed, by confirming the flat pattern held at the segment level too, not just in the overall average. This rules out channel based budget reallocation as a useful lever and shifts the analytical focus toward customer level factors instead.

**Recommendation**

Deprioritize channel based budget reallocation entirely. Since no channel shows a meaningful advantage in order value, profit, margin, or return rate, spend decisions between Google Ads, Facebook Ads, Instagram, and the rest should be made on acquisition cost efficiency rather than any expectation of different order quality.

### Question 2: How does customer segment relate to actual profitability, and what is driving the gap?

**Visualize Data**

A primary PivotTable comparing the four customer segments (Business, Consumer, Premium, VIP) on Order Count, Average Order Value, Total Profit, Average Profit per Order, Average Margin, and Return Rate. A supporting table broke down Average Discount, Discount Rate, and Product Cost Rate by segment to identify the mechanism behind any gap found.

**Results**

Premium and VIP customers generated meaningfully less profit per order ($523 to $528) than Business and Consumer customers ($565 to $566), a 7.2% relative gap, despite Premium and VIP actually spending slightly more per order before any discount ($1,423 to $1,430 in gross sales, versus $1,347 to $1,355 for Business and Consumer). The supporting table found the cause: Premium and VIP customers received discount rates roughly four percentage points higher (17.7% of gross sales versus 13.7%). Product cost rate, checked as an alternative explanation, came back nearly identical across all four segments (48.62% to 48.69%) once measured directly, ruling it out. A further check controlling for cart size confirmed the discount gap held even when comparing orders of the same size, and loyalty point activity was checked and also ruled out, showing no meaningful difference by segment.

**Insights**

The segment profitability gap is caused entirely by discount policy, not by product cost, not by cart size, and not by loyalty point redemption, all three were tested directly and ruled out. Since Premium and VIP customers already spend more per order before any discount, and show no measurable difference in age, order value, or lifetime value from other segments, the extra discounting does not appear justified by anything measurable about who these customers are.

**Recommendation**

Test a reduced discount rate for Premium and VIP customers, moving from the current 17.7% of gross sales toward the 13% to 14% range seen in Business and Consumer segments, on a sample group, while tracking order frequency and retention over the following months. If retention holds, this represents close to the full profitability gap as recoverable margin.

### Question 3: What does RFM segmentation and CLV to CAC reveal about customer value?

**Visualize Data**

Recency, Frequency, and Monetary values were aggregated per customer using a PivotTable, then scored 1 through 5 using percentile breakpoints calculated from the actual data distribution rather than fixed thresholds. A snapshot date was fixed at January 1, 2026, one day after the latest order in the dataset (December 31, 2025), rather than using the live current date, so the analysis stays reproducible regardless of when the workbook is reopened. Customers were classified into six behavioral segments (Champions, Loyal Customers, At Risk, Needs Attention, New or Promising, Lost) using pattern based logic rather than a simple score total, since a raw sum concentrated too heavily in the middle to produce useful groups. Customer Lifetime Value and Customer Acquisition Cost were pulled in via INDEX and MATCH, and a segment level summary table was built comparing Customer Count percentage, Sum of Monetary percentage, Average CLV, Average CAC, Average CLV to CAC Ratio, Average Frequency, and Average Recency.

**Results**

Champions represent 13.53% of customers but generate 23.16% of total monetary value, a Value Index of 1.71, meaning they contribute 71% more than their group size alone would predict. At Risk customers, despite an average of 456 days since their last order, ranked second highest on both CLV to CAC Ratio (435.89) and Value Index (1.64), behind only Champions. Lost customers ranked lowest on every measure (Value Index of 0.42, average of 687 days since last order). Acquisition cost was found to be nearly flat across all six segments (roughly $42 regardless of segment), meaning the business is not currently targeting acquisition spend by customer type at all.

One number required a specific caveat. The overall average CLV to CAC ratio came out to 288.9, far beyond the 3 to 1 ratio generally considered healthy in real marketing analytics. This was investigated and found to stem from acquisition costs in this dataset being unrealistically small ($5 to $80) relative to lifetime value (averaging over $7,700), a property of how the dataset was constructed rather than a realistic business outcome. The absolute ratio is not a usable benchmark, but the relative comparison between segments remains valid, since the same scale issue affects every segment equally.

**Insights**

RFM segmentation worked as intended here, successfully separating customers by genuine behavioral value rather than surface level labels, made possible by this dataset's high repeat purchase rate. The clearest actionable finding is the At Risk segment: these are customers who have already proven themselves highly valuable by both individual return and proportional business impact, they simply have not ordered recently, making them the most cost efficient and best justified group for a win back campaign, rather than a broad, unfocused retention effort.

**Recommendation**

Launch a win back campaign targeting the At Risk segment specifically, rather than lapsed customers broadly. This group ranks second highest in the business on both CLV to CAC Ratio and Value Index, behind only Champions, meaning retention spend here is both high impact and low risk compared to targeting less proven customer groups.

### Question 4: Which factors drive the highest return rates?

**Visualize Data**

Three PivotTables were built: return rate by region, order count by return reason, and return rate grouped by year to check for a trend over time. A supporting check broke return reason mix down by year as well, to test whether any rising trend was concentrated in one specific cause.

**Results**

Return rate by region ranged narrowly from 6.40% (East) to 7.11% (Central and North), a 0.71 percentage point spread. Return reason counts, across all eight categories from Wrong Product to Damaged Product, fell within about 100 orders of each other (1,138 to 1,237), with no single cause dominating. The year over year breakdown told a different story: return rate climbed steadily and consistently, from 6.62% in 2021 and 2022 up to 7.15% in 2025. The follow up check confirmed this increase was broad based, the proportional mix of return reasons stayed essentially constant across every year, meaning the rise was not driven by any single cause getting worse.

**Insights**

Returns in this business are not a regional or single cause problem, both were tested directly and ruled out, but they are a real and growing structural cost, up roughly 8% in relative terms since 2021. Since the increase is broad based rather than concentrated, an effective response needs to be company wide (clearer product descriptions, tighter quality control, more realistic delivery estimates) rather than targeted at one region or one failure type, and it should be treated as increasingly urgent given the consistent upward trend rather than deprioritized as a stable cost.

**Recommendation**

Treat rising return rates as an ongoing, company wide priority rather than a stable cost to budget around. Since the increase is broad based across every region and cause rather than concentrated in one area, invest in systemic fixes, clearer product descriptions and sizing guidance, tighter quality control, and more realistic delivery estimates, applied consistently, rather than a targeted fix aimed at a single region or failure type.

### Question 5: Does shipping speed erode profit margin, and is it priced appropriately?

**Visualize Data**

A PivotTable comparing the four shipping methods (Economy, Standard, Express, Same Day) on Order Count, Average Gross Sales, Average Shipping Cost, and Average Profit Margin.

**Results**

Average Gross Sales stayed nearly flat across all four shipping methods ($1,368.56 to $1,380.80), meaning customers are not paying meaningfully more for faster shipping. Average Shipping Cost, however, more than doubled from Economy ($16.44) to Same Day ($36.65). Profit Margin fell in direct step with shipping speed, from 45.68% (Economy) down to 43.54% (Same Day).

**Insights**

Faster shipping is genuinely eroding margin, and the mechanism is clear and mechanistic rather than a data artifact, shipping cost rises sharply with speed while the amount customers pay does not rise to match it, so the extra cost comes directly out of profit. Same Day shipping in particular appears to be priced without fully accounting for its true fulfillment cost.

**Recommendation**

Audit Express and Same Day shipping pricing specifically. Since customers are not currently paying meaningfully more for faster shipping while its cost more than doubles, introducing a surcharge or a minimum order threshold to qualify for expedited shipping would let the business recover margin currently being absorbed rather than passed through.

### Question 6: Which marketing campaigns actually drove profitable orders?

**Visualize Data**

A PivotTable comparing all sixteen named marketing campaigns (excluding a Default Campaign catch all label for untracked orders) on Order Count, Average Order Value, Average Profit per Order, Total Profit, Average Profit Margin, and Average Discount.

**Results**

Referral_Program ranked highest on profit margin (45.35%) while also carrying one of the lower average discounts ($230.60). FB_Dynamic ranked lowest on margin (44.47%) with an above average discount ($249.39). Across all sixteen campaigns, a moderate correlation (negative 0.459) was found between average discount and average margin, campaigns leaning on deeper discounts tended to run lower margins, a real but less pronounced version of the pattern already confirmed at the customer segment level in Question 2.

**Insights**

Unlike marketing channel in Question 1, which showed no real signal at all, specific named campaigns within those channels do show a real, if moderate, difference in profitability. Referral_Program stands out as the strongest performer, earning its results through customer driven referrals rather than heavy discounting, while FB_Dynamic and Promo_Email lean more heavily on discounting for a below average return. This is the most direct, literal answer to how marketing budget should be allocated found anywhere in this project, and the recommendation is to shift incremental budget toward Referral_Program while re evaluating discount depth on FB_Dynamic and Promo_Email specifically.

**Recommendation**

Shift incremental marketing budget toward Referral_Program, the strongest performer on margin with the lowest reliance on discounting of any campaign tested. Separately, re evaluate discount depth on FB_Dynamic and Promo_Email, both lean on above average discounting for a below average return, and are the two weakest performers in this comparison.

### Question 7: Does profit margin vary by product category, and does that reveal a marketing allocation opportunity?

**Visualize Data**

A PivotTable built from order line item data, joined to the product catalog for category, comparing all 15 product categories on Line Item Count, Total Net Sales, Total Profit, Return Rate, and Average Margin (calculated per line item, consistent with how margin is defined everywhere else in this project).

**Results**

Profit margin varies dramatically by category, from 34.15% (Electronics) to 52.77% (Grocery), an 18.62 percentage point spread, the largest gap found anywhere in this project. Line item count stayed fairly even across categories (roughly 5.76% to 7.70% share each), ruling out volume as the explanation. Return rate also stayed flat by category (6.55% to 7.23%), so this is a genuine margin story, not a returns story. A supporting check found that Electronics, despite having the lowest margin percentage of any category, still generates the single highest total profit in dollar terms (roughly $14.1 million), more than any other category, driven entirely by its much larger sales volume.

**Insights**

Product category carries a real, structural difference in profitability that has nothing to do with how well something is marketed, lower cost, everyday categories naturally return a larger share of each sale as profit, while higher ticket categories return less. The Electronics finding is an important complicating detail, margin percentage and total profit dollars are not the same measure, and a category can be simultaneously your least efficient and your largest profit contributor.

**Recommendation**

Weight marketing and promotional effort toward higher margin categories such as Grocery, Office Supplies, and Toys & Games, rather than treating category choice as neutral. At the same time, recognize that Electronics remains a major profit contributor in absolute terms, so the goal is a more deliberate balance between margin and scale, not simply reducing investment in lower margin categories.

**Supporting Analysis: Sales vs. Profit by Category, and Top 5 Products**

Two additional PivotTables were built to stress test the category margin finding before finalizing a recommendation. The first, comparing Total Sales and Total Profit by category side by side, confirmed that Electronics leads every other category in both measures (roughly $41.1 million in sales, $14.1 million in profit), nearly double the next highest category, Jewelry, in profit dollars, despite sitting at the bottom on margin percentage. The second, a Top 5 Products by Total Profit ranking built from individual product records, reinforced the same pattern from a different angle: four of the five single highest profit generating products in the entire catalog are Electronics items (a gaming console, a smart watch, a smartphone, and a camera), with the fifth being a Jewelry pendant, the same two categories that rank lowest on margin.

Together, these two checks matter directly for the overarching question of how marketing budget should be allocated. A margin only view would suggest de-emphasizing Electronics. A profit dollars view shows Electronics is simultaneously the single most valuable category to this business in absolute terms. The correct read is not that one number is right and the other wrong, it is that budget allocation needs to weigh both, protecting and continuing to invest in Electronics given its scale, while shifting incremental, growth oriented spend toward higher margin categories where a marketing dollar converts more efficiently into profit.

## What I Learned

This project reinforced that a flat, unremarkable result at the surface level is not the end of an investigation, it is a prompt to check one layer deeper. The clearest example was Question 1, where checking channel performance within each customer segment, rather than stopping at the overall average, confirmed the flat result was genuine rather than hiding a canceled out effect. The opposite lesson came from the regional margin check late in this project, where an apparently large and interesting 4.38 percentage point gap turned out to be entirely explained by regional sales tax differences embedded in how the dataset defined net sales, not a real profitability difference, and was set aside once the mechanism was traced and understood rather than reported as a finding.

I also learned the practical limits of Excel formulas at scale. Building customer level RFM scores for 24,911 customers using the same per row formula approach that worked for a 913 row project would have been impractically slow, which is why the Recency, Frequency, and Monetary aggregation for Question 3 was done with a PivotTable first, and only the scoring logic layered on top as formulas, a more scalable pattern for large datasets that I would default to earlier next time.

Finally, this project sharpened the distinction between a metric describing one customer (like the CLV to CAC Ratio) and a metric describing a group's proportional impact (like the Value Index in Question 3), two numbers that can tell genuinely different stories about the same segment and are both needed for a complete picture.

## Conclusions

Across all seven questions, a consistent pattern emerged. Broad, surface level categories, which marketing channel a customer came through, which sales platform they purchased on, which region they live in, do not meaningfully predict profitability in this business. Specific, granular factors do: which named campaign drove the order, how deeply a specific customer segment is discounted, how a specific customer has behaved over time, how a specific shipping method is priced relative to its true cost, and which product category an order falls into. This distinction matters directly for the overarching question this project set out to answer, since it means budget and policy decisions should be made at the level of individual campaigns and specific policies, not broad categories, because that is the level at which real differences actually exist in this data.

Four findings in particular point to the same underlying inefficiency, repeated across four separate parts of the business. In product strategy, marketing effort appears to be applied evenly across categories despite an 18.62 percentage point margin gap between the best and worst performer, meaning equal effort produces very different returns depending on where it is aimed. In marketing spend, one campaign (FB_Dynamic) relies on above average discounting and still delivers the weakest margin of any campaign, while another (Referral_Program) achieves the strongest margin with comparatively little discounting at all, evidence that discount dependent growth is being funded in a place where the data does not support it. In customer retention, an At Risk segment generates 1.65 times its proportional share of value despite being inactive for over a year, nearly matching Champions, while a Lost segment generates only 0.43 times its share, two groups that would look identical if only current activity were considered, but that clearly are not identical in value. In pricing, Premium and VIP customers receive a discount rate roughly four percentage points deeper than Business and Consumer customers, despite spending more per order beforehand, a gap with no measurable behavioral justification.

Taken together, these four findings are not four unrelated issues. They are the same pattern appearing in four different areas: resources, whether marketing effort, discount dollars, or retention spend, are being allocated as though groups are uniform, when the data consistently shows they are not. That repetition across product, marketing, retention, and pricing is the central finding of this project, more so than any single result on its own.

## Closing Thoughts

This project combined large scale PivotTable aggregation, percentile based customer scoring, cross sheet lookups, and repeated verification against independently calculated numbers throughout. Two of the most valuable moments in the project were not the headline findings themselves, but the points where a plausible looking result (the regional margin gap, the extreme CLV to CAC ratio) was investigated rather than accepted, and turned out to need an honest caveat rather than a celebration. That kind of verification, tracing a surprising number back to its source before trusting it, is the skill I most want a reader of this project to take away from it.
