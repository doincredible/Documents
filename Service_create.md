# How to Create a systemd Startup Service for Your Django Application

This guide walks you through the steps required to set up a systemd startup service for your Django application on an Ubuntu server.

---

## Step 1: Create the Service File

To begin, you need to create a service file that will define how your Django application is started and managed by systemd.

### Command:
```bash
sudo nano /etc/systemd/system/your-django-app.service
```
## Step 2: Configure the Service File
Now, edit the newly created file by adding the necessary configuration. Replace the placeholders with your actual environment paths and file names.
Example Configuration:
### File: `your-project.service`
```basg
[Unit]
Description=Django app service
After=network.target
[Service]
User=root
Group=root
#Set your environment variable
Environment="your project Environment variable value"
Environment="YOURPROJECTNAME_DB_NAME=abc"
Environment="YOURPROJECTNAME_DB_USER=abc
Environment="YOURPROJECTNAME_DB_PASS=abc"    
Environment="YOURPROJECTNAME_DB_HOST=localhost"    
Environment="YOURPROJECTNAME_DB_PORT=5432"    
#Define the command to start your Django application
ExecStart=/root/your/env/bin/python3 /root/your-folder-name/project-folder-name/manage.py runserver 0.0.0.0:8000
#Log output to specific files
StandardOutput=append:/var/log/kudo/filename.log
StandardError=append:/var/log/kudo/filename.log
#Restart the service on failure
Restart=on-failure
[Install]
WantedBy=multi-user.target
```
Save and close the file by pressing Ctrl + X, then Y, and hit Enter.
# Your Django `settings.py` file define (Environment variables) defilne in your-django-app.service file
```bash
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': os.getenv('YOURPROJECTNAME_DB_NAME','abc'),
        'USER': os.getenv('YOURPROJECTNAME_DB_USER','abc'), 
        'PASSWORD': os.getenv('YOURPROJECTNAME_DB_PASS','abc'),
        'HOST': os.getenv('YOURPROJECTNAME_DB_HOST','localhost'),  
        'PORT': os.getenv('YOURPROJECTNAMEN_DB_PORT','5432') 
    }
}
```
# Step 3: Reload systemd Daemon
To ensure that the newly created service file is recognized by systemd, reload the systemd daemon.
Command:
```bash
sudo systemctl daemon-reload
```
# Step 4: Start the Django Service
Now, start your service by running the following command:
Command:
```bash
sudo systemctl start your-django-app.service
```
# Step 5: Enable the Service to Start on Boot
If you want your Django app to start automatically when the server reboots, you need to enable the service to start on boot.

Command:
```bash
sudo systemctl enable your-django-app.service
```
# Step 6: Check the Status of the Service
To verify whether your service is running correctly, check its status using:

Command:
```bash
sudo systemctl status your-django-app.service
```
# Log output cheak to specific files
```bash
/var/log/kudo/your-filename.log
```
# This command will display the current status of your service and whether it started successfully.

