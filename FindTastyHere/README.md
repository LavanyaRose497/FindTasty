# FoodSite
# Reference - Learn Django from scratch by Ashutosh Pawar (Udemy)

# Steps to setup venv:
1. python -m venv path_to_venv
2. Navigate to Scripts folder and run activate.bat
3. Install the required packages
4. To freeze the packages: pip freeze > reqs.txt
5. To install packages from reqs.txt: pip install -r reqs.txt
6. deactivate the venv by: deactivate.bat inside Scripts folder

# Steps to setup a django project and start on local server:
1.  pip install django
2. django-admin startproject project_name(FoodSite) -> this creates all the initial setup files and folders required for a django project
3. manage.py -> helps to interact with the files of the project
4. __init__.py -> tell Python that its parent folder is a package and can be reused
5. settings.py -> contains all the settings and configurations of the current project
6. urls.py -> contains all URL patterns that should match with incoming requests for the project and returns with results
7. To run the app in local server: Navigate to folder where manage.py resides and run the command: python manage.py runserver

# Steps to create an app:
1. A Django project can contain multiple apps(sections)
2. Navigate inside the project folder (eg. FoodSite/) -> run: python manage.py startapp app_name(food)
3. Each app contains a views.py -> which processes the user request and sends back an appropriate response
4. After each app is created, it should be added to list of 'installed apps' in settings.py file

# Steps to connect the app view with URL Patterns:
* This root url config is done in 'settings.py' file and generally points to mainapp/urls.py
1. create a urls.py inside the app and define a view in the same app
2. In the app/urls.py, import the app/views.py and define the urlpatterns as:
    from django.urls import path
    urlpatterns = [
        path('home/', views.home, name='home'),
        ]
3. To connect the app/urls.py to mainapp/urls.py:
    from django.urls import path, include
    urlpatterns = [
        path('admin', admin.site.urls),
        path('food/', include('food.urls')),
    ]
    Now, you can reach the home view by 'http:127.0.0.1:8000/food/home/'
    Thus, mainapp urlpattern comes first, followed by currentapp urlpattern

# Steps on Models and Databases:
1. Models are the blueprints/schemas that allow us to create the database tables
2. Identical to classes in Python
3. Unapplied migrations: This denotes that the list of 'installed apps' in settings.py file has one or more apps that are requires/using database tables whereas it is not created yet. On running 'python manage.py migrate', those changes are reflected/the tables are created in the database
4. Example for 'Food' model:
    from django.db import models
    class Item(models.Model):
        item_name = models.CharField(max_length=200)
        item_desc = models.CharField(max_length=300)
        item_price = models.IntegerField()
5. Migrate command: This helps to create the db table from the defined models. This checks the 'installed apps' in settings.py file and creates the db tables for those apps
6. Creating db tables for apps: Check the apps.py file of current app ie.food and get the class name ie. FoodConfig. Add this into the list of 'installed apps' as 'food.apps.FoodConfig' ie.appname.apps.classname. Run the 'python manage.py makemigrations appname' command to let know Django that some changes are made in models. Followed by this, run 'python manage.py sqlmigrate appname 0001' which executes the 'create table' SQL command. Run 'python manage.py migrate' to make the final changes reflect in the db

# Steps to add data to DB tables:
1. Django uses DATABASE ABSTRACTION API to perform CRUD operations on objects(instances of Item class)
2. We can use Python Shell to interact with the DB model which can be invoked by 'python manage.py shell'
3. Example:
    from food.models import Item -> importing the model
    Item.objects.all() -> lists all objects in the model
    a = Item(item_name="Pizza", item_desc="Cheesy Pizza", item_price=150) -> creating an object of Item model
    a.save() -> adds the object to the Item model/table
    a.id, a.item_name -> helps to access the current object details
    a.id, a.pk -> to get the default-created id/primary key of the current object
4. On retrieving the objects, we get the output as 'Item object 1 and 2'. To fix this with item_name, go the models.py and define __str__(self) method that returns the desired output.
    def __str__(self):
        return self.item_name

# Django Admin and Superuser
1. 
