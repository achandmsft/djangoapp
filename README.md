# Deploying a Python Django app with PostgreSQL on Azure
Go to http://shell.azure.com and run the following (Or run locally using the [Azure CLI](http://aka.ms/cli)) -  
# Create a virtual environment    
    $ git clone https://github.com/achandmsft/djangoapp
    $ cd djangoapp
    $ virtualenv venv
    $ source venv/bin/activate
# Create a PostgreSQL database    
    (venv)$ az postgres up -d pollsdb -u dbuser -p <secretpassword>
    (venv)$ export DBHOST="<servername>.postgres.database.azure.com"
		(venv)$ export DBUSER="dbuser@<servername>"
		(venv)$ export DBNAME="pollsdb"
		(venv)$ export DBPASS="<secretpassword>"
# Configure the Django App     
    (venv)$ python manage.py migrate
    (venv)$ python manage.py createsuperuser
# Deploy the Django App and connect it to the PostgreSQL database     
    (venv)$ az webapp up --name <enter a unique name for your app> --location centralus
    (venv)$ az webapp config appsettings set --name <web app name> --resource-group <web app rg> --location centralus --settings DBHOST=$DBHOST DBUSER=$DBUSER DBPASS=$DBPASS DBNAME=$DBNAME 

# Make changes and redeploy the app
    $ code app.py
    $ az webapp up --name <enter a unique name for your app> --location centralus   

For more on Azure CLI, go to http://aka.ms/cli (az webapp up: http://aka.ms/azwebappup)

# Contributing
This project has adopted the [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/). For more information see the [Code of Conduct FAQ](https://opensource.microsoft.com/codeofconduct/faq/) or contact [opencode@microsoft.com](mailto:opencode@microsoft.com) with any additional questions or comments.
