# Deploying a Python Django app with PostgreSQL on Azure
Go to http://shell.azure.com and run the following (Or run locally using the [Azure CLI](http://aka.ms/cli)) -  

# Create a virtual environment    
    $ git clone https://github.com/achandmsft/djangoapp
    $ cd djangoapp
    $ python3 -m venv env
    $ source env/bin/activate
    (env)$ source ./env.sh
    (env)$ pip install -r requirements.txt

# Create a PostgreSQL database    
    (env)$ az extension add --name db-up
    (env)$ DBPASS=$(openssl rand -hex 12)'A1!'
    (env)$ az postgres up -d pollsdb -u manager -p $DBPASS
    (env)$ export DBHOST="<servername>.postgres.database.azure.com"
    (env)$ export DBUSER="manager@<servername>"
    (env)$ export DBNAME="pollsdb"

# Configure the Django App   
The name for your app must be globally unique as it will prefix the website address: http://yourapp.azurewebsites.net/ 
    (env)$ python manage.py migrate
    (env)$ python manage.py createsuperuser

# Deploy the Django App and connect it to the PostgreSQL database     
    (env)$ az webapp up --name <enter a unique name for your app> --location centralus
    (env)$ az webapp config appsettings set --name <from above> --resource-group <from above> --settings DBHOST=$DBHOST DBUSER=$DBUSER DBPASS=$DBPASS DBNAME=$DBNAME 

For more on Azure CLI, go to http://aka.ms/cli (az postgres up: http://aka.ms/azpostgresup, az webapp up: http://aka.ms/azwebappup)

# Known issue
If you get an exception with the above in shell.azure.com that resource group name is invalid, run az login and complete the instructions clicking on the devicelogin link and pasting the code from the az login output.

# Contributing
This project has adopted the [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/). For more information see the [Code of Conduct FAQ](https://opensource.microsoft.com/codeofconduct/faq/) or contact [opencode@microsoft.com](mailto:opencode@microsoft.com) with any additional questions or comments.
