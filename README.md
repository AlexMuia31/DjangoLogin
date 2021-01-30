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
