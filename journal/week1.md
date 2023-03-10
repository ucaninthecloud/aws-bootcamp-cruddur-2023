# Week 1 — App Containerization

## Reproducing Session ##

I joined the live session to follow along with the demo and conversation.

I created a new branch and kept all my activities in that one. At the end of the required activities, I merged everything to main.

For anyone that is not using Google Chrome and the Gitpod plugin, you can create a workspace out of a branch by adding */tree/name-of-branch* at the end of the standard URL:

```
https://gitpod.io/#https://github.com/ucaninthecloud/aws-bootcamp-cruddur-2023/tree/week1-session
```

After creating the Dockerfile for both (the backend and frontend containers) I initiated a docker compose. There is always an error when starting the app from a new Gitpod Workspace due to the missing npm install in the frontend directory so I updated  my .gitpod.yml to include the npm install in the /workspace/aws-bootcamp-cruddr-2023/frontend-react-js:

```yml
  - name: npm front-end
    init: |
      cd /workspace/aws-bootcamp-cruddur-2023/frontend-react-js
      npm install
```

I stopped and deleted the Workspace and tried once more. It was failing with a directory not found. On my second attempt I left it with ./frontend-react-js and it worked:

```yml
  - name: npm front-end
    init: |
      cd ./frontend-react-js
      npm install
```

I tried to capture the error by trying again with the complete path but it worked this time. One of those strange gitpod "glitches".

After the composer finished, I was able to see all 4 services started and open them to the public:

![Alt Text](assets/2023-02-28-Docker_Ports.png "Open Dockert ports")

The docker images are:

![Alt Text](assets/2023-02-28-Docker_Images.png "Available Images")

And the site loads without errors:

<img src="assets/2023-02-28-Docker_Frontend.png" width="70%">


Curl returns a good response as well:

<img src="assets/2023-02-28-Curl_Application.png" width="70%">

Verified how the mounts are attached to the containers and updated the backend-flask/services/home_activities.py to show the following message:

<img src="assets/2023-02-28-Mount_Changes.png" width="70%">

While I was following along, I submitted a PR for week 1. I tried to submit early in the week but I missed a couple steps after my fork was created.

```sh
docker run --rm -p 4567:4567 -it backend-flask
FRONTEND_URL="*" BACKEND_URL="*" docker run --rm -p 4567:4567 -it backend-flask
export FRONTEND_URL="*"
export BACKEND_URL="*"
docker run --rm -p 4567:4567 -it  -e FRONTEND_URL -e BACKEND_URL backend-flask
unset FRONTEND_URL
unset BACKEND_URL
docker run --rm -p 4567:4567 -it -e FRONTEND_URL='*' -e BACKEND_URL='*' backend-flask
```

Basically to update the order and to remove the "equal sign" to unset.

For some reason, I still don't trust Gitpod 100% to store my AWS secret access key so I also created a new _reference directory in my repo to place all my *pre-requisites*

As part of the backend configuration, I created the Notifications endpoint and added one static entry:

<img src="assets/2023-02-28-Notifications.png">


The last piece was to setup DynamoDB. This exercice is going to download a local copy of AWS DynamoDB for development. As I don't use the local credential manager (Gitpod), I needed to declare my AccessKey

```
export AWS_ACCESS_KEY_ID=""
export AWS_SECRET_ACCESS_KEY=""
export AWS_DEFAULT_REGION=us-east-1
```

I configured the Dynamodb container configuration in Docker and restarted the Gitpod workspace. Once everything was rebuilt, I set once more my Access Key and verified there was no Table crested yet using: 

```
aws dynamodb list-tables --endpoint-url http://localhost:8000
```

<img src=assets\2023-02-28-NoTable.png>

Copied the 100 Days of Cloud code to create a sample Table:

<img src=assets\2023-02-28-SampleTable.png>

And now the list-tables command shows an output

<img src=assets\2023-02-28-MusicTable.png>

I also updated my docker-compose.yml to include the PostgreSQL when deploying the Gitpod environment.

## Challenges ##

Added to the backend-flask Docker file the installation of "ps" to stop processes within the container.

```sh
RUN apt-get update && apt-get install -y procps
```

Also added the following lines to the frontend Dockerfile healthcheck.
```
HEALTHCHECK CMD curl --fail http://localhost:3000 || exit 1
```

## My Journey to the Cloud!

I am going to become a **Cloud Platform Engineer**

I am a good fit because **I am interested in enabling developers to get the infrastructure they need in a better and repeatable way via a self-service approach.**

| I will know | I will not get distracted by |
| ---- | ---- |
| Python | Web Development |
| API integrations | Database management |
| Ansible and Terraform | AR, AI or ML |
| Github Actions | Jenkins |
| AWS and Azure | GCP |


