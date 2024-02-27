## User Signup and Login
### Install dependencies
```BASH
pip insrall -r req.txt
```
### Run django server
```BASH
python manage.py runserver
```



## views.py
### Renders the home page.

Args:
- request: The request object. 

Returns:
- The rendered home page.

```python
def home(request):
    return render(request,'index.html')
```
### signin

Creates a new user in the database.

If the request method is POST, gets the username, password, and email from the form. 
Creates a new User object with create_user() using these values. 
Saves the user and redirects to the home page.

If the request method is not POST, just redirects to the home page.

Args:
- request: The request object.

Returns:
- A redirect to the home page


```python
def createuser(request):
    if request.method == 'POST':
        username = request.POST.get('siusername')
        password = request.POST.get('sipassword')
        email = request.POST.get('siemail')
        if username and password and email:
            user = User.objects.create_user(username=username, password=password, email=email)
            user.save()
            return redirect('/')
        else:
            return redirect('/')

    else:
        return redirect('/')

```
### login

- Logs in a user if the username and password are valid.
- Redirects to the home page if login succeeds, back to the login page if it fails.
- Logs in a user if the provided username and password are valid.
- Redirects to the home page if login succeeds, or back to the login page if it fails.
- Handles both GET and POST requests - renders the login form on GET, processes it on POST.

```python

def loginuser(request):
    if request.method == 'POST':
        username = request.POST.get('username')
        password = request.POST.get('password')
        print(username,password)
        user = authenticate(request, username=username, password=password)
        if user is not None:
            login(request, user)
            return redirect('/')
        else:
            return redirect('/')

    else:
         return redirect('/')

```
### logout
Logs out the currently logged in user.

Redirects to the home page after logging out.

Args:
- request: The request object.
    
Returns: 
- A redirect to the home page.

```python
def logoutuser(request):
    logout(request)
    return redirect('/')

```

## urls.py
```python
from django.contrib import admin
from django.urls import path
from home import views

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', views.home, name='home'),
    path('login/', views.loginuser,name="login"),
    path('signup/',views.createuser ,name="signup"),
    path('logout/',views.logoutuser,name="logout"),
    
]
```
