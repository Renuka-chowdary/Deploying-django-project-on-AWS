# Deploying-django-project-on-AWS

1. SSH into ec2 server using pem file

       sudo ssh -i /home/hashedin/Downloads/xxxxx.pem ubuntu@ec2-x.x.x.x.compute-1.amazonaws.com
       
2. Installing required packages.

          sudo apt-get update
          sudo apt-get install python3-pip python3-dev libpq-dev postgresql postgresql-contrib nginx


3. Create the PostgreSQL Database and User:

           sudo -u postgres psql
               CREATE DATABASE myproject;
               CREATE USER myprojectuser WITH PASSWORD 'password';
               GRANT ALL PRIVILEGES ON DATABASE myproject TO myprojectuser;
               \q

4. Create a Python Virtual Environment for your Project:

              sudo -H pip3 install --upgrade pip
              sudo -H pip3 install virtualenv
              
5. Create and move into a directory where we can keep our project files:
 
              mkdir ~/myproject
              cd ~/myproject


 6. Within the project directory, create a Python virtual environment by typing:

               virtualenv myprojectenv

    Activate the virtual environment. 

               source myprojectenv/bin/activate
    
    So will look something like this:

              (myprojectenv)user@host:~/myproject$


7. Creating the Django Project

              (myprojectenv) $: django-admin.py startproject myproject ~/myproject
              (myprojectenv) $: nano ~/myproject/myproject/settings.py

   Edit the file as follows:

            ALLOWED_HOSTS = ['your_ec2 -server_IP’s']

   Then choose database section and edit it as follows:
 
            DATABASES = {
                 'default': {
                      'ENGINE': 'django.db.backends.postgresql_psycopg2',
                      'NAME': 'myproject',
                      'USER': 'myprojectuser',
                      'PASSWORD': 'password',
                      'HOST': 'localhost',
                      'PORT': '',
                  }
              }



   Then at the bottom of the file add the this line:

              STATIC_ROOT = os.path.join(BASE_DIR, 'static/')
  
   Save and close the file.

8. Now, we can migrate the initial database schema to our PostgreSQL database using  
     the management script:

            (myprojectenv) $:  ~/myproject/python manage.py makemigrations
             (myprojectenv) $:  ~/myproject/python manage.py migrate

9. Create an administrative user for the project by typing:

             (myprojectenv) $ ~/myproject/python manage.py createsuperuser


      You will have to provide a username, mail address and password.     
   
10. Collecting  all of the static content into the directory location 

             (myprojectenv) $ ~/myproject/python manage.py collectstatic
                                       sudo ufw allow 8000
                                        ~/myproject/python manage.py runserver 0.0.0.0:8000


11. Now open browser and give

             http://ec2-server_IP:8000  (or)

             Your instance Public DNS 
             ec2-x-x-x-x-x.compute-1.amazonaws.com

 
So you get the  Django output like this





12. Add /admin to that url

              http://ec2-server_IP:8000/admin

