
# Investigating the Significance of Recipe Tags
<p style="text-align:center;">a DSC 80 project — Nicolas Toon</p>

### <b>Introduction</b>
Food is undoubtedly a significant part of human lives. We eat food to survive, yet it can be a significant detriment to our lives with too much consumption, as seen by the obesity epidemic in the United States. One solution to controlling consumption is cooking, so that the amount of ingredients and macronutrients can be controlled. Online recipes can be a great method to find something simple and quick to prep, yet healthy to eat.

Similar to social media, most online recipes contain tags that help expand their outreach. The question I pose and strive to answer is: do more tags on a post lead to higher ratings?

In this project, I examined two datasets originally scraped by <a href='https://cseweb.ucsd.edu/~jmcauley/pdfs/emnlp19c.pdf'>Majumder, et al.</a>, which compiled recipe details and reviews posted on <a href='https://www.food.com/'>food.com</a> from 2008 to 2018.

The first dataset, <code>recipes</code>, contains 83,782 rows, with each row denoting a unique recipe. Each unique recipe has 12 features:
<table>
  <tr>
    <th>Feature</th>
    <th>Description</th>
  </tr>
  <tr>
    <td><code>'name'</code></td>
    <td>recipe name</td>
  </tr>
  <tr>
    <td><code>'id'</code></td>
    <td>unique identifier</td>
  </tr>
  <tr>
    <td><code>'minutes'</code></td>
    <td>estimated time</td>
  </tr>
  <tr>
    <td><code>'contributor_id'</code></td>
    <td>unique identifier for user who posted the recipe</td>
  </tr>
  <tr>
    <td><code>'submitted'</code></td>
    <td>posting date</td>
  </tr>
  <tr>
    <td><code>'tags'</code></td>
    <td>recipe tags</td>
  </tr>
  <tr>
    <td><code>'nutrition'</code></td>
    <td>nutritional information</td>
  </tr>
  <tr>
    <td><code>'n_steps'</code></td>
    <td>number of steps</td>
  </tr>
  <tr>
    <td><code>'steps'</code></td>
    <td>steps for recipe, in order</td>
  </tr>
  <tr>
    <td><code>'description'</code></td>
    <td>user generated description</td>
  </tr>
  <tr>
    <td><code>'ingredients'</code></td>
    <td>ingredients used</td>
  </tr>
  <tr>
    <td><code>'n_ingredients'</code></td>
    <td>number of ingredients</td>
  </tr>
</table>

The second dataset, <code>interactions</code>, contains 731,927 rows, with each row denoting a unique review. Each unique review has 5 features:
<table>
  <tr>
    <th>Feature</th>
    <th>Description</th>
  </tr>
  <tr>
    <td><code>'user_id'</code></td>
    <td>unique identifier for reviewer</td>
  </tr>
  <tr>
    <td><code>'recipe_id'</code></td>
    <td>unique identifier for the recipe the review is about</td>
  </tr>
  <tr>
    <td><code>'date'</code></td>
    <td>review date</td>
  </tr>
  <tr>
    <td><code>'rating'</code></td>
    <td>rating given</td>
  </tr>
  <tr>
    <td><code>'review'</code></td>
    <td>review/comment text</td>
  </tr>
</table>

### <b>Data Cleaning and Exploratory Data Analysis</b>
#### Data Cleaning
To make the data exploration process easier, the following steps were executed, in order:
<ol type=1>
  <li>Left merging the <code>recipes</code> and <code>interactions</code> datasets on the recipe id</li>
    <ol>
      <li>This matches each review with its corresponding recipe, making it easier to associate ratings with recipes.</li>
    </ol>
    
  <li>Replacing all zeroes with <code>np.nan</code></li>
    <ol>
      <li>Any entry of 0 does not make sense for each of the features, so 0 can be assumed as "missing values". Thus, we can replace all zeroes so that they are not included in data analysis.</li>
    </ol>

  <li>Replacing all zeroes with <code>np.nan</code></li>
    <ol>
      <li>Any entry of 0 does not make sense for each of the features, so 0 can be assumed as "missing values". Thus, we can replace all zeroes so that they are not included in data analysis.</li>
    </ol>
  <li>Left merging the <code>recipes</code> and <code>interactions</code></li> datasets on the recipe id
    <ol>
      <li>This matches each review with its corresponding recipe, making it easier to associate ratings with recipes.</li>
    </ol>
  <li>Replacing all zeroes with <code>np.nan</code></li>
    <ol>
      <li>Any entry of 0 does not make sense for each of the features, so 0 can be assumed as "missing values". Thus, we can replace all zeroes so that they are not included in data analysis.</li>
    </ol>
  <li>Replacing all zeroes with <code>np.nan</code></li>
    <ol>
      <li>Any entry of 0 does not make sense for each of the features, so 0 can be assumed as "missing values". Thus, we can replace all zeroes so that they are not included in data analysis.</li>
    </ol>
  <li>Replacing all zeroes with <code>np.nan</code></li>
    <ol>
      <li>Any entry of 0 does not make sense for each of the features, so 0 can be assumed as "missing values". Thus, we can replace all zeroes so that they are not included in data analysis.</li>
    </ol>
  <li>Left merging the <code>recipes</code> and <code>interactions</code> datasets on the recipe id</li>
    <ol>
      <li>This matches each review with its corresponding recipe, making it easier to associate ratings with recipes.</li>
    </ol>
  <li>Replacing all zeroes with <code>np.nan</code></li>
    <ol>
      <li>Any entry of 0 does not make sense for each of the features, so 0 can be assumed as "missing values". Thus, we can replace all zeroes so that they are not included in data analysis.</li>
    </ol>
  <li>Replacing all zeroes with <code>np.nan</code></li>
    <ol>
      <li>Any entry of 0 does not make sense for each of the features, so 0 can be assumed as "missing values". Thus, we can replace all zeroes so that they are not included in data analysis.</li>
    </ol>

</ol>

### <b>Assessment of Missingness</b>
### <b>Hypothesis Testing</b>
### <b>Framing a Prediction Problem</b>
### <b>Baseline Model</b>
### <b>Final Model</b>
### <b>Fairness Analysis</b>
