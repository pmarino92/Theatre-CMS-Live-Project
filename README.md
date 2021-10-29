
# Theatre-CMS-Live-Project <a id="top"></a>
Live Project with a team of members using C#, JavaScript, HTML/CSS, Visual Studio and Agile methodologies.
# Introduction 
For my C# live project at The Tech Academy, I worked with a development team of my peers on a CMS (Content Management System) for a local theatre here in Portland, Oregon. The CMS itself was made with ASP.Net MVC and Entity Framework. I personally worked on the Rental Area portion of the CMS where I employed Bootstrap coupled with CSS for the styling and added JavaScript/C# for the functionality.
# Front-end Stories
* [Styling of Index Page](#index-page)
* [](#)
* [](#)

# Back-end Stories
* [Entity Model](#entity-model)
* [](#)


# Entity Model-Rental History <a id="entity-model"></a>
For my first story I was assigned the task of creating an entity model for Rental History. Here is the code I came up with:
```
public class RentalHistory
{
    public int Id { get; set; }
    public bool RentalDamaged  { get; set; }
    public string DamagesIncurred { get; set; }
    public string Rental { get; set; }
}
```
From there I was able to use the pre-existing DB context and scaffold the corresponding CRUD pages relatively easily.


# Styling of the Index Page <a id="index-page"></a>
After creating my model, the next step was styling the index page. To do this I employed a number of resources which included: Bootstrap, Font Awesome, and of course CSS. This story was more difficult than I anticipated with getting everything to work accordingly, however, I am proud of the outcome.

[![index.png](https://i.postimg.cc/SRTGtf4m/index.png)](https://postimg.cc/0KSmzmr4)

Features I was able to implement:
- If a rental was damaged a font aweseom red X would appear to the left and if the rental was not damaged a green check would be present. 
- If the rental damage description (third column) was too long a horizontal ellipsis would shorten the description for readability's sake.
- A vertical ellipsis containing Create, Edit, Delete functions appears upon hovering any of the rows.


```
<head>
    <meta charset="UTF-8">
    <title>{% block title %}{% endblock %}</title>
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
    <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/4.0.0/css/bootstrap.min.css" integrity="sha384-Gn5384xqQ1aoWXA+058RXPxPg6fy4IWvTNh0E263XmFcJlSAwiGgFAW/dAiS6JXm" crossorigin="anonymous">
    <link rel="stylesheet" href="https://unpkg.com/bootstrap-table@1.18.3/dist/bootstrap-table.min.css">
    <link rel="stylesheet" href="{% static 'css/games.css' %}">
</head>
<body>
    <!-- NAV BAR -->
    <nav>
        <ul>
            <li><a href="{% url 'BestGamesEver_Home' %}">Home</a></li>
            <li><a href="{% url 'Game_Create' %}">Add Game</a></li>
            <li><a href="{% url 'Game_View' %}">View Games</a></li>
            <li><a href="{% url 'View_Price' %}">View Prices</a></li>
            
        </ul>
    </nav>
    
   ```
# Models and Database Functionality <a id="edit-and-delete-functions"></a>
I was able to create a model named "Game" which allowed storage within my SQLite3 database. This allowed the user to input several fields which included: name of the game, year it was released, genre, and a description of the game. Furthermore, I allowed for edits and deletion of entries through the use of model forms and instances in my views.py file that displayed the content of a single item from the database as well as a deletion confirmation alert to the user.

```
class Game(models.Model):
    name = models.CharField(max_length=60)
    genre = models.CharField(max_length=60)
    year = models.IntegerField(default='2000')
    description = models.TextField(max_length=200)

    objects = models.Manager()


    def __str__(self):
        return self.name
```

```
def Edit_Games(request, game_id):
    item = get_object_or_404(Game, id=game_id)
    form = GameForm(data=request.POST or None, instance=item)
    if request.method == 'POST':
        if form.is_valid():
            form.save()
            return redirect("Game_View")
    content = {'form': form}
    return render(request, 'BestGamesEver/Game_Edit.html', content)


# Function to delete an entry
def Delete_Games(request, game_id):
    item = get_object_or_404(Game, id=game_id)
    form = GameForm(data=request.POST or None, instance=item)
    if request.method == 'POST':
            item.delete()
            return redirect("Game_View")
    content = {'form': form}
    return render(request, 'BestGamesEver/Game_Delete.html', {'item': item, 'form': form})
 ```
 
 # Beautiful Soup <a id="beautiful-soup"></a>
 I employed web scraping with the use of Beautiful Soup to scrape data from Amazon to give current video game prices. While this was one of the more challenging aspects of my project, I was able to print out the video game title as well as the price in dictionary form through the use of two functions. The first function (get_html_content) would scrape the specific game's current price from amazon which would pass the information to the second function(View_Price) to display the content to the user.
 
 ```
 def get_html_content(game):
    USER_AGENT = "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/44.0.2403.157 Safari/537.36"
    LANGUAGE = "en-US,en;q=0.5"
    session = requests.Session()
    session.headers['User-Agent'] = USER_AGENT
    session.headers['Accept-Language'] = LANGUAGE
    session.headers['Content-Language'] = LANGUAGE
    game = game.replace(' ', '+')
    html_content = session.get(f'https://www.amazon.com/s?k={game}&ref=nb_sb_noss_2').text
    return html_content


# Beautiful Soup Function
def View_Price(request):
    game_data = None
    if 'game' in request.GET:
        # Fetch game data
        game = request.GET.get('game')
        html_content = get_html_content(game)
        soup = BeautifulSoup(html_content, 'html.parser')
        # Setting our variable to dictionary form
        game_data = {}
        # Will display name of the game and current price
        game_data['title'] = soup.find('span', attrs={'class': 'a-size-medium a-color-base a-text-normal'}).text
        game_data['price'] = soup.find('span', attrs={'class': 'a-offscreen'}).text

    return render(request, 'BestGamesEver/View_Price.html', {'game': game_data})
 
 ```
 
 
 *Jump to [Bootstrap for styling](#bootstrap), [Navigation Bar](#navigation-bar), [Edit and Delete User Inputs](#edit-and-delete-functions), [Top](#top)


