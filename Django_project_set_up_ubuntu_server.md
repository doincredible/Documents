# Deploying a Django Project on a DigitalOcean Ubuntu Server
Deploying a Django project on a DigitalOcean Ubuntu server involves several steps. Below are the detailed instructions with explanations and commands for each step.
## Prerequisites:
1. A DigitalOcean account.
2. A Django project ready for deployment.
3. Basic knowledge of Linux commands.
---

## Steps:

### Step 1: Create a Droplet
1. Log in to DigitalOcean and create a new Droplet.
2. Choose Ubuntu as the operating system.
3. Select a suitable plan based on your project's requirements.
4. Choose a datacenter region.
5. Add your SSH key for secure access.
6. Click "Create Droplet".
---

## Setting Up the Environment

## Step 1:  Set up the environment and Update the system
### Command:
```bash
$ sudo apt update
```
## Step 2:  Set up the environment upgrade the system.
### Command:
```bash
$ sudo apt upgrade
```
## step 3: Create a Directory for Your Django Project
You will need a directory to store your project files.
### Command:
```bash
mkdir folder-name
# Example: mkdir django-apps
```
## Step 4: Navigate to the Directory
Change into the newly created directory.
### Command:
```bash
cd folder-name/
# Example: cd django-apps/
```
---
# Virtual Environment Setup
## Step 5:Install Virtual Environment Tool
To isolate your project dependencies, it's recommended to create a virtual environment for your Django project.
### Command:
```bash
$ sudo apt install python3-venv
```
## Step 6:Create a Virtual Environment
Create a virtual environment inside your project directory.
### Command:
```bash
python3 -m venv myprojectenv
# Replace "myprojectenv" with your desired virtual environment name.

```
## Step 7:Verify the Virtual Environment Creation
Check if the virtual environment folder has been created.
### Command:
```bash
ls
# example  root@ubuntu-s-1vcpu-1gb-blr1-01:~/django-apps# ls
output: myprojectenv
```
## Step 8:Activate the Virtual Environment
Activate the virtual environment to start working within it.
### Command:
```bash
source myprojectenv/bin/activate
# To deactivate the virtual environment later:
deactivate
```
---
# Clone Your Django Project
## Step 9: Clone the Project from GitHub
Navigate to the directory where you want to clone the project and use Git to pull your code from the repository.
### Command:
```bash
cd /YourFolderName
sudo git clone https://github.com/yourusername/your-django-repo.git
```
## Step 10: Authenticate with GitHub
If prompted, provide your GitHub username and token (or password) to complete the clone operation.
```bash
Username for 'https://github.com/yourusername/your-django-repo.git': your-github-username
Password for 'https://github.com/yourusername/your-django-repo.git': your-github-token
```
---
# PostgreSQL Setup
## Step 11:  Install PostgreSQL
Install PostgreSQL, a popular database that Django works well with.
### Command:
```bash
sudo apt install postgresql postgresql-contrib
```
## Step 12: Start and Enable PostgreSQL Service
Ensure that PostgreSQL is running and enabled to start on boot.
### Command:
```bash
sudo systemctl start postgresql
sudo systemctl enable postgresql
```
## Step 13: Verify PostgreSQL Version
Check the installed version of PostgreSQL.
### Command:
```bash
psql --version
# Example output: psql (PostgreSQL) 16.4 (Ubuntu)
```
---
# Database Setup
## Step 14: Switch to PostgreSQL User
Switch to the postgres user to begin database configuration.
### Command:
```bash
sudo -i -u postgres
```
## Step 15:  Access PostgreSQL Command Line
Start the PostgreSQL interactive terminal.
### Command:
```bash
psql
```
## Step 16: Create a New Database
Create a database that Django will use.
### Command:
```bash
CREATE DATABASE your_db_name;
# Example output: CREATE DATABASE
```
## Step 17: Create a New User (Role)
# Create a PostgreSQL user and set a password.:
### Command:
```bash
CREATE USER your_db_user WITH PASSWORD 'your_password';
# Example output: CREATE ROLE
```
## Step 18: Grant Privileges to the User
Grant the necessary permissions for the user to interact with the database.
### Command:
```bash
GRANT ALL PRIVILEGES ON DATABASE your_db_name TO your_db_user;
# Example output: GRANT
```
## Step 19: Exit the PostgreSQL Terminal
# Exit the PostgreSQL interactive terminal.
### Command:
```bash
\q
```
## Step 20: Exit from the PostgreSQL User
Return to the root or regular user session.
### Command:
```bash
exit
# Output: Logout
```
---
# Optional: Allow Remote Access to PostgreSQL
### Step 21:  Edit PostgreSQL Configuration for Remote Access (Optional)
Edit `postgresql.conf` to allow listening on all IP addresses:
# step 1:Open postgresql.conf
Find and edit the `listen_addresses` setting to allow PostgreSQL to listen on all IPs.
### Command:
```bash
sudo nano /etc/postgresql/14/main/postgresql.conf
```
Change the line to:
```bash
listen_addresses = '*'
```
# step 2:Open `pg_hba.conf`
Edit the `pg_hba.conf` file to allow access from specific IPs or all IPs.
``` bash
sudo nano /etc/postgresql/14/main/pg_hba.conf
```
# Add the following line at the end to allow access from any IP address:
serch IPv4 local connection
```bash
local   all            all             127.0.0.0/0            scrum-sh256
```
Update the IPv4 local connections:
```bash
host    all             all             0.0.0.0/0               md5
```
# step 3: Restart PostgreSQL
Restart the PostgreSQL service to apply the changes.
```bash
sudo systemctl restart postgresql
```
---
# Django Project Setup
## Step 22: Django Project Setup
Go to the root directory of your Django project.
### Command:
```bash
cd path/to/your/project-directory
```
## Step 23:Activate the Virtual Environment
Reactivate the virtual environment if it's not already active.
### Command:
```bash
source myprojectenv/bin/activate
# Example: (myprojectenv) root@ubuntu:~/django-apps#
```
## Step 24:  Install Project Dependencies
Install the necessary packages listed in your requirements.txt file.
### Command:
```bash
pip install -r requirements.txt
```
---
# If you have to use PostgreSql then install psycopg2 library Challanges
# Install psycopg2 (for PostgreSQL)
```bash
 pip3 install psycopg2
```
If your project uses PostgreSQL, you need to install the psycopg2 package.
```bash
Python               version	                                            Command	Note
Default Python 3	sudo apt install libpq-dev python3-dev	
Python 3.x	        sudo apt install libpq-dev python3.x-dev	substitute x in command
Python 2	        sudo apt install libpq-dev python-dev	
```
If that's not enough, you might additionally need to install
```bash
sudo apt install build-essential
OR
sudo apt install postgresql-server-dev-all
```
as well before installing psycopg2 again.


