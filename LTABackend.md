# Lang-Track-App Backend

In this readme file you can learn about how to get the back-end server and its web app and web service of  **LTA, the Lang-Track-App**, up and spinning. 

## Supporting tools and components
 
 * **Firebase** for user authentication
 * **Docker** for containerization of the Web Service and the Web App, and for infra for routing and the database. Dockerized components used are also:
	 * **NginX** for routing and https
	 * **MongoDB** for storing all survey data and anonymous collected data

## Container and network overview
![cointainer_network](https://i.imgur.com/5z1qp4D.png)


# What you need for the Server side

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
See [the firebase page](https://github.com/HumlabLu/HumlabLu/blob/main/Firebase.md). 

The same account should be used for authentication both for back-end (this readme) and for the iOS/Android clients.

# Get it spinning
This guide takes you forward in sections. You need to complete all sections to be able to submit, schedule, and publish, any surveys.

When completed, your LTA server is ready for live, online, usage for your own survey JSON files and your iPhone and Android users. Note that you will not get anywhere without all the prerequisites under **What you need** above in place first.

If you rather want to play around with the back-end only, locally on your dev machine, without making it live online for smartphone users, you can too, with some small changes to code, scripts, and steps. This readme does however not cater directly to that sub-scenario.

## Firebase
Also see [the firebase page](https://github.com/HumlabLu/HumlabLu/blob/main/Firebase.md). 

1. In the Web Service config file `constants\surveyApi.constants.ts`, initialize the constant `ADMIN_USERNAMES` with the admin user names you created (eg `adm_def_456`).
4. In the Firebase console, generate a JSON credentials file with a private key that the Web Service can use, see [initialize the SDK](https://firebase.google.com/docs/admin/setup#initialize-sdk) in the firebase documentation.
5. Add your JSON credentials file with your own firebase project's private key, to the Web Service source code. 
	* See the `constants\PROJECT-firebase-adminsdk-ID.json.sample` credentials sample file. This is just a sample file, that you can remove if you want to.
	* Put your own file in the `constants` folder.
	* In the `firebaseAdmin.service.ts` file, set the path to your credentials json file, to initialise the `serviceAccount` variable.  

## HTTPS
1. From your HTTPS certificate, find or generate a `.pem` and a `.key` file, alternatively a `.crt` bundle file and a `.key` file. Providers often have support documentation online for how to generate them.
2. Edit the `/nginx_config/default.conf` to match your `.pem` and `.key` file names, alternatively your `.crt` bundle and a `.key`.
3. Move the files over to the VM by eg a `scp` command to `<yourfolder>/src/cert`

## Web Service
System-specific setup:

1. In `src/services/surveyApi.service.ts`, replace all instances of `@humlablu.com` with your own email service's domain.
2. In `src/services/firebaseAdmin.service.ts`, replace the `databaseURL` with your own Firebase database link (see [the Firebase page](https://github.com/HumlabLu/HumlabLu/blob/main/Firebase.md#web-api) for details)
3. In `docker-compose.yml`, replace the image name with your own user like so: `image: $YOU/lta-api:latest`.
4. In `package.json`, set `main` to `src/server.ts` (the actual entry point to the program).
	* Some dependencies may need updating.

Generating a docker image:

1. Go to the `surveyApi` path on your dev machine with a terminal window
2. Build the docker image by `docker build -t <yourname>/lta-api .`
3. Pack the docker image by `docker save -o ./docker-lta-api-<yourversion>.tar <yourname>/lta-api`
4. Move that tar over to the VM by eg a `scp` command. Place it in `<somefolder>/src/surveyApi`.
6. SSH to the VM
7. On the VM, load the package as a docker image by `docker load -i docker-lta-api-<yourversion>.tar`

## Web App
System-specific setup:

1. In `src/main.js`, change the contents of `firebaseConfig` to your own Firebase information (see [the Firebase page](https://github.com/HumlabLu/HumlabLu/blob/main/Firebase.md#web-app) for details)
1. In `src/http-common.js`, change the `baseURL` to the domain you will be hosting at app on.
2. In `src/router.js`, remove the `/` alias from the `/surveys` object and add it to the `/login` one (this will make the login page the default).
3. In `nginx_config/default.conf`, change `server_name`, `ssl_certificate`, and `ssl_certificate_key` to the values you will be using.

Generating a docker image:

1. Go to the `webapp` path with a terminal window
2. Build the docker image by `docker build -t <yourname>/lta-webapp .`
3. Pack the docker image by `docker save -o ./docker-lta-webapp-<yourversion>.tar <yourname>/lta-api`
4. Move that tar over to the VM by eg a `scp` command. Place it in `<yourfolder>/src`.
	* You may need to `chown` it to your own user first.
6. SSH to the VM
7. On the VM, load the package as a docker image by `docker load -i docker-lta-webapp-<yourversion>.tar`

## Docker-compose
1. Place the web app's `docker-compose.yml` in the  `<yourfolder>/src` folder. It covers both the web service, its routing and security, and the web app.
2. `docker-compose up`

## Is it working?
In Chrome, navigate to `<yourdomain>`. It should show you a webpage where you can log in with an _admin_ user.
![enter image description here](https://i.imgur.com/yy1dh7g.png)

Try a full user journey for both participants and researchers, by following the stesps in [Try it](https://github.com/HumlabLu/HumlabLu/blob/main/try-it.md).

## 404?
Trouble shooting, you could:
* From Chrome or Postman on eg your dev machine, call  `<yourdomain>/api/ping`. This is an unauthenticated API endpoint that should always reply `pong`, independent from authentication and DB. If this works, and other things does not work, the networking and routing is working, and the problem is not that the web service is not serving, but perhaps a problem in authentication or in the db.
* From the VM terminal, `curl <yourdomain>:80/api/ping` . If this works from the VM, but calling it over the internet does not, there is a problem in the network setup.


