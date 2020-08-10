The image binary classification application deals with the basic distinction between photos of airplanes and automobiles. The application was written in streamlit with a pre-trained VGG16 model from the Keras deep learning library.

The application is deployed in two forms: through a production app cloud platform (Heroku) and also through a container platform (Docker).

## Implementation

The first part of the implementation begins with training a custom VGG pre-trained model using a VGG16 Keras API. The VGG16 pre-trained model requires little custom dataset for training due to its transfer learning capabilities.

I scraped 100 photos of airplanes and 100 photos of cars from Google images. I organized them into two subfolders in the dataset and fed them into the VGG16 pre-trained model.

The model was customized by removing the final fully connected layers and adding new classifier layers for training. To faciliate the training of the model, I used a function for running test harness to evaluate performance of the model configuration. Once completed, the model is saved.

Next, I wrote the application using the streamlit library. Streamlit is a powerful open-source library that lets developers write data applications in fewer lines of code than standard web frameworks. The interface is simple: the user is given an image file picker to select or drag and drop a photo. Underneath it is a "Process" button that when clicked, will call a function which loads the saved VGG model to predict the class of the photo. The function will then return a string value that indicates whether the photo in question contains either an airplane, automobile, or neither.

The second part of the project implementation focuses on the deployment of the deep learning model. The deployment was done in two forms: through a Platform as a service (Heroku) and the other through a container (Docker). I chose Heroku because it supports most web development languages and frameworks and that the speed of deployment is faster for smaller web-based data apps.

Normally, training a computer vision model would take hours and would result into larger model file, but because I used a few custom images and that I used a pre-trained model for transfer learning, the size of the saved model is much smaller - at ~85-86MB. The other code files are smaller sizes so the entire codebase was committed to a Github repository.

From this point on, I deployed the data app to production using only Heroku. It was straightforward as the size of the project folder was relatively small. The deployed app is given here: https://airplanes-or-cars.herokuapp.com

![martymcfly](https://user-images.githubusercontent.com/3411100/86633685-f686f880-bf9e-11ea-94d3-45607d88d644.png)

Finally, I wrote a Dockerfile for building a docker image and running a container through a computer. Like Heroku, deploying through Docker is fast just by a few commands for running a container.

The app is also rewritten in Flask and successfully Dockerized and deployed to production: https://airplanes-or-cars-docker-flask.herokuapp.com/

![martymcfly](https://user-images.githubusercontent.com/3411100/89757005-0f109400-dab2-11ea-8338-69da014cabd1.png)

As the classifier is available as a container and a Heroku-deployed app, this project serves as a feasibility study not only of computer vision, but also of a comparison of both deployment options.

## Heroku or Docker?

Heroku is an intuitive PaaS to use and the the app in production is easy to access from the viewpoint of a non-technical user. Like any other web application, any user can enter the URL to view the app.

Beneath the surface, Heroku applications run on a lightweight container thanks to the Procfile (needs to be committed to the repo) that contains the commands to run at launch. However Heroku defines its underlying container so there are restrictions in how data apps are deployed. Additional deployment features are paid features.

Meanwhile, Docker (CaaS) offers developers flexibility in configuring containers for their applications. Developers can build their images on top of any other base image to customize the containers that their applications run on. Running containers is highly convenient because their applications can run as isolated pieces of software regardless of the machine it runs on.

The advantage of this is that a Docker container can then be hosted on a cloud platform in production. Usually they can be run on platforms such as Heroku, Azure, AWS, and Google Cloud Platform (GCP).

Both Heroku and Docker have their own pros and cons and they serve different purposes. Heroku applications are suited for developers who prioritize app development without worrying about hosting on servers and managing app delivery pipeline, whereas Docker applications allow for custom development and hosting/deployment options at much lower costs.

My experience with Heroku is that the deployment options are limited, but they are suitable for my interests in developing software and deep learning systems, but  I am certainly open to any situation that calls for running customized containers.

## Limitations and Challenges

My limitations and challenges are mostly rooted in the deployment process. While the classifier application is deployed on Heroku without trouble, my attempt to deploy to production via Docker did not help much. Like the IG Content Generator, the classifier containerized app encountered some errors when deploying to Heroku:

![martymcfly](https://user-images.githubusercontent.com/3411100/89724679-32fda800-d9d4-11ea-9eb7-522cfe1ba06d.png)

I also attempted to deploy the Streamlit containerized application to Heroku, however it does not recognize the port that was written in the exposed port number that was specified in the Dockerfile. This is not the case when working with Flask containers, but with Streamlit applications the container is not able to the port number from Heroku.

Finally, both Streamlit and containerized Flask versions of the app can only resize images of certain sizes to 224 * 224 * 3:

![martymcfly](https://user-images.githubusercontent.com/3411100/89756917-cbb62580-dab1-11ea-8ba0-fe51ab61241d.png)

## Some Ideas for Improvement
- pivot to more sophisticated classification use cases, such as multi-class classification on medical images to diagnose a disease
- apply image augmentation to create more diversity of images for training to reduce any potential overfitting or biases.
- add additional pre-trained classification models such as ResNet and Inception for comparison between them in performance.

## Conclusion

Image classification has evolved and matured as a deep learning application. Therefore there is much to explore from configuring pre-trained models and applying it to more business areas.

Development and deployment practices have not yet matured at this stage in the machine learning industry. Given the scope of the use case, it would make sense to deploy the classifier straight to Heroku without any need for containers. There's little to no configuration required and I could focus on development and tinker with different machine learning models while delivering results at speed.

I'm not discounting Docker, though. The Docker Hub repository hosts thousands or millions of images that a machine learning practitioner could quickly choose to base on, provide their specification such as ports to expose and dependencies to install, and the containerized application can run anywhere as long as a platform can accommodate it.
