Creating and Deploying a Google Kubernetes Engine Cluster
Introduction
In this hands-on lab, we’ll use the Cloud Shell command line from start to finish: first containerizing an app and then deploying and exposing it.

Solution
On the lab page, right-click Open GCP Console and select the option to open it in a new private browser window (this option will read differently depending on your browser — e.g., in Chrome, it says "Open Link in Incognito Window"). Then, sign in to Google Cloud Platform using the credentials provided on the lab page.

On the Welcome to your new account screen, review the text, and click Accept. In the "Welcome L.A.!" pop-up once you're signed in, check to agree to the terms of service, choose your country of residence, and click Agree and Continue.

Enable the Kubernetes Engine API
From the main Google Cloud console navigation menu, choose APIs & Services > Library.
Search for "Kubernetes", and enable the Kubernetes Engine API.
Clone the Repo to Access the Files
Activate the Cloud Shell by clicking the icon in the top row.
Clone the GitHub repository:

git clone https://github.com/linuxacademy/content-gc-essentials
Change directory:

cd content-gc-essentials/gke-lab-01
Create the Docker Image
Still in the Cloud Shell, build the Docker image:

docker build -t la-container-image .
Configure the Docker command line to authenticate to Container Registry:

gcloud auth configure-docker
Tag the registry image (you can replace your project ID where indicated by highlighting the code in yellow in the Cloud Shell, which will automatically add it to your clipboard):

docker tag la-container-image gcr.io/<PROJECT_ID>/la-container-image:v1
Push the image forward (substituting your project ID where indicated):

docker push gcr.io/<PROJECT_ID>/la-container-image:v1
Create the Kubernetes Engine Cluster
Configure the zone:

gcloud config set compute/zone us-central1-a
Create the clusters:

gcloud container clusters create la-gke-1 --num-nodes=4
Get authentication credentials:

gcloud container clusters get-credentials la-gke-1
Deploy the app (substituting your project ID where indicated):

kubectl create deployment la-greetings --image=gcr.io/<PROJECT_ID>/la-container-image:v1
Expose the Deployed Workload
Expose the deployment:

kubectl expose deployment la-greetings --type=LoadBalancer --name=la-greetings-service --port=80 --target-port=80
Check its status:

kubectl get services la-greetings-service
When an external IP address has been generated, copy it and paste it into a new browser tab. It should result in our application.

Conclusion
Congratulations on completing this hands-on lab!
