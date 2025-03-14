
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

### <b>Data Cleaning and Exploratory Data Analysis</b>
#### Data Cleaning
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
<table border="1" class="dataframe">\n  <thead>\n    <tr style="text-align: right;">\n      <th>name</th>\n      <th>id</th>\n      <th>minutes</th>\n      <th>contributor_id</th>\n      <th>submitted</th>\n      <th>tags</th>\n      <th>nutrition</th>\n      <th>n_steps</th>\n      <th>steps</th>\n      <th>description</th>\n      <th>ingredients</th>\n      <th>n_ingredients</th>\n      <th>avg_rating</th>\n      <th>n_ratings</th>\n      <th>n_tags</th>\n      <th>year</th>\n    </tr>\n  </thead>\n  <tbody>\n    <tr>\n      <td>1 brownies in the world    best ever</td>\n      <td>333281</td>\n      <td>40.0</td>\n      <td>985201</td>\n      <td>2008-10-27</td>\n      <td>[60-minutes-or-less, time-to-make, course, main-ingredient, preparation, for-large-groups, desserts, lunch, snacks, cookies-and-brownies, chocolate, bar-cookies, brownies, number-of-servings]</td>\n      <td>[138.4, 10.0, 50.0, 3.0, 3.0, 19.0, 6.0]</td>\n      <td>10</td>\n      <td>[\'heat the oven to 350f and arrange the rack in the middle\', \'line an 8-by-8-inch glass baking dish with aluminum foil\', \'combine chocolate and butter in a medium saucepan and cook over medium-low heat , stirring frequently , until evenly melted\', \'remove from heat and let cool to room temperature\', \'combine eggs , sugar , cocoa powder , vanilla extract , espresso , and salt in a large bowl and briefly stir until just evenly incorporated\', \'add cooled chocolate and mix until uniform in color\', \'add flour and stir until just incorporated\', \'transfer batter to the prepared baking dish\', \'bake until a tester inserted in the center of the brownies comes out clean , about 25 to 30 minutes\', \'remove from the oven and cool completely before cutting\']</td>\n      <td>these are the most; chocolatey, moist, rich, dense, fudgy, delicious brownies that you\'ll ever make.....sereiously! there\'s no doubt that these will be your fav brownies ever for you can add things to them or make them plain.....either way they\'re pure heaven!</td>\n      <td>[bittersweet chocolate, unsalted butter, eggs, granulated sugar, unsweetened cocoa powder, vanilla extract, brewed espresso, kosher salt, all-purpose flour]</td>\n      <td>9</td>\n      <td>4.0</td>\n      <td>1</td>\n      <td>14</td>\n      <td>2008</td>\n    </tr>\n    <tr>\n      <td>1 in canada chocolate chip cookies</td>\n      <td>453467</td>\n      <td>45.0</td>\n      <td>1848091</td>\n      <td>2011-04-11</td>\n      <td>[60-minutes-or-less, time-to-make, cuisine, preparation, north-american, for-large-groups, canadian, british-columbian, number-of-servings]</td>\n      <td>[595.1, 46.0, 211.0, 22.0, 13.0, 51.0, 26.0]</td>\n      <td>12</td>\n      <td>[\'pre-heat oven the 350 degrees f\', \'in a mixing bowl , sift together the flours and baking powder\', \'set aside\', \'in another mixing bowl , blend together the sugars , margarine , and salt until light and fluffy\', \'add the eggs , water , and vanilla to the margarine / sugar mixture and mix together until well combined\', \'add in the flour mixture to the wet ingredients and blend until combined\', \'scrape down the sides of the bowl and add the chocolate chips\', \'mix until combined\', \'scrape down the sides to the bowl again\', \'using an ice cream scoop , scoop evenly rounded balls of dough and place of cookie sheet about 1 - 2 inches apart to allow for spreading during baking\', \'bake for 10 - 15 minutes or until golden brown on the outside and soft &amp; chewy in the center\', \'serve hot and enjoy !\']</td>\n      <td>this is the recipe that we use at my school cafeteria for chocolate chip cookies. they must be the best chocolate chip cookies i have ever had! if you don\'t have margarine or don\'t like it, then just use butter (softened) instead.</td>\n      <td>[white sugar, brown sugar, salt, margarine, eggs, vanilla, water, all-purpose flour, whole wheat flour, baking soda, chocolate chips]</td>\n      <td>11</td>\n      <td>5.0</td>\n      <td>1</td>\n      <td>9</td>\n      <td>2011</td>\n    </tr>\n    <tr>\n      <td>412 broccoli casserole</td>\n      <td>306168</td>\n      <td>40.0</td>\n      <td>50969</td>\n      <td>2008-05-30</td>\n      <td>[60-minutes-or-less, time-to-make, course, main-ingredient, preparation, side-dishes, vegetables, easy, beginner-cook, broccoli]</td>\n      <td>[194.8, 20.0, 6.0, 32.0, 22.0, 36.0, 3.0]</td>\n      <td>6</td>\n      <td>[\'preheat oven to 350 degrees\', \'spray a 2 quart baking dish with cooking spray , set aside\', \'in a large bowl mix together broccoli , soup , one cup of cheese , garlic powder , pepper , salt , milk , 1 cup of french onions , and soy sauce\', \'pour into baking dish , sprinkle remaining cheese over top\', \'bake for 25 minutes or until cheese is lightly browned\', \'sprinkle with rest of french fried onions and bake until onions are browned and cheese is bubbly , about 10 more minutes\']</td>\n      <td>since there are already 411 recipes for broccoli casserole posted to "zaar" ,i decided to call this one  #412 broccoli casserole.i don\'t think there are any like this one in the database. i based this one on the famous "green bean casserole" from campbell\'s soup. but i think mine is better since i don\'t like cream of mushroom soup.submitted to "zaar" on may 28th,2008</td>\n      <td>[frozen broccoli cuts, cream of chicken soup, sharp cheddar cheese, garlic powder, ground black pepper, salt, milk, soy sauce, french-fried onions]</td>\n      <td>9</td>\n      <td>5.0</td>\n      <td>4</td>\n      <td>10</td>\n      <td>2008</td>\n    </tr>\n    <tr>\n      <td>millionaire pound cake</td>\n      <td>286009</td>\n      <td>120.0</td>\n      <td>461724</td>\n      <td>2008-02-12</td>\n      <td>[time-to-make, course, cuisine, preparation, occasion, north-american, desserts, american, southern-united-states, dinner-party, holiday-event, cakes, dietary, christmas, thanksgiving, low-sodium, low-in-something, taste-mood, sweet, 4-hours-or-less]</td>\n      <td>[878.3, 63.0, 326.0, 13.0, 20.0, 123.0, 39.0]</td>\n      <td>7</td>\n      <td>[\'freheat the oven to 300 degrees\', \'grease a 10-inch tube pan with butter , dust the bottom and sides with flour , and set aside\', \'in a large mixing bowl , cream the butter and sugar with an electric mixer and add the eggs one at a time , beating after each addition\', \'alternately add the flour and milk , stirring till the batter is smooth\', \'add the two extracts and stir till well blended\', \'scrape the batter into the prepared pan and bake till a cake tester or knife blade inserted in the center comes out clean , about 1 1 / 2 hours\', \'cool the cake in the pan on a rack for 5 minutes , then turn it out on the rack to cool completely\']</td>\n      <td>why a millionaire pound cake?  because it\'s super rich!  this scrumptious cake is the pride of an elderly belle from jackson, mississippi.  the recipe comes from "the glory of southern cooking" by james villas.</td>\n      <td>[butter, sugar, eggs, all-purpose flour, whole milk, pure vanilla extract, almond extract]</td>\n      <td>7</td>\n      <td>5.0</td>\n      <td>1</td>\n      <td>20</td>\n      <td>2008</td>\n    </tr>\n    <tr>\n      <td>2000 meatloaf</td>\n      <td>475785</td>\n      <td>90.0</td>\n      <td>2202916</td>\n      <td>2012-03-06</td>\n      <td>[time-to-make, course, main-ingredient, preparation, main-dish, potatoes, vegetables, 4-hours-or-less, meatloaf, simply-potatoes2]</td>\n      <td>[267.0, 30.0, 12.0, 12.0, 29.0, 48.0, 2.0]</td>\n      <td>17</td>\n      <td>[\'pan fry bacon , and set aside on a paper towel to absorb excess grease\', \'mince yellow onion , red bell pepper , and add to your mixing bowl\', \'chop garlic and set aside\', \'put 1tbsp olive oil into a saut pan , along with chopped garlic , teaspoons white pepper and a pinch of kosher salt\', \'bring to a medium heat to sweat your garlic\', \'preheat oven to 350f\', \'coarsely chop your baby spinach add to your heated pan , stir frequently for approximately 5 min to wilt\', \'add your spinach to the mixing bowl\', \'chop your now cooled bacon , and add it to the mixing bowl\', \'add your meatloaf mix to the bowl , with one egg and mix till thoroughly combined\', \'add your goat cheese , one egg , 1 / 8 tsp white pepper and 1 / 8 tsp of kosher salt and mix till thoroughly combined\', \'transfer to a 9x5 meatloaf pan , and cook for 60 min or until the internal temperature is at least 160f\', \'let stand for 5min\', \'melt 1tbsp unsalted butter into a frying pan , and cook up to three eggs at a time\', \'crack each egg into a separate dish , in order to prevent egg shells from reaching the pan , then add salt and pepper to taste\', \'wait until the egg whites are firm looking , but slightly runny on top before flipping your eggs\', \'after flipping , wait 10~20 seconds before removing each egg and placing it over your slices of meatloaf\']</td>\n      <td>ready, set, cook! special edition contest entry: a mediterranean flavor inspired meatloaf dish. featuring: simply potatoes - shredded hash browns, egg, bacon, spinach, red bell pepper, and goat cheese.</td>\n      <td>[meatloaf mixture, unsmoked bacon, goat cheese, unsalted butter, eggs, baby spinach, yellow onion, red bell pepper, simply potatoes shredded hash browns, fresh garlic, kosher salt, white pepper, olive oil]</td>\n      <td>13</td>\n      <td>5.0</td>\n      <td>2</td>\n      <td>10</td>\n      <td>2012</td>\n    </tr>\n  </tbody>\n</table>

### <b>Assessment of Missingness</b>
### <b>Hypothesis Testing</b>
### <b>Framing a Prediction Problem</b>
### <b>Baseline Model</b>
### <b>Final Model</b>
### <b>Fairness Analysis</b>
