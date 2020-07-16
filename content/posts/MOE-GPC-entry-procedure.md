---
title: "MOE GPC Entry Procedure"
description: "Onboarding to GPC"
date: 2020-07-16T11:58:34+08:00
draft: false
---
## DEPLOYMENT STEPS
---
### 1. ARTEFACT PREP
_download artefacts, prep & tar folder, import to server (steps 1-5 to be done before going to GPC)_
1. download zip file of front and back artifacts from GIT
2. unzip both folders, you will get moe-ansible and moe-ansible 2
3. copy contents of moe-ansible 2 into moe-ansible
4. open terminal and cd into enclosing folder
5. tar moe-ansible folder
    1. tar -zcvf moe-ansible.tar.gz moe-ansible
---
### 2. BACK UP DB
_(BACKUP CURRENT DB BEFORE DOING ANYTHING FOR DEPLOYMENT)_
1. Login to RADIUS Portal (refer to “Login to RADIUS Portal”)
2. Select “Favourites”
3. Select server T00301BRND005 OR 006(for prod) T00505BRND001(for uat use VMAdmin) (Try logging into DB, if unable to, means its the other server) SSH
4. LOGIN USING ANOTHER ACC (only for PROD, password not required for UAT)
    - id: moedsa\moeadmin  pwd: Adm1n@123456
5. open Microsoft SQL -> swee heng will enter password
6. file explorer -> E drive -> MSSQL 12.SQLCLUSTER2014 -> MSSQL -> Backup -> OldBackups -> 2020backup -> PRODUCTION
7. create folder with date and issue eg: 19JuneAppReportB4
8. backup DB in folder and name it dsadb.bak
    - Right click dsadb (under Databases)   
    - Select Task … Backup
    - click “Add” and select path of “19JuneAppReportB4”
    - File name to save as dsadb.bak
    - Click ok
    - Select the backup and click "Ok"     _—> Backup will begin and prompt upon success._
Go to enclosing backup folder in file explorer and check backup file size
---
### 3. ARTEFACT PREP
_at gpc,_
1. connect secured thumb-drive (provided by swee heng) to local pc
2. unlock thumb-drive by press the green unlock button once, then numbers "112233445", then green unlock button again.
3. transfer moe-ansible.tar.gz into secured drive
transfer artefact folder to server (UAT/PROD) (use WinSCP to transfer files from thumb-drive to GPC PC)
backup current prod artefact tar file before transferring the new one
4. Login to RADIUS Portal (refer to “Login to RADIUS Portal”)
5. Select “Favourites”
6. Select server T00301BRNT002
7. Connect using SSH
8. Enter a message "deployment to uat" or any logical message
9. Click Allow
10. Terminal will be launched
11. Run script to keep session alive (ctrl + r >> ‘wh’ >> enter)
    - backup current moe-ansible.tar
    - ls -la
    - mv moe-ansible.tar.gz moe-ansible.tar.gz_b419june 	--> rename to backup current prod artefact b4 deployment
    - su appuser
    - enter password -> A66!QAZ2wsx
    - ls -la
    - rm -rf moe-ansible 	     --> (delete previous untar artefact)
    - ls -la (make sure files are renamed/deleted)
    - transfer artefact file into TMT server
12. Connect using PSM-WinSCP
13. Type in message "transfer artefacts for deployment" and select "map local drive"
14. Click yes and continue
15. On the left, go to thumbdrive folder - drive Z: D
16. On the right, go to /home/VMAdmin1
17. Drag moe-ansible.tar.gz from the left to right
18.  Close the session once successfully copied to server, prep artefact file for deployment
19. Go back to RADIUS Portal
20. Select server T00301BRNT002
21. Connect using SSH
22. Backup previous artefact & untar new artefact file
    - sudo cd /home/appuser
    - ls -la
    * _change to appuser_
    - su appuser
    - enter password -> A66!QAZ2wsx
    - ls -la
    - sudo cp /home/VMAdmin1/moe-ansible.tar.gz . 	--> this copies artefact from /home/VMAdmin1 to current directory “.”(/home/appuser)
    - sudo ls -la (to check if file is copied)
    * _Change owner of file_
    - sudo chown appuser:appuser moe-ansible.tar.gz
    - ls -lart (check owner is now appuser and not root)
    * _Untar file_
    - tar -zxvf moe-ansible.tar.gz
    - ls -ltr 	--> you will see a file named moe-ansible
---
### 4. DEPLOYMENT TO UAT/PROD
1. Login to RADIUS Portal (refer to “Login to RADIUS Portal”)
2. Select “Favourites”
3. Select server T00301BRNT002 	--> both UAT and PROD uses the same server for deployment. deployment script is what differentiate whether deploy to uat or prod
4. su appuser
5. enter password -> A66!QAZ2wsx
6. cd /home/appuser/moe-ansible
7. ls -la
8. check permission for uat_gpc_deployment.sh and change its permission
9. chmod +x uat_gpc_deployment.sh or prod_gpc_deployment.sh 	--> depending on UAT or prod deployment
10. ./uat_gpc_deployment.sh or ./prod_gpc_deployment.sh
11. Palo@uat (this pwd is for UAT) Palo@123 (this pwd is for PROD)
###### ** KEEP MOVING MOUSE TO KEEP SESSION ALIVE. DEPLOYMENT WILL HALT IF PC GOES TO SLEEP **
12. Once deployment done, ensure 0 errors (i.e. unreachable = 0, failed = 0)

---
### Login to GPC PC 
1. Turn on Server device on the desk (black ThinkCentre device) - press the round button on the left
2. Turn on PC - bitlocker pwd 345678
3. Windows login credentials - user SOE\PALOIT-VandaG, pwd 1qaz2wsx#EDC
###### ** note that password expires yearly, next reset is to be made BY 7th July 2021 **
* call AFM service desk for reset of password, number is **1800-879-6333**
---
### Login to RADIUS Portal
1. Open Internet Explorer, go to Favourites, Select NCS TFM RAI PORTAL
2. Click on RADIUS
3. RADIUS login credentials - user RTMOE-PALOIT-VANDAG, pwd DsaPaLo7 + token
---
### For more details, full documentation can be found [here!](https://drive.google.com/file/d/1ekNXO0M49leaLHgfrIHepXQICQePbyvn/view?usp=sharing)