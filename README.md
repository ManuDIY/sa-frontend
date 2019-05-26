# Continous Integration (CI)

Pipleline pre-requisites
- Private Docker registry - to publish artifacts
- Gitlab hook - build-trigger & chat notification
- Rocket.Chat - for notification

Adopt the file according to your build pipeline
Change env variable REACT_APP_API_URL to point to differnt API endpoint 
# Local build

## Starting the Web App Locally
` $ yarn start `

## Building the application
` $ yarn build `

## Building the container
` $ docker build -f Dockerfile -t $DOCKER_USER_ID/sentiment-analysis-frontend . `

## Running the container
` $ docker run -d -p 80:80 $DOCKER_USER_ID/sentiment-analysis-frontend `

## Pushing the container ###
` $ docker push $DOCKER_USER_ID/sentiment-analysis-frontend `

