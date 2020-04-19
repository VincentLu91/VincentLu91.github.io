# Project: My Attempt to create an Instagram Content Generator

During the months of February and March, I set out to build out a project to solve a problem. To say the least, the problem has remained unsolved for years, and I feel that my first stab at it would pave the way to enable commercial creativity for anyone regardless the budget.

Allow me to describe the background context behind the project...

## Content Marketing

Content marketing has evolved over the past 20 years through new forms and media. The first known form is a blog, then came video content. However, new forms have sprung up and became commonplace as well, including social media and podcast. All forms of content are now freely available to businesses to build audiences. 

One of the most popular and prevalent forms of content marketing in the last 5 years is through social media, which is now known as socia media marketing. With platforms including Snapchat, Twitter, Tik Tok, Facebook, and Instagram, entrepreneurs and influencers can tap into networks of connected users to establish tribes, while providing value in previously unseen ways.

In 2020, there is no doubt that there are more users active on social media than ever. While engaging users through content on social media has become a marketing necessity, the cost of the activity of engaging users has by no question gone up. The average user is looking for high quality content including video, gifs, photos, text, and stories. This cannot be anymore true than on Instagram, where 95 million posts are uploaded each day to compete for the attention of over 100 million users.

For established brands, this is not an issue, but anedotally, the cost of creating quality relevant and compelling content is going out of reach for cash-strapped or time-strapped businesses.

I have come to realize this years ago when trying out different types of businesses. Thus, I decided to create a proof of concept data app to attack the problem of creating visual and caption content, catered to businesses looking to build an audience on Instagram.

## Enter: The Instagram Content Generator

Think of this is the Content Generator v0.1.

Having spent weeks building it, this is my attempt to having a generator to create content that could potentially help someone.

I built this project with one goal in mind: to enable businesses - bootstrapped startups or solo businesses alike - to efficiently automate content creation while allocating the rest of their resources on value-producing, cashflow-generating activities.

## The Architecture of the Instagram Content Generator

The IG Content Generator is composed of two essential components, a pre-trained Generative Adversarial Network (GAN) for generating visual content, and a pre-trained GPT-2 network for generating caption content.

The GAN used is an existing Keras implementation of a pre-trained Progressive Growing GAN (PGGAN). The PGGAN works by starting with convolutional layers that process 4x4 images. Later on, the PGGAN incrementally adds in new layers with images of increasing resolution, effectively "fading in" images for learning details in visual structures. This is the "progressive" aspect of building a GAN to generate high resolution, photorealistic images.

The GPT-2 model used is a smaller model with 124M parameters but is resource efficient. It is capable of generating near-coherent text by way of natural language processing.

To prepare for training both deep learning networks, I have scraped over 20,000 Instagram posts. Due to the scope of this project, I limited the feasibility study to only landscape content of tree posts, using the hashtag #treesofinstagram.

The training process is to account for the feasibility of the project as well. Since the IG Content Generator is made up of the GAN and the GPT-2, I had to train them separately. The GAN training process was intensive. I took the images and resized them all to be 1024x1024 resolution so that they are processed for training (the GAN is limited to training on images of a certain size). I then zipped these images into an hdf5 file (the GAN does use Tensorflow in one of the codebases, but the implementation is mainly Keras) so the GAN could begin training on it. The GAN was trained for 10000 epochs, with model snapshots saved every 10 epochs. 

Further to that, I rented out a virtual GPU - a V100 - to train the GAN. The process took 1 day and 14 hours to completely train with a final model outputted at the end. As part of the experiment, I trained both with and without pre-trained weights that were previously trained on the celebA HD dataset, and the quality of the outputs are the same.

For the GPT-2 training, I took all the captions that I scraped and I combined them into a single text file. This is because I learned that applying the GPT-2 on 20,000+ text files for training was unnecessarily long. I then trained the GPT-2 on the text file using a jupyter notebook on Google Colab. In contrast to the GAN training time, the GPT-2 training time took 2-3 minutes to generate a model.

With both models trained, I wrote a small data app to show the auto generation of visual and text content. 

You will find the demonstration of it here: https://youtu.be/OAgeJVUTXyw

## Some Limitations

Generative Adversarial Networks have only emerged in the mid-2010s, and it wasn't until 2017 that we have started to see state-of-the-art results. Up to this time, it is still incredibly difficult to replicate such results as those whose networks are created by the researchers behind them.

The GAN network has been capable of generating somewhat plausible images of trees, but it is not capable of generating photorealistic images. This is due to several reasons.

One, is because I do not have access to the same type of hardware used by the teams that produced the high-resolution GANs. The hardware requirements are otherwise expensive to attain or incompatible with the environment that I use for coding and training.

Second, is there are multiple code implementations and most of them are difficult to understand. Some documentations describe empirical instructions for reproducing such results but they may leave out certain details that otherwise would have been helpful to the machine learning practitioner.

Third, the required libraries and the specific versions for generating images are not always compatible and oftentimes unspecified in documentations. Much experimentation was required for getting the GAN to train.

These challenges made up the bulk of the project time. 

For text generation, the GPT-2 is able to produce cohesive sentences and hashtags. But because I trained it separately from the GAN, it generates text with no relevant understanding to the image generated in question. While there are image-to-text generation techniques, the GPT-2 is not capable of such use case. Conversely, traditional image-to-text techniques produce generic, descriptive text without the style of the caption found in the dataset.

Instagram captions require up to 30 hashtags in order to post. What I found from running the data app is that the GPT-2 tends to generate more than 30 hashtags, and also some hashtags duplicate multiple times in text output.

## Plans for future

Until GANs continue to advance and evolve, there is not much certainty. However, I plan to continue work on this project when there is potential to experiment with cutting edge GAN networks as well as when required hardware becomes accessible for training on the cloud cheaply. 

As addressed above, I feel that limiting the number of hashtags is also important, while enforcing constraints on the GPT-2 by not duplicating hashtags in resultant text. As well, I plan to take the opportunity to experiment with image-to-text language models while preserving the style of the captions from existing Instagram Posts whenever the possibility arises.

For feasibility studies, I plan to train on and generate diverse types of content. Trees are a good start to generating mood boards, but the Generator has potential to create quote posts, product placements, and promotional posts. As well, I plan to expand the GAN's capabilities to generate Instagram Stories, as they lend themselves to high rates of call to action from brand followers.

The project is currently available in the form of a data app, but from the production standpoint my next step would be to convert it into a web application and mobile application. This enables the use case for brands to accordingly select and generate a type of content of their choice before sharing on Instagram.

## Conclusion

While this version 0.1 is far from the expected quality, it demonstrates the ability to generate social media content on command. Given that GANs and language models have emerged in recent years, I am not surprised if working with them to solve the content marketing problem will be like grabbing a tiger by the tail. 

GANs are extremely difficult to train, and text generation approaches have a long way to go. But when implemented as intended, the Generator will has enormous potential to help business owners and bootstrapped startups to efficiently content on demand.

One more thing to note, this is not intended to replace filmographers or photographers. This is intended to help businesses free up resources by having the Generator to "think up" content based on its training on existing Instagram posts. Moreover, there is always potential for visual and text discrepancies in AI-generated content.
