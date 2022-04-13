# How to install Python **Django REST framework** for a **REST API** project?
Make sure you have python 3 installed on your machine. This tutorial will be covered based on Ubuntu 20 Operating System. <br>

![Alt Django REST framework](/screenshots/logo.png "Django REST framework")

[Visit this link if you didn't install Python yet!](https://www.python.org/downloads/) 

* Check your Python installed successfully or not <br>

```python 
  python3 --version
```
* Update the packages <br>

```python 
  sudo apt update
```

* Install **pip** and **Python Virtual Environment**  <br>

```python 
  sudo apt install python3-pip python3-venv
```

* Create a new directory in  your machine and allow **Read Write** permission to it  <br>

```python 
  sudo mkdir test
  cd test
  sudo chmod -R 777 ./ #You can edit permission later
```


* Create your **Python Virtual Environment**, here I named it **my_env**  <br>

```python 
  python3 -m venv my_env
```

* Enable your virtual environment  <br>

```python 
  source my_env/bin/activate
```

* Install **Django**  <br>

```python 
  pip install django
```

* Check Django installed successfully or not  <br>

```python 
  django-admin --version
```

* Create a new project with Django   <br>

```python 
  
  django-admin startproject my_project .


  ls # You will see a file named manage.py
  
```

* Now, **Start the server!** <br>

```python 
  python3 manage.py runserver
```

A server will run on your machine on 8000 port. <br>
Now open your browser and copy and paste the link **http://127.0.0.1:8000/** or **http://localhost:8000** or **localhost:8000**

### If you see something like the screenshot below then **Congratulation!** You successfully installed **Python Django** on your machine. <br><br>

![Alt text](/screenshots/_django_installation.JPG "Python Django")


* Create an app <br>

```python 
  django-admin startapp my_rest_api_app
```

## Now, we have to install Django Rest Framework



Django REST framework is a powerful and flexible toolkit for building Web APIs.

[Django REST framework](https://www.django-rest-framework.org/) 

* Install the main framework <br>

```python 
  pip install djangorestframework
```



* Add **'rest_framework'** to your **INSTALLED_APPS** setting. <br>

```python 
  INSTALLED_APPS = [
    ...
    'rest_framework',
]
```

![Alt djangorestframework](/screenshots/s1.png "djangorestframework")

### And thats all! You successfully installed **Django REST Framework**.

## Preparing a REST api server :

* Sync your database for the first time: <br>

```python 
  python manage.py migrate
```

![Alt djangorestframework](/screenshots/s2.JPG "djangorestframework")

* We'll also create an initial user named admin with a password of password123. We'll authenticate as that user later in our example <br>

```python 
  python3  manage.py createsuperuser --email turzoxpress@gmail.com --username turzo
  #A password input will show after this command, use 12345678 or anything for password
```

* First up we're going to define some serializers. Let's create a new module named **my_project/my_rest_api_app/serializers.py** that we'll use for our data representations. <br>

```python 
from django.contrib.auth.models import User, Group
from rest_framework import serializers


class UserSerializer(serializers.HyperlinkedModelSerializer):
    class Meta:
        model = User
        fields = ['url', 'username', 'email', 'groups']


class GroupSerializer(serializers.HyperlinkedModelSerializer):
    class Meta:
        model = Group
        fields = ['url', 'name']
  
```

* Right, we'd better write some views then. Open **my_project/my_rest_api_app/views.py** and get typing. <br>

```python 
from django.contrib.auth.models import User, Group
from rest_framework import viewsets
from rest_framework import permissions
from my_rest_api_app.serializers import UserSerializer, GroupSerializer


class UserViewSet(viewsets.ModelViewSet):
    """
    API endpoint that allows users to be viewed or edited.
    """
    queryset = User.objects.all().order_by('-date_joined')
    serializer_class = UserSerializer
    permission_classes = [permissions.IsAuthenticated]


class GroupViewSet(viewsets.ModelViewSet):
    """
    API endpoint that allows groups to be viewed or edited.
    """
    queryset = Group.objects.all()
    serializer_class = GroupSerializer
    permission_classes = [permissions.IsAuthenticated]
  
```

* Okay, now let's wire up the API URLs. On to **my_project/urls.py** <br>

```python 
from django.urls import include, path
from rest_framework import routers
from my_rest_api_app import views

router = routers.DefaultRouter()
router.register(r'users', views.UserViewSet)
router.register(r'groups', views.GroupViewSet)

# Wire up our API using automatic URL routing.
# Additionally, we include login URLs for the browsable API.
urlpatterns = [
    path('', include(router.urls)),
    path('api-auth/', include('rest_framework.urls', namespace='rest_framework'))
]

```

* Pagination allows you to control how many objects per page are returned. To enable it add the following lines to **my_project/settings.py** <br>

```python 
REST_FRAMEWORK = {
    'DEFAULT_PAGINATION_CLASS': 'rest_framework.pagination.PageNumberPagination',
    'PAGE_SIZE': 10
}
  
```

* Add 'rest_framework' and your **app name** to INSTALLED_APPS. The settings module will be in **my_project/settings.py** <br>

```python 
INSTALLED_APPS = [
    ...
    'rest_framework',
    'my_rest_api_app'
]
  
```

# Testing our API

* We're now ready to test the API we've built. Let's fire up the server from the command line. <br>

```python 
  python manage.py runserver
```

* Open your browser. Hit this URL
```
  http://127.0.0.1:8000/
```
## If you see something like below then your **Django REST API** backend or server is working perfectly!

![Alt djangorestframework](/screenshots/s3.png "djangorestframework")

* Login with your username and password. In my case :

```
username : turzo
password : 123456 

```

* Then hit the link below into your browser.

```
  http://127.0.0.1:8000/users
```

![Alt djangorestframework](/screenshots/s4.png "djangorestframework")

## Congratulations! You successfully installed and running Django REST Framework based REST API server on your machine! 😄 😄 😄

### Keep Coding!!! 