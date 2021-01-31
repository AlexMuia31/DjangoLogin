# Django login project using Allauth

- Go to wireframepro.mockflow.com and draw a design of what you like your frontend to look like. Here's mine.

![](images/wireframe.png)

- Check if you have python3 installed in your system using the command: python --version

- Install virtualenv using the command: pip install virtualenv

- create a virtual environment using the command: virtualenv nameOfTheVenv in your prefered directory

- cd into the directory you have created the virtualenv and activate it using the command: source nameOfTheVenv/bin/activate

- Install django in the activated virtualenv using the command: pip install django

- start a new project with the virtualenv activated using the command: django-admin startproject NameOfProject

- cd into the project and start one app with the command: python manage.py startapp NameOfYourApp

- go to settings.py and add the app into installed apps as shown below

            INSTALLED_APPS = [
                    # defaultApps
                    .....
                     # MyApps
                    'NameOfYourApp',
            ]

# Using django-allauth

Django-allauth is an Integrated set of Django applications addressing authentication, registration, account management as well as 3rd party (social) account authentication.

For this task we are going to use the social account authentication. Social authentication
is always convenient to users because the dont have to memorize the password to their many accounts on different platforms.

- Install allauth using the command: pip install django-allauth

* Go to settings.py and add 'django.contrib.sites' to INSTALLED_APPS and also add allauth as shown below:

            AUTHENTICATION_BACKENDS = [
                    ...
                    #Needed to login by username in Django admin, regardless of `allauth`
                    'django.contrib.auth.backends.ModelBackend',

                    #`allauth` specific authentication methods, such as login by e-mail
                    'allauth.account.auth_backends.AuthenticationBackend',
                    ...
                ]

            INSTALLED_APPS = [
                            ...
                      # The following apps are required:

                        'allauth',
                        'allauth.account',
                        'allauth.socialaccount',
                        'allauth.socialaccount.providers.google',
                        'allauth.socialaccount.providers.facebook',
                        'allauth.socialaccount.providers.github',

        #here we've added google,facebook, and github as our social authenticators

* add a SITE_ID in settings.py like this
  SITE_ID = 1

* Go to urls.py and and this to urlpatterns:

                        urlpatterns = [
                            ...
                            path('accounts/', include('allauth.urls')),
                            ...
                        ]

* Go to the App you created and make a folder called templates. Inside this folder create another folder with the name same as the app. In the second folder create a file called home.html or your preferred name.
  write the following code inside there. You can style it as you want.

                      {% if user.is_authenticated %}
              <h3>Welcome to InterIntel</h3>
              <p>You're logged in as , {{ user.username }} !</p>

              {% else %}
              <h5>Register/login below</h5>
              <a href="{% provider_login_url 'google' action='reauthenticate'  %}"
              ><button type="button" class="btn btn-danger mx-5 mb-3">
                  <i class="fa fa-google"></i>
                  Login with Google
              </button></a
              ></br>
              <a href="{% provider_login_url 'github' action='reauthenticate' %}">
              <button type="button" class="btn btn-dark mx-5 mb-3">
              <i class="fa fa-github"></i> Login with Github
              </button>
                  </a>
              </br>
              <a href="{% provider_login_url 'facebook' action='reauthenticate' %}">
              <button type="button" class="btn btn-primary mx-5">
                  <i class="fa fa-facebook"></i> Login with facebook
              </button>
              </a>

                  {% endif %}

* Then go to urls.py and tell it to point to this file as follows:

            from django.contrib import admin
            from django.urls import path, include
            from django.views.generic import TemplateView #this is for class based views
            from django.conf import settings #settings for static files
            from django.conf.urls.static import static # for static files

            urlpatterns = [
                path('', TemplateView.as_view(template_name='login/home.html')),
                path('admin/', admin.site.urls),
                path('accounts/', include('allauth.urls')),

            ] + static(settings.STATIC_URL, document_root=settings.STATIC_ROOT) # loading static files

* This is how my app looks after styling:
  ![](images/frontend.png)

* Now run this command to make the migrations: python manage.py migrate
  Then create an admin using the command: python manage.py createsuperuser
  Run the server using the command: python manage.py runserver and go to http://localhost:8000/admin/

* Add a Site for your domain, matching settings.SITE_ID (django.contrib.sites app) as shown below:

![](images/site.png)

- Go to https://console.developers.google.com/ to create a OAuth app. and input the details Name, URIs, and
  Authorized URIs as shown below:

![](images/google1.png)

![](images/google2.png)

- For github log into your account then go to settings then developer settings. create a OAuth app and get the details as shown below:

![](images/github.png)

- for facebook head to https://developers.facebook.com/ and get the details as shown below:

![](images/facebookDev.png)

- In all apps the callback should be:
  https://localhost/accounts/google/login/callback
  https://localhost/accounts/github/login/callback
  https://localhost/accounts/facebook/login/callback

- the URIs should be:
  https://localhost:8000

* Take the Client ID and the secret key the head to the admin dashboard and add the a social application as shown below:

![](images/googlesocial.png)

![](images/facebooksocial.png)

![](images/githubsocial.png)

- facebook wont work without ssl . To set it up you need to install the following packages:

                pip install django-extensions
                pip install Werkzeug
                pip install pyOpenSSL

                then add 'django-extensions' to your INSTALLED_APPS

* now run the server using the following command:

         python manage.py runserver_plus --cert-file /tmp/cert

* Go to your app and you'll be able to log in with the social apps. Here's how mine looks like:

![](images/login1.png)

![](images/login2.png)

# Setting uwsgi/nginx configuration

To configure uswgi in our project we start my installing the python development files needed through
ths command: sudo apt-get install python3-dev .

- Install uwsgi using this command: pip install https://projects.unbit.it/downloads/uwsgi-lts.tar.gz . this will give you the long term support version.

* Since this is a global installation make a directory that will hold the configurations using this command:

              sudo nano /etc/uwsgi/sites/yourPreferedName.ini

Next, we need to set up uWSGI so that it handles our project well. Change into the root project directory by setting the chdir option. We are going to use Nginx as a proxy server to specify how uWSGI shold listen for connections. Here is how the configuration file should look like.

![](images/nano2.png)

- Next, we’ll create a systemd unit file to manage the uWSGI emperor process and automatically start uWSGI at boot.

* We will create the unit file in the /etc/systemd/system directory, where administrator-created unit files are kept. We will call our file uwsgi.service using this command.

          sudo nano /etc/systemd/system/uwsgi.service

Here is how it should look like:

![](images/uswgi.png)

# Configuring Nginx

- Install and configure Nginx as our reverse proxy using the following command:

          sudo apt-get install nginx

* create a server block configuration using this command:

          sudo nano /etc/nginx/sites-available/NameOfYourProject

* The configurations should look like this:

![](images/conf.png)

Save the changes and enable the Nginx using this command:

        sudo ln -s /etc/nginx/sites-available/NameOfYourProject /etc/nginx/sites-enabled

- Check the configuration syntax by typing:

        sudo nginx -t

* Check the configuration syntax by typing:

          sudo nginx -t

If no syntax errors are detected, you can restart your Nginx service to load the new configuration:

      sudo systemctl restart nginx

If you remember from earlier, we never actually started the uWSGI server. Do that now by typing:

        sudo systemctl start uwsgi

- You should now be able to reach your two projects by going to their respective domain names. Both the public and administrative interfaces should work as expected.

- If this goes well, you can enable both of the services to start automatically at boot by typing:

          sudo systemctl enable nginx
          sudo systemctl enable uwsgi
