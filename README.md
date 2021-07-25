# This is a typical example of a business ready django backend project
### here i'll explain all the necessary information in a step by step manner , that how we start building any business application , what decisions and configurations we start making from the first day and what wise decision we make while setting up a new project

## BASIC
creating a project and connect it with GIT

#1 connect with GIT
prerequisite : GIT
- 1.0 first we create a empty folder and connect it with GIT
- 1.1 create a empty folder anywhere in the machine (EC2 / laptop)
- 1.2 open that floder in terminal
- 1.3 execute : `git init`
- 1.4 add two files in this folder i) .gitignore ii) README.md
- 1.5 execute : `git add .`
- 1.6 execute : `git commit -m "write some message you like"`
Now Go to GIT website , login/signup to your GIT account , and create a repository ( without README.md file )
As this repository gets created , there is a green button in top right named "code" , it is for giving you the url of this new GIT repository
there are three types of url it'll offer -> https , ssh, githubcli . copy the url according to how your git is configured on your laptop (http/ssh).
from the url , remove '.git' form end and copy the rest of the part
let's say this part is X-url
Now come back to your terminal
- 1.7 execute : `git remote add origin X-url`
- 1.8 execute : `git push -u origin master`

Now go back to your repository in GIT web , and you'll be able to see the new files added by you.


#2 add basic project 
prerequisite : python, virtualenv

common steps :
- 2.1 open the project folder in terminal , where you want to make your project ( i am assuming that this folder is already connected with GIT)
- 2.2 check active python version -> execute : `python --version`
- 2.3 check which python is currently active -> execute : `which python` , this will point to system python interpreter
- 2.4 create virtual environment :- (assuming your python version is 3.7) ,execute : `virtualenv --python=python3.7 name_of_virtualenv_venv`
- 2.5 activate virtual environment :- `source path_to_virtual_env_folder/name_of_virtual_env/bin/activate`
- 2.6 ignore the virtualenv files from GIT : add this `*env/` in .gitignore file (assuming that only the name of virtualenv is ending with 'venv'. This '' is for pattern matching the folder name to ignore)

steps for django :
- 2.7 install djnago and it's rest framework , execute : `pip install django djangorestframework`
- 2.8 create project folder with necessary files , execute : `django-admin startproject project_folder_name`
- 2.9 `cd project_folder_name`
- 2.10 `mkdir apps`
- 2.11 `$ cd apps`
- 2.12 `$ mkdir app_name`
- 2.13 `$ cd ../../`
- 2.14 `python3 manage.py startapp app_name apps/app_name`
Now this way your apps will start getting created in a seperate apps folder
- 2.15 goto your app_name folder , and open apps.py file :-
```
class SampleServiceConfig(AppConfig):
    default_auto_field = 'django.db.models.BigAutoField'
    name = 'app_name'
```
change it to :
```
class SampleServiceConfig(AppConfig):
    default_auto_field = 'django.db.models.BigAutoField'
    name = 'apps.app_name'
```

- 2.16 goto settings.py file and add two new app's entry in INSTALLED_APPS list :-
```
INSTALLED_APPS = [
    ...
    'apps.app_name',
    'rest_framework',
]
```
<!-- add below information in a seperate lecture -->
- 2.17 [^1] now you can create a server using the inbuilt wsgi server of django : ``python3 manage.py runserver`` , if you want to run on specific ip and on specific port , `python3 manage.py runserver <ip>:<port>`
BUT , you will see this error in the terminal :- 'unapplied migrations . your project may run with problems'
There is concept of models , migrations and ORM in django . models are classes representing tables in database . migration are log/record files which keep record of any structural chage/modification activity you do on the connected database . these files help the other developer who is setting up a project created by you , he need not ask you how to make the database , what relations to keep and constarints to add on tables and rows , what indexes to add etc etc .
But you did'nt create any tables yet ( or you might even not have connected to a DB as well ), then why this error ?? 
Because , there are some default migration files for the basic inbuilt tables of django . you don'nt see these migration files , but they exsist in the djnago package which u installed .
So is it necessary to use these tables ?? Not at all !!
your wish .
some tables like - djnago_sessiontable is there , just in case when you wish to your default session authentication of django , this table will be used by django to store rows of session data.
but again , not a compulsion !! . write you own session authentication logic , make your own session table . some overridings might need to be done , like you gotta override the system that deals with request/reponse handling , coz u need to get the session information from the request object comming in your application and verify it against the session table you created , so that you can allow/block that request accordingly . This is a the same simple thing that djnago does for you in it's default django session auth .
But again , i told this , just to make you aware that , nothing is compulsive from django's side !!
<!-- add the above info in a seperate lecture -->

[^1]: TODO : add this info in seperate lecture
