The image binary classification application deals with the basic distinguish between airplanes and automobiles. The application is written in streamlit with a Keras model trained from VGG16 API.

The application is deployed in two forms: through a production app cloud platform (Heroku) and also through a contain platform (Docker).

## Implementation

The first part of the implementation begins with training a custom VGG pre-trained model using a VGG16 Keras API. The VGG16 pre-trained model requires little custom dataset for training due to its transfer learning capabilities.

I experimented with scraping 100 photos of airplanes and 100 photos of cars from Google images. I organized them into two subfolders in the dataset and fed them into the VGG16 pre-trained model.

The VGG16 pre-trained model was customized by marking a loaded layers as not trainable and instead adding new classifier layers for training. To faciliate the training of the model, I used a function for running test harness to evaluate how well the model can distinguish the contents in a photograph. Once completed, the model is saved.

Next, I wrote the application using the streamlit library. Streamlit is a powerful open-source library to write data applications in fewer lines of code than standard web frameworks. The interface is simple: the user is given an image file picker to select or drag and drop a photo. Underneath it is a "Process" button which when clicked, will call a function which loads the saved VGG model to predict the class of the photo. The function will then return a string value that indicates the user whether the photo in question is classified as airplane, automobile, or neither airplane nor automobile.

However, the focus of the the project implementation is on the deployment of the deep learning model. The first approach was through deploying to Heroku. I chose Heroku because it supports most web development languages and frameworks and that the speed of deployment is faster for smaller web-based data apps.

Normally, training a computer vision model would take hours and would result into larger model file, but because I used a few custom images and the VGG model used was pre-trained, the size is much smaller - at ~85-86MB. The other code files are smaller sizes so the entire codebase was committed to a Github repository. 

From this point on, deploying the data app to production to Heroku because very straightforward. The deployed app is given here: https://airplanes-or-cars.herokuapp.com

![martymcfly](https://user-images.githubusercontent.com/3411100/86633685-f686f880-bf9e-11ea-94d3-45607d88d644.png)

Finally, I wrote a Dockerfile for building a docker image and running a container through a computer. Like Heroku, deploying through Docker is fast just by a few commands for running a container.

## Heroku or Docker?

Heroku is an intuitive PaaS to use from the viewpoint of a non-technical user. It appears like any other web application: enter the URL and the app is accessible.

Beneath the surface, Heroku applications run on a lightweight container thanks to the Procfile that contains the commands to run at launch. However it defines its underlying container so there are restrictions in how data apps are deployed. Certain deployment features are priced

Meanwhile, Docker (CaaS) offers developers flexibility in configuring containers for their applications. They have a container standards and developers can build their images on top of any other base image to customize the containers that the applications can run on.

Both have their own pros and cons and they serve different purposes. Heroku applications are suited for developers to prioritize app development without worrying about hosting on and managing app delivery pipeline, whereas Docker applications allow for custom development and hosting/deployment options at much lower costs.

My experience with Heroku is that the deployment options are limited, but they are suitable for my interests in developing software and deep learning systems, but  I am certainly open to any situation that calls for more customizations when running containers.

## Some Ideas for Improvement
- pivot to more sophisticated classification use cases, such as multi-class classification on medical images to diagnose a disease
- add additional pre-trained classification models such as ResNet and Inception for comparison between them in performance.

## Conclusion

Image classification has evolved and matured as a deep learning application. Therefore there is much to explore from configuring pre-trained models and applying it to more business areas. Meanwhile,having knowledge of both Heroku and Docker enable me to choose how and where to deploy depending on business needs.
