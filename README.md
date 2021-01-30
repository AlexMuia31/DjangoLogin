# Django login project using Allauth

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

* Now run this command to make the migrations: python manage.py migrate
  Then create an admin using the command: python manage.py createsuperuser
  Run the server using the command: python manage.py runserver and go to http://localhost:8000/admin/

* Add a Site for your domain, matching settings.SITE_ID (django.contrib.sites app) as shown below:

![](images/site.png)
