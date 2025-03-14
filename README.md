
# Investigating the Significance of Recipe Tags
<p style="text-align:center;">a DSC 80 project</p>
<p style="text-align:center;">Nicolas Toon</p>

### Introduction
Food is undoubtedly a significant part of human lives. We eat food to survive, yet it can be a significant detriment to our lives with too much consumption, as seen by the obesity epidemic in the United States. One solution to controlling consumption is cooking, so that the amount of ingredients and macronutrients can be controlled. Online recipes can be a great method to find something simple and quick to prep, yet healthy to eat.

Similar to social media, most online recipes contain tags that help expand their outreach. The question I pose and strive to answer is: do more tags on a post lead to higher ratings?

In this project, I examined two datasets originally scraped by <a href='https://cseweb.ucsd.edu/~jmcauley/pdfs/emnlp19c.pdf'>Majumder, et al.</a>, which compiled recipe details and reviews posted on <a href='https://www.food.com/'>food.com</a> from 2008 to 2018.

The first dataset contains 83,782 rows, with each row denoting a unique recipe. Each unique recipe has 12 features:
<table>
  <tr>
    <th>Feature</th>
    <th>Description</th>
  </tr>
  <tr>
    <td>`'name'`</td>
    <td>recipe name</td>
  </tr>
  <tr>
    <td>`'id'`</td>
    <td>unique identifier</td>
  </tr>
  <tr>
    <td>`'minutes'`</td>
    <td>estimated time</td>
  </tr>
  <tr>
    <td>`'contributor_id'`</td>
    <td>unique identifier for user who posted the recipe</td>
  </tr>
  <tr>
    <td>`'submitted'`</td>
    <td>posting date</td>
  </tr>
  <tr>
    <td>`'tags'`</td>
    <td>recipe tags</td>
  </tr>
  <tr>
    <td>`'nutrition'`</td>
    <td>nutritional information</td>
  </tr>
  <tr>
    <td>`'n_steps'`</td>
    <td>number of steps</td>
  </tr>
  <tr>
    <td>`'steps'`</td>
    <td>steps for recipe, in order</td>
  </tr>
  <tr>
    <td>`'description'`</td>
    <td>user generated description</td>
  </tr>
  <tr>
    <td>`'ingredients'`</td>
    <td>ingredients used</td>
  </tr>
  <tr>
    <td>`'n_ingredients'`</td>
    <td>number of ingredients</td>
  </tr>
</table>

The second dataset contains 731,927 rows, with each row denoting a unique review. Each unique review has 5 features:
<table>
  <tr>
    <th>Feature</th>
    <th>Description</th>
  </tr>
  <tr>
    <td>`'user_id'`</td>
    <td>unique identifier for reviewer</td>
  </tr>
  <tr>
    <td>`'recipe_id'`</td>
    <td>unique identifier for the recipe the review is about</td>
  </tr>
  <tr>
    <td>`'date'`</td>
    <td>review date</td>
  </tr>
  <tr>
    <td>`'rating'`</td>
    <td>rating given</td>
  </tr>
  <tr>
    <td>`'review'`</td>
    <td>review/comment text</td>
  </tr>
</table>

### Data Cleaning and Exploratory Data Analysis
### Assessment of Missingness
### Hypothesis Testing
### Framing a Prediction Problem
### Baseline Model
### Final Model
### Fairness Analysis
