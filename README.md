
# Investigating the Significance of Recipe Tags
<p style="text-align:center;">a DSC 80 project — Nicolas Toon</p>

### <strong>Introduction</strong>
Food is undoubtedly a significant part of human lives. We eat food to survive, yet it can be a significant detriment to our lives with too much consumption, as seen by the obesity epidemic in the United States. One solution to controlling consumption is cooking, so that the amount of ingredients and macronutrients can be controlled. Online recipes can be a great method to find something simple and quick to prep, yet healthy to eat.

Similar to social media, most online recipes contain tags that help expand their outreach. The question I pose and strive to answer is: do more tags on a post lead to higher ratings?

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

### <b>Assessment of Missingness</b>
### <b>Hypothesis Testing</b>
### <b>Framing a Prediction Problem</b>
### <b>Baseline Model</b>
### <b>Final Model</b>
### <b>Fairness Analysis</b>
