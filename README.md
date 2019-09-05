# Token Based Authentication
## Introduction
Token Authentication is a way to authorize users by using an API Key or Auth Token. The way Django REST Framework implements Token Authentication requires you to add a header for each request. This header will be in the following format:
```
bash  
Authorization: Token 93138ba960dfb4ef2eef6b907718ae04400f606a
```
* "Authorization" is the header key.
* "Token 93138ba960dfb4ef2eef6b907718ae04400f606a" is the header value. 

## Install the Requirements:
```
bash 
pip install -r requirements.txt  
```
## Preliminary Configuration:
Configure the authentication scheme in settings.py
***TokenExample/settings.py***
```
bash
REST_FRAMEWORK = {
    'DEFAULT_AUTHENTICATION_CLASSES': (
        'rest_framework.authentication.TokenAuthentication',
    ),
```
Add an app to your INSTALLED_APPS array
```
bash
INSTALLED_APPS = [
    # Django Apps
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',

    # Third-Party Apps
    'rest_framework',
    'rest_framework.authtoken',

    # Local Apps (Your project's apps)
    'TokenExample.core',
    ]
```
## Implement token authentication
***myapi/core/views.py**
```
bash
from rest_framework.views import APIView
from rest_framework.response import Response
from rest_framework.permissions import IsAuthenticated  # <-- Here


class HelloView(APIView):
    permission_classes = (IsAuthenticated,)             # <-- And here

    def get(self, request):
        content = {'message': 'Hello, World!'}
        return Response(content)
```
Create the superuser
```
bash
python manage.py createsuperuser 
```
The easiest way to generate a token using the command line utility:
```
bash
python manage.py drf_create_token username
```
* This piece of information, the random string is what we are going to use next to authenticate.
### How to use the code:
Open Postman. Make sure the dropdown says, **GET** and address bar- http://127.0.0.1:8000/hello
Go to header add
* key: Authorization
* value: Token --generated token--
Make sure under Authorization **No Auth** is selected.
Click send you should retrive the content

### Other way of creating token:
The DRF provide an endpoint for the users to request an authentication token using their username and password.
*** TokenExample/urls.py
```
bash
from django.urls import path
from rest_framework.authtoken.views import obtain_auth_token  # <-- Here
from myapi.core import views

urlpatterns = [
    path('hello/', views.HelloView.as_view(), name='hello'),
    path('api-token-auth/', obtain_auth_token, name='api_token_auth'),  # <-- And here
]
```


