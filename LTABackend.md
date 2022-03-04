# What you need

To get started, you need to have these things already set up.

## A host machine 
To serve the web app, and the service, you need a host machine.
* A VM that is reachable from the internet. (We used a self hosted Centos 7 2009, 2.1 GHz Intel Xeon, 8 GB RAM, 10 GB disk free)
* A docker daemon installed on the VM.
* The firewall of the VM, and the firewall of the network, opened for web traffic on port 80 and 443.

For the network setup of the host machine, you also need:
* The VM reachable by SSH, preferably behind VPN, to deploy by below methods.
* An HTTPS certificate for `<yourdomain>` (eg for myproject.myorg.com) from a certificate provider (like DigiCert). 
* A backup service able to copy a directory on the VM into external backup of your choice outside the VM.

## A dev machine
These GitHub repos contain the Web Service component and the Web App components. To make modifications to the Web Service and the Web App, particular to your project, you need a dev machine setup.
* An editor, eg MS Code.
* NPM
* Docker
* The Web Service repo, and the Web App repo, cloned down to your machine.

## A Firebase account
To authenticate both as an administrator web user, and as a smartphone end user, Firebase users are used. Firebase is simply used as an identity provider, and you can create your users anonymously. 

To create the Users, you need a Firebase account. The same account should be used for authentication both for back-end (this readme) and for the iOS/Android clients ([the LTA readme](https://github.com/HumlabLu/HumlabLu#readme)).

# Get it spinning
This guide takes you forward in sections. You need to complete all sections to be able to submit, schedule, and publish, any surveys.

When completed, your LTA server is ready for live, online, usage for your own survey JSON files and your iPhone and Android users. Note that you will not get anywhere without all the prerequisites under **What you need** above in place first.

If you rather want to play around with the back-end only, locally on your dev machine, without making it live online for smartphone users, you can too, with some small changes to code, scripts, and steps. This readme does however not cater directly to that sub-scenario.

## Firebase
1. In the firebase web console, create anonymous user names per study, and admin user names, eg `s1_abc_123` and `adm_def_456`, and secure passwords for each user. ***!TODO link to LTA readme # firebase instead***
3. Specify admin user names (eg `adm_def_456`) in the constant `ADMIN_USERNAMES` in the Web Service config file `surveyApi.constants.ts`.
4. Generate a JSON file with a private key that the Web Service can use, see [initialize the SDK](https://firebase.google.com/docs/admin/setup#initialize-sdk) in the firebase documentation.
5. Add your JSON credentials file to the Web Service build by
	*  putting it in the `constants` folder. ***! TODO remove ours from public repo***
	* initialising the `serviceAccount` variable with the path to your credential file in the `firebaseAdmin.service.ts` file. 

## HTTPS
1. From your HTTPS certificate, find or generate a `.pem` and a `.key` file, alternatively a `.crt` bundle file and a `.key` file. Providers often have support documentation online for how to generate them.
2. Place the ***! TODO where to place it***
3. Edit the `/nginx_config/default.conf` to match your `.pem` and `.key` file names, alternatively your  `.crt` bundle and a `.key`.

## Web Service
1. Go to the `surveyApi` path on your dev machine with a terminal window
2. Build the docker image by `docker build -t <yourname>/lta-api .`
3. Pack the docker image by `docker save -o ./docker-lta-api-<yourversion>.tar <yourname>/lta-api`
4. Move that tar over to the VM by eg a `scp` command. Place it in `<somefolder>/src/surveyApi`.
6. SSH to the VM
7. On the VM, load the package as a docker image by `docker load -i docker-lta-api-<yourversion>.tar`

## Web App
1. Go to the `webapp` path with a terminal window
2. Build the docker image by `docker build -t <yourname>/lta-webapp .`
3. Pack the docker image by `docker save -o ./docker-lta-webapp-<yourversion>.tar <yourname>/lta-api`
4. Move that tar over to the VM by eg a `scp` command. Place it in `<yourfolder>/src`.
6. SSH to the VM
7. On the VM, load the package as a docker image by `docker load -i docker-lta-webapp-<yourversion>.tar`

## Docker-compose
1. Place the web app's `docker-compose.yml` in the  `<yourfolder>/src` folder. It covers both the web service, its routing and security, and the web app.
2. `docker-compose up`

## Is it working?
In Chrome, navigate to `<yourdomain>`. It should show you a webpage where you can log in with an _admin_ user.
![enter image description here](https://i.imgur.com/yy1dh7g.png)

## 404?
Trouble shooting, you could:
* From Chrome or Postman on eg your dev machine, call  `<yourdomain>/api/ping`. This is an unauthenticated API endpoint that should always reply `pong`, independent from authentication and DB. If this works, and other things does not work, the networking and routing is working, and the problem is not that the web service is not serving, but perhaps a problem in authentication or in the db.
* From the VM terminal, `curl <yourdomain>:80/api/ping` . If this works from the VM, but calling it over the internet does not, there is a problem in the network setup.

## Try it

1. On a smartphone LTA app, log in with `<youruser>` and close the app.
2. On a the web app, log in with `<youradminuser>`. (It could be the same as `<youruser>` if you specified `<youruser>` as an admin.)
3. Submit a survey config (you can copy-paste the example file ***! TODO example file*** next to this readme file).
4. On the page of the newly created survey, schedule a series to a `<youruser>` eg starting and ending today, with one assignment at the time of right now, and one 30 minutes forward.
5. Within 20 seconds from you scheduling the series, you should have a notification on your phone.
6. Upon pressing the notification, the app opens, and in the app, in the list of assignments, find the first assignment of your scheduled series.
7. Complete the assignment, and submit it.
8. In the web app, on the page of `<youruser>`, see the list of assignments that are scheduled from the series you just created.
9. See that the _Track_ and _Notif_ fields has been gotten updated from your first assignment. It should now have
	*  one notification (the reminder notification is not there if you completed it well in time),
	* a record of you opening it, and
	* a dataset with your answers.
10. On the survey, press *Generate CSV* , or `curl <yourdomain>/api/survey/<survey_id>/datasets/csv` a for JSON file.  

# Before going live
## External backup
The dockerfile points the db folder to `/db/live`. In `/db/backup/` you can stow away live data locally, so that your external backup system (slower) does not have to read live data.

1. Create a script `zip_and_clean.sh` to copy from the live folder into  `<yourfolder>/db/backup`,  eg like this, with rolling removal on 10 days:
```
sudo tar -zcvf "/home/<yourfolder>/db/backup/db-$(date +"%Y-%m-%d %H%M%S").tar.gz" /home/<yourfolder>/db/live/

find '/home/jo3302gr/db/backup/' -name 'db-*.tar.gz' -mtime +10 -type f -delete
```
3. Create a cron job to run it, eg like this, every morning at 00:30 AM.
```
30 0 * * * /home/<yourfolder>/db/backup/zip_and_clean.sh >  /home/<yourfolder>/db/backup/cron.log
```
4. Point your external backup system to `<yourfolder>/db/backup` 
5. Schedule your (nightly) external backup to 30 min after your cron job.


