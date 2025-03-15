
# Investigating the Significance of Recipe Tags
<p style="text-align:center;">a DSC 80 project — Nicolas Toon</p>

### <strong>Introduction</strong>
Food is undoubtedly a significant part of human lives. We eat food to survive, yet it can be a significant detriment to our lives with too much consumption, as seen by the obesity epidemic in the United States. One solution to controlling consumption is cooking, so that the amount of ingredients and macronutrients can be controlled. Online recipes can be a great method to find something simple and quick to prep, yet healthy to eat.

Similar to social media, most online recipes contain tags that help expand their audience. However, many posts put lots of tags in order to maximize outreach. The question I pose and strive to answer is: are tags representative of the content they are on?

In this project, I examined two datasets originally scraped by <a href='https://cseweb.ucsd.edu/~jmcauley/pdfs/emnlp19c.pdf'>Majumder, et al.</a>, which compiled recipe details and reviews posted on <a href='https://www.food.com/'>food.com</a> from 2008 to 2018.

The first dataset, <code>recipes</code>, contains 83,782 rows, with each row denoting a unique recipe. Each unique recipe has 12 features:
<table>
  <tr>
    <th>Feature</th>
    <th>Data Type</th>
    <th>Description</th>
  </tr>
  <tr>
    <td><code>'name'</code></td>
    <td><code>str</code></td>
    <td>recipe name</td>
  </tr>
  <tr>
    <td><code>'id'</code></td>
    <td><code>int64</code></td>
    <td>unique identifier</td>
  </tr>
  <tr>
    <td><code>'minutes'</code></td>
    <td><code>int64</code></td>
    <td>estimated time</td>
  </tr>
  <tr>
    <td><code>'contributor_id'</code></td>
    <td><code>int64</code></td>
    <td>unique identifier for user who posted the recipe</td>
  </tr>
  <tr>
    <td><code>'submitted'</code></td>
    <td><code>str</code></td>
    <td>posting date</td>
  </tr>
  <tr>
    <td><code>'tags'</code></td>
    <td><code>str</code></td>
    <td>recipe tags</td>
  </tr>
  <tr>
    <td><code>'nutrition'</code></td>
    <td><code>str</code></td>
    <td>nutritional information</td>
  </tr>
  <tr>
    <td><code>'n_steps'</code></td>
    <td><code>int64</code></td>
    <td>number of steps</td>
  </tr>
  <tr>
    <td><code>'steps'</code></td>
    <td><code>str</code></td>
    <td>steps for recipe, in order</td>
  </tr>
  <tr>
    <td><code>'description'</code></td>
    <td><code>str</code></td>
    <td>user generated description</td>
  </tr>
  <tr>
    <td><code>'ingredients'</code></td>
    <td><code>str</code></td>
    <td>ingredients used</td>
  </tr>
  <tr>
    <td><code>'n_ingredients'</code></td>
    <td><code>int64</code></td>
    <td>number of ingredients</td>
  </tr>
</table>

The second dataset, <code>interactions</code>, contains 731,927 rows, with each row denoting a unique review. Each unique review has 5 features:
<table>
  <tr>
    <th>Feature</th>
    <th>Data Type</th>
    <th>Description</th>
  </tr>
  <tr>
    <td><code>'user_id'</code></td>
    <td><code>int64</code></td>
    <td>unique identifier for reviewer</td>
  </tr>
  <tr>
    <td><code>'recipe_id'</code></td>
    <td><code>int64</code></td>
    <td>unique identifier for the recipe the review is about</td>
  </tr>
  <tr>
    <td><code>'date'</code></td>
    <td><code>str</code></td>
    <td>review date</td>
  </tr>
  <tr>
    <td><code>'rating'</code></td>
    <td><code>int64</code></td>
    <td>rating given</td>
  </tr>
  <tr>
    <td><code>'review'</code></td>
    <td><code>str</code></td>
    <td>review/comment text</td>
  </tr>
</table>

### <strong>Data Cleaning and Exploratory Data Analysis</strong>
#### <b>Data Cleaning</b>
To make the data exploration process easier, the following steps were executed, in order:
<ol type=1>
  <li>Left merging the <code>recipes</code> and <code>interactions</code> datasets on the recipe id</li>
    <ul>
      <li>This matches each review with its corresponding recipe, making it easier to associate ratings with recipes.</li>
    </ul>


  <li>Replacing all zeroes with <code>np.nan</code></li>
    <ul>
      <li>Any entry of 0 does not make sense for each of the features, so 0 can be assumed as "missing values". Thus, we can replace all zeroes so that they are not included in data analysis.</li>
    </ul>


  <li>Finding the average rating of each recipe</li>
    <ul>
      <li>Taking the mean of all ratings associated with each recipe allows us to generalize ratings from recipe to recipe.</li>
    </ul>


  <li>Keeping only unique recipes</li> 
    <ul>
      <li>Since merging resulting in duplicate recipes, and we only need to analyze recipes and not individual reviews, we can filter for unique recipes, so that recipes with more reviews don't count more than recipes with less reviews.</li>
    </ul>


  <li>Converting <code>'submitted'</code> to a datetime object</li>
    <ul>
      <li>Originally, <code>'submitted'</code> was a string, but by converting to a datetime object, we can manipulate the date easily.</li>
    </ul>


  <li>Converting <code>'nutrition'</code> to a list</li>
    <ul>
      <li>Originally, <code>'nutrition'</code> was a string, but by converting to a list, we can manipulate information from the nutritional values easily.</li>
    </ul>


  <li>Converting <code>'ingredients'</code> to a list</li>
    <ul>
      <li>Originally, <code>'ingredients'</code> was a string, but by converting to a list, we can manipulate information from the ingredients easily.</li>
    </ul>


  <li>Converting <code>'tags'</code> to a list</li>
    <ul>
      <li>Originally, <code>'tags'</code> was a string, but by converting to a list, we can manipulate information from the tags easily.</li>
    </ul>


  <li>Creating a column of <code>'n_tags'</code></li>
    <ul>
      <li><code>'n_tags'</code> is a column of integers that corresponds to the number of tags each recipe has, which will be useful in comparing recipes with more vs. less tags.</li>
    </ul>
    

  <li>Creating a column of <code>'year'</code></li>
    <ul>
      <li><code>'year'</code> is a column of integers that corresponds to the year that each recipe was posted, which will be useful for any time series analysis.</li>
    </ul>


  <li>Dropping any unnecessary/repetitive columns</li>
    <ul>
      <li>Any columns that contained repetitive information as other columns were dropped. In addition, any columns that contains unnecessary information (for the purposes of this project) were dropped.</li>
    </ul> 
</ol>

The first five rows of the cleaned DataFrame are shown below:

| name                                 |     id |   minutes |   n_steps |   n_ingredients |   avg_rating |   n_ratings |   n_tags |   year |
|:-------------------------------------|-------:|----------:|----------:|----------------:|-------------:|------------:|---------:|-------:|
| 1 brownies in the world    best ever | 333281 |        40 |        10 |               9 |            4 |           1 |       14 |   2008 |
| 1 in canada chocolate chip cookies   | 453467 |        45 |        12 |              11 |            5 |           1 |        9 |   2011 |
| 412 broccoli casserole               | 306168 |        40 |         6 |               9 |            5 |           4 |       10 |   2008 |
| millionaire pound cake               | 286009 |       120 |         7 |               7 |            5 |           1 |       20 |   2008 |
| 2000 meatloaf                        | 475785 |        90 |        17 |              13 |            5 |           2 |       10 |   2012 |

Because the columns <code>'nutrition'</code>, <code>'ingredients'</code>, and <code>'tags'</code> are lists within the DataFrame, we extracted the lists into separate DataFrames, with row order preserved (so the first row of the new DataFrames will correspond to the same recipe as the first row of the cleaned DataFrame). In <code>ingredients_df</code> and <code>tags_df</code>, 1 represents that the recipe contains the ingredient/tag and 0 represents that the recipe does not contain the ingredient/tag.

The first two rows of each new DataFrame are shown below:

<code>nutrition_df</code>:

|   calories (#) |   total fat (PDV) |   sugar (PDV) |   sodium (PDV) |   protein (PDV) |   saturated fat (PDV) |   carbohydrates (PDV) |
|---------------:|------------------:|--------------:|---------------:|----------------:|----------------------:|----------------------:|
|          138.4 |                10 |            50 |              3 |               3 |                    19 |                     6 |
|          595.1 |                46 |           211 |             22 |              13 |                    51 |                    26 |

<code>ingredients_df</code>:

|   salt |   butter |   sugar |   onion |   olive oil |   water |   garlic cloves |   eggs |   milk |   pepper |   flour |   all-purpose flour |   brown sugar |   baking powder |   egg |   salt and pepper |   parmesan cheese |   black pepper |   lemon juice |   vegetable oil |   baking soda |   garlic clove |   cinnamon |   garlic powder |   tomatoes |
|-------:|---------:|--------:|--------:|------------:|--------:|----------------:|-------:|-------:|---------:|--------:|--------------------:|--------------:|----------------:|------:|------------------:|------------------:|---------------:|--------------:|----------------:|--------------:|---------------:|-----------:|----------------:|-----------:|
|      1 |        1 |       1 |       0 |           0 |       0 |               0 |      1 |      0 |        0 |       1 |                   1 |             0 |               0 |     1 |                 0 |                 0 |              0 |             0 |               0 |             0 |              0 |          0 |               0 |          0 |
|      1 |        0 |       1 |       0 |           0 |       1 |               0 |      1 |      0 |        0 |       1 |                   1 |             1 |               0 |     1 |                 0 |                 0 |              0 |             0 |               0 |             1 |              0 |          0 |               0 |          0 |

<code>tags_df</code>:

|   preparation |   time-to-make |   course |   main-ingredient |   dietary |   easy |   occasion |   cuisine |   low-in-something |   60-minutes-or-less |   main-dish |   3-steps-or-less |   30-minutes-or-less |   meat |   number-of-servings |   vegetables |   15-minutes-or-less |   4-hours-or-less |   taste-mood |   low-carb |   low-sodium |   north-american |   healthy |   desserts |   low-cholesterol |   low-calorie |   5-ingredients-or-less |   equipment |   vegetarian |   beginner-cook |   low-protein |   low-saturated-fat |   inexpensive |   dinner-party |   for-1-or-2 |   american |   pasta-rice-and-grains |   eggs-dairy |   side-dishes |   european |   holiday-event |   weeknight |   poultry |   fruit |   kid-friendly |   low-fat |   chicken |   lunch |   comfort-food |   appetizers |   for-large-groups |   presentation |   seasonal |   brunch |   one-dish-meal |   beef |   breakfast |   salads |   breads |   seafood |   free-of-something |   asian |   beverages |   cheese |   soups-stews |   pasta |
|--------------:|---------------:|---------:|------------------:|----------:|-------:|-----------:|----------:|-------------------:|---------------------:|------------:|------------------:|---------------------:|-------:|---------------------:|-------------:|---------------------:|------------------:|-------------:|-----------:|-------------:|-----------------:|----------:|-----------:|------------------:|--------------:|------------------------:|------------:|-------------:|----------------:|--------------:|--------------------:|--------------:|---------------:|-------------:|-----------:|------------------------:|-------------:|--------------:|-----------:|----------------:|------------:|----------:|--------:|---------------:|----------:|----------:|--------:|---------------:|-------------:|-------------------:|---------------:|-----------:|---------:|----------------:|-------:|------------:|---------:|---------:|----------:|--------------------:|--------:|------------:|---------:|--------------:|--------:|
|             1 |              1 |        1 |                 1 |         0 |      0 |          0 |         0 |                  0 |                    1 |           0 |                 0 |                    0 |      0 |                    1 |            0 |                    0 |                 0 |            0 |          0 |            0 |                0 |         0 |          1 |                 0 |             0 |                       0 |           0 |            0 |               0 |             0 |                   0 |             0 |              0 |            0 |          0 |                       0 |            0 |             0 |          0 |               0 |           0 |         0 |       0 |              0 |         0 |         0 |       1 |              0 |            0 |                  1 |              0 |          0 |        0 |               0 |      0 |           0 |        0 |        0 |         0 |                   0 |       0 |           0 |        0 |             0 |       0 |
|             1 |              1 |        0 |                 0 |         0 |      0 |          0 |         1 |                  0 |                    1 |           0 |                 0 |                    0 |      0 |                    1 |            0 |                    0 |                 0 |            0 |          0 |            0 |                1 |         0 |          0 |                 0 |             0 |                       0 |           0 |            0 |               0 |             0 |                   0 |             0 |              0 |            0 |          1 |                       0 |            0 |             0 |          0 |               0 |           0 |         0 |       0 |              0 |         0 |         0 |       0 |              0 |            0 |                  1 |              0 |          0 |        0 |               0 |      0 |           0 |        0 |        0 |         0 |                   0 |       0 |           0 |        0 |             0 |       0 |

#### <b>Univariate Analysis</b>
To initially explore the data, I examined the distribution of ratings for recipes.

<iframe
  src="plots/avg-ratings-hist.html"
  width="750"
  height="550"
  frameborder="0"
></iframe>

As the histogram shows, ratings are significantly skewed left. This implies that there are more recipes on that are rated a 4 through 5, compared to 0 through 4, creating a massive class imbalance.

---

I also examined the distribution of the number of tags for each recipe.

<iframe
  src="plots/n-tags-hist.html"
  width="750"
  height="550"
  frameborder="0"
></iframe>

The distribution of the histogram is approximately normal with an approximate mean of 16.3 and an approximate standard deviation of 6.8. This suggests that recipes can be expected to have anywhere from 10 to 23 tags, using the empirical rule. In addition, the dataset seems to contain outliers that have over 40 tags!

#### <b>Bivariate Analysis</b>
For further exploration, I was curious about if there were any tags that became more popular as the years passed, so I plotted the proportion of each tag's usage over time.

<iframe
  src="plots/tag-props-line.html"
  width="750"
  height="550"
  frameborder="0"
></iframe>

We can see that most tags don't have much usage, only making up less than 4% of all tags used. There are a few tags that are frequently used, such as <code>'preparation'</code>, <code>'time-to-make'</code>, and <code>'course'</code>. The line graph is a bit cluttered, so let's only take the five most variable lines (lines that have the greatest minimum and maximum proportion).

<iframe
  src="plots/top-5-line.html"
  width="750"
  height="550"
  frameborder="0"
></iframe>

The three tags mentioned earlier, <code>'preparation'</code>, <code>'time-to-make'</code>, and <code>'course'</code>, also have a lot of variance. The tag <code>'60-minutes-or-less'</code> had a big jump in usage in 2015! On the other hand,the tag <code>'easy'</code> actually had a great decrease in 2018, meaning the usage for the tag significantly dropped in recipes posted in 2018!

#### <b>Interesting Aggregates</b>
I was curious whether recipes with a certain tag had attributes that recipes without that tag didn't have. For this investigation, I utilized the <code>'dietary'</code> tag, and grouped recipes with and without the tag. Then, I took the mean of the nutritional values for each group. The aggregation is shown in the DataFrame below:

| dietary   |   calories (#) |   total fat (PDV) |   sugar (PDV) |   sodium (PDV) |   protein (PDV) |   saturated fat (PDV) |   carbohydrates (PDV) |
|:----------|---------------:|------------------:|--------------:|---------------:|----------------:|----------------------:|----------------------:|
| False     |          326.7 |                23 |            23 |             16 |              20 |                    26 |                     9 |
| True      |          292.6 |                18 |            23 |             13 |              17 |                    18 |                     9 |

The DataFrame shows that nutritional values in recipes with the <code>'dietary'</code> tag were consistently less than or equal to nutritional values in recipes without the <code>'dietary'</code> tag. This suggests that there is a difference between the two groups, but is this difference significant or not?

### <strong>Assessment of Missingness</strong>
By taking a look at the columns in the cleaned dataset, there are two columns with a significant number of missing values: <code>'description'</code> and <code>'avg_rating'</code>

#### <b>NMAR Analysis</b>
I believe that the missingness of <code>'description'</code> is NMAR (not missing at random) because the values of <code>'description'</code> depends on whether the contributor submitted a description or not. There could be a variety of reasons that causes a contributor to not submit a description (forgetfulness, complicated details, self-explanatory recipe, etc.). One variable that we could collect to explain the missingness of the data is whether the contributor is part of a company or is known to be a cooking influencer, since contributors who are well established/dedicated to recipe-making often add descriptions. If the dataset included this variable, then it might be probable that <code>description</code> is MAR (missing at random).

#### <b>Missingness Dependency</b>
I suspect that the missingness of <code>'avg_rating'</code> is MAR (missing at random). In other words, the missingness of <code>'avg_rating'</code> is related to the values of another column. The first column I will investigate is <code>minutes</code>.

---

<p><b>Null Hypothesis: </b>The missingness of <code>'avg_rating'</code> is not related to the recipe time.</p>
<p><b>Alternate Hypothesis: </b>The missingness of <code>'avg_rating'</code> is related to the recipe time.</p>
<p><b>Test Statistic: </b>The absolute difference in recipe time between rows missing and not missing <code>'avg_rating'</code>.</p>
<p><b>Significance Level: </b>0.01</p>

The observed test statistic between recipes with and without a rating was approximately 117.34.

To see if my test statistic was significant, I conducted a permutation test and ran 1,000 simulations to generate an empirical distribution of the test statistic under the null hypothesis. The plot below visualizes the empirical distribution found:

<iframe
  src="plots/missingness-minutes-hist.html"
  width="750"
  height="550"
  frameborder="0"
></iframe>

The p-value I calculated was 0.041, which is greater than the significance level of 0.01. Thus, we fail to reject the null hypothesis, which implies that the values of <code>minutes</code> does not have a correlation with the missingness of <code>avg_rating</code>.

So far, the missingness of <code>avg_rating</code> has not been proven to be MAR yet. However, let's test another variable: <code>n_tags</code>.

---

<p><b>Null Hypothesis: </b>The missingness of <code>'avg_rating'</code> is not related to the number of tags.</p>
<p><b>Alternate Hypothesis: </b>The missingness of <code>'avg_rating'</code> is related to the number of tags.</p>
<p><b>Test Statistic: </b>The absolute difference in the number of tags between rows missing and not missing <code>'avg_rating'</code>.</p>
<p><b>Significance Level: </b>0.01</p>

The observed test statistic between recipes with and without a rating was approximately 0.94.

Again, to see if my test statistic was significant, I conducted a permutation test and ran 1,000 simulations to generate an empirical distribution of the test statistic under the null hypothesis. The plot below visualizes the empirical distribution found:

<iframe
  src="plots/missingness-tags-hist.html"
  width="750"
  height="550"
  frameborder="0"
></iframe>

The p-value I calculated was 0, which is less than the significance level of 0.01. Thus, we are able to reject the null hypothesis. This outcome strongly suggests that the values of <code>n_tags</code> does have a correlation with the missingness of <code>avg_rating</code>.

### <strong>Hypothesis Testing</strong>
This hypothesis test will aim to answer to our original question proposed in the introduction: are tags representative of the content they are on?

The tag that we will use is <code>'dietary'</code>. We previously saw in our "Interesting Aggregates" that there seemed to be a difference between the nutritional values between recipes with and without the tag. Logically, if one wants to go on a diet, they would generally want to eat less calories. Thus, this hypothesis test will see if the difference in calories in the two groups is significant or not.

For our hypothesis test, we will be running a permutation test since we want to compare whether recipes with and without the <code>dietary</code> tag have similar distributions and populations.

For our test statistic, I will use the absolute difference in mean calories. The absolute difference in means is useful because we have two groups, each with a quantitative mean that looks to be different from the other group. By comparing the simulated and observed test statistics, we can see if our observation is significant or not.

<p><b>Null Hypothesis: </b>Recipes with the <code>dietary</code> tag have the same amount of calories as recipes without the <code>dietary</code> tag.</p>
<p><b>Alternate Hypothesis: </b>Recipes with the <code>dietary</code> tag have less calories as recipes without the <code>dietary</code> tag</p>
<p><b>Test Statistic: </b>Absolute difference of mean calories between recipes with and without the <code>dietary</code> tag</p>
<p><b>Significance Level: </b>0.01</p>

The observed test statistic between recipes with and without <code>'dietary'</code> was approximately 34.1.

To see if my test statistic was significant, I ran 1,000 simulations to generate an empirical distribution of the test statistic under the null hypothesis. The plot below visualizes the empirical distribution found:

<iframe
  src="plots/hypothesis-test-hist.html"
  width="750"
  height="550"
  frameborder="0"
></iframe>

The p-value I calculated was 0, which is less than the significance level of 0.01. Thus, we are able to reject the null hypothesis. This outcome strongly suggests that the dietary tag is representative of dietary calorie levels.

### <strong>Framing a Prediction Problem</strong>
From the perspective of the person posting the recipe, it would be very useful to know if certain tags are representative of their recipe, as to not mislead their audience, but at the same time, it would be bad if the missing tag costed the person million of views, potentially. 

Because this investigation has found distinct characteristics of the dietary tag,  this suggests that there will be features in our dataset that can predict whether or not a given recipe should have the dietary tag. Thus, the model will be predicting whether or not a recipe should have the dietary tag.

This model will be performing binary classification: 0 if the model predicts that the recipe should not have the <code>'dietary'</code> tag and 1 if the model predicts that the recipe should have the <code>'dietary'</code> tag. To perform this classification, a random forest classifier will be used.

To evaluate the performance of the model, f1 score will be utilized. Although accuracy is useful to just determine whether the predictions are correct, the original purpose of creating this model was to figure out whether tags were representative of their recipe, yet also get as many views as possible. As a result, we want to minimize both false positives and false negatives, so f1 score is the best metric to measure this.

From the perspective of the person posting the recipe, the only information that we would know are the recipe details. This would include ingredients, nutrients, estimated time, steps, etc. The model will be using features from this pool.

### <strong>Baseline Model</strong>
The features the baseline model will use are: 


### <strong>Final Model</strong>
### <strong>Fairness Analysis</strong>
