# Sample MERN with Microservices



For `helloService`, create `.env` file with the content:
```bash
PORT=3001
```

For `profileService`, create `.env` file with the content:
```bash
PORT=3002
MONGO_URL=specifyYourMongoURLHereWithDatabaseNameInTheEnd
```

Finally install packages in both the services by running the command `npm install`.

<br/>
For frontend, you have to install and start the frontend server:

```bash
cd frontend
npm install
npm start
```

Note: This will run the frontend in the development server. To run in production, build the application by running the command `npm run build`




```
radeonxfx@DESKTOP-JIH47ND:~/GCP-MERN-APP$ gcloud auth configure-docker us-central1-docker.pkg.dev
WARNING: Your config file at [/home/radeonxfx/.docker/config.json] contains these credential helper entries:

{
  "credHelpers": {
    "asia-east1-docker.pkg.dev": "gcloud"
  }
}
Adding credentials for: us-central1-docker.pkg.dev
After update, the following will be written to your Docker config file located at [/home/radeonxfx/.docker/config.json]:
 {
  "credHelpers": {
    "asia-east1-docker.pkg.dev": "gcloud",
    "us-central1-docker.pkg.dev": "gcloud"
  }
}

Do you want to continue (Y/n)?  y

Docker configuration file updated.
radeonxfx@DESKTOP-JIH47ND:~/GCP-MERN-APP$ docker tag hello-service us-central1-docker.pkg.dev/heroviredacademics/my-repo/hello-service:latest
docker tag profile-service us-central1-docker.pkg.dev/heroviredacademics/my-repo/profile-service:latest
docker tag frontend us-central1-docker.pkg.dev/heroviredacademics/my-repo/frontend:latest
radeonxfx@DESKTOP-JIH47ND:~/GCP-MERN-APP$ docker push us-central1-docker.pkg.dev/heroviredacademics/my-repo/hello-service:latest
docker push us-central1-docker.pkg.dev/heroviredacademics/my-repo/profile-service:latest
docker push us-central1-docker.pkg.dev/heroviredacademics/my-repo/frontend:latest
```
