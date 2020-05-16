# Live Project

#### During a two week sprint I worked on a team of developers utilizing agile software development. I acquired front-end and back-end development skills by utilizing Django to build a web application that leveraged a database and data scraping.

### Edit & Save Function

I created a view function that gets an item from the database for editing and saves the changes when a user submits them back. The function uses an if statement to decide whether to display the item or save the item. It checks if there were any validation errors and redirects to the details page when saved.

```python
# View function to edit a residency
def edit_residency(request, pk):
    global form
    pk = int(pk)  #casts value of pk to an int so it's in the proper form
    residency = get_object_or_404(Residency, pk=pk)  #gets single instance of the residency from the database
    if request.method =='POST':
        form = ResidencyForm(request.POST, instance=residency)
        if form.is_valid(): #validation
            residency = form.save()
            return redirect('residencyDetails', pk=residency.pk) #redirects to details page
    else:
        form = ResidencyForm(instance=residency)
    return render(request, 'ArtApp/art_app_edit.html', {'form' : form}) #renders to the edit template using the context and request
```

### Delete Function

This is a function that deletes an item in the database or displays a confirmation page to confirm that the item should be deleted.

```python
# View function to delete a residency to the database
def delete_residency(request, pk):
    residency = get_object_or_404(Residency, pk=pk)
    if request.method == 'POST':
        #delete confirmation
        residency.delete()
        return redirect('artApp')  #redirects to the home page, which is named 'artapp' in the urls
    context = {'residency': residency}
    return render(request, 'ArtApp/art_app_delete.html', context)
```

### Data Scraping

I created a function to scrape data from a website. This function leverages Beautiful Soup to create a navigatable object. It sends the targeted objects to the template where their data is displayed. 

```python
# View function to scrape data from AAC webpage
def external (request):
    page = requests.get("https://www.artistcommunities.org/residencies/upcoming-deadlines")
    print(page.status_code)   #prints status code in terminal
    soup = BeautifulSoup(page.content,'html.parser')   #creates beautiful soup object
    all_title_tags = soup.find_all('div', class_='views-field-title')   #locates specific class
    residencies = [] #array
    for i in range(0, 5):
        title = all_title_tags[i].span.a.string
        href = 'https://www.artistcommunities.org' + all_title_tags[i].span.a['href']
        residencies.append({   #adds object to the array
            'name': title,
            'link': href
        })
    context = {'residencies': residencies}
    return render(request, 'ArtApp/art_app_external.html', context)
```

### Create Function

I made a views function to create a new item in the database. It gets the posted form, checks for errors and saves to the database.

```python
# View function to add a new residency to the database
def add_residency(request):
    form = ResidencyForm(request.POST or None)  # Gets the posted form, if one exists
    if form.is_valid():  # Checks the form for errors, ensures it's filled in
        form.save()  # Saves the valid form/residency to the database
        return redirect('artApp')  # Redirects to the home page, which is named 'artapp' in the urls
    else:
        print(form.errors)  # Prints any errors for the posted form to the terminal
        # form = ResidencyForm()                     #Creates a new blank form
    return render(request, 'ArtApp/art_app_create.html', {'form': form})
```
### Front End

I utilized Bootstrap to created a navigation bar in the base template.
```html
<nav class="navbar navbar-expand-sm navbar-light bg-light">
    <div class="container">
        <a class="navbar-brand" href="{% url 'artApp' %}">Art App</a>
        <div class="collapse navbar-collapse" id="navbarNav">
            <ul class="navbar-nav">
                <li class="nav-item">
                    <a class="nav-link" href="{% url 'createResidency' %}">Create Residency</a>
                </li>
                <li class="nav-item">
                    <a class="nav-link" href="{% url 'listResidencies' %}">Residencies</a>
                </li>
                <li class="nav-item">
                    <a class="nav-link" href="{% url 'external' %}">External Residencies</a>
                </li>
                 <li class="nav-item">
                    <a class="nav-link" href="{% url 'home' %}">Hobby Apps</a>
                </li>
            </ul>
        </div>
    </div>
</nav>
```

### Skills

*  Experience working on a Sprint with a team of developers in an Agile framework.
*  Balance of front-end and back-end skills to create a functional application with clean code.
*  Working in a team environment and contributing to the project through daily stand ups, reports, and code retrospectives.
*  Experience utilizing Django, SQL Lite, React, Python, Beautiful Soup, Bootstrap, Javascript and JSON.
*  Organization and completion of a series of stories and project time management.

The Tech Academy Live Project provided me with real-life experience working on a team of software developers. I enjoyed working in an iterative and flexible framework, which prepared me for a professional web development position. It was great to see how each developer brought solutions to their stories, creating a learning experience for all while producing a functional app in a brief amount of time.

 During this time I gained confidence working in an agile framework with PyCharm, Django, Bootstrap, Beautiful Soup, Git, Azure, and more. I developed new problem-solving skills, research abilities and learned how to balance technical and design concerns. 

