# Remote-Backup-HPE-OneView
#Pulls backup files from OneView appliances and stores on target system - This solution is to be used if for some reason using the automatic backups is not an option.
#For a script more code to create automatic backups on appliances, please refer to other GitHub repository entitled Configuring Automatic Backups in OneView


There are a few things to do beforehand to properly run the script:

1.)	Setup backup administrator account - The script leverages a specialized role called “backup administrator” on the OneView appliance and is used specifically for backing up the appliance. To proceed we will need to create the “backup administrator” account on each OneView appliance.

From the Users and Groups page  in OneView select "Add User" - Choose the "Backup Administrator" account. Note: Backups can be exectued with a standard administrative user.
 

2.)	Populate Environment File – With OneView Hostname/IP,  backup administrator Username and Password secure standard string of OneView appliances. 

Run script, when prompted, enter password for backup administrator account. The script will then create a file called password.txt in the specified directory and then stop at “Populate environment file” before importing the environment file. At this point, copy the contents of the password.txt file and paste into the environment file under the password header. The script will pull this encrypted standard string later, convert it to a secure string object and use it as the password to log into the appliance.
 

3.)	Running the script – Choose to continue running the script after the “populate environment file” pause by hitting enter. The script will loop through the environment file, connect to the OneView appliances Hostname/IP address, download and store the backup file in a folder created based off the hostname in the environment file, then finally disconnect from the appliance. The default location where the download files will be stored is C: drive. This can be edited by changing line 65 of the script:

 

The script will also take the output of the “New-HPOVBackup” cmdlet and pass it into a file called “backuplogs.csv”. The output will append the content acting as a log file making separate entries for each host in the environment file.

4.)	File Management – This script also includes a file management algorithm that handles how many backup files to keep in a directory and for how long to keep them. Every time the script is ran, it analyzes the hostname directory, taking in how many files exist and how old they are. The algorithm will keep all files that are generated from the current and previous month. Other months outside of this range will only have one copy kept and anything else will be deleted. This will cut down on the size of the directory and make file organization part of the process while reducing complexity. 

Note: Line 118 in the script will delete backup files but will not remove them entirely from the server. It will place them in the recycle bin so they can be restored if desired.
               

 
        
	

