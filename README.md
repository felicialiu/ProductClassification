# ProductClassification

Author: Felicia Liu

Date: August 14, 2019

## Prerequisites

There is a `requirements.txt` file for all the packages I installed for my dev environment.

```
pip install -r requirements.txt
```

## Folder structure

`DownloadImages.ipynb` contains some code to download all images in the data set.

`ExploreDescriptions.ipynb` contains the code to inspect the product descriptions, determine what preprocessing to do, the results of topic modeling, and a termite plot visualizing the most important terms and their distribution over topics.

`Modeling.ipynb` contains partial code for label spreading based on 100 hand-labeled data and a baseline logistic regression model used for making predictions.

`product_data_result.json` contains 100 hand-labeled labels and 900 labels predicted with a baseline approach.

## Main idea for approach
Since the challenge asks for you to classify data without a pre-labeled data set, my initial focus was to think about how I can create an initial small labeled subset and build up from there.

I wanted to use topic modeling to see if there are any latent topics in the product descriptions and see how these map to the product categories we need to predict. In `ExploreDescriptions.ipynb` you can see the top words for each topic and some of the topics corresponded to the category _Tops_ and _Dresses_, but some of the topics covered descriptions in foreign languages, or focused on products from the Men's or Girls' department. I used the results for topic modeling to prelabel the product descriptions with classes the topics correspond to. Some of the topics were not informative, so I did not cast all the resulting groupings to one of the product descriptions.

Then, with the data having been pre-labeled, I wanted to focus on hand-correcting 100 of these data points.

Ideally, at this stage I would use label spreading to iteratively label the data set I got. Unfortunately, I ran out of time to get this working properly.

An alternative approach involving human-in-the-loop active learning (that would have taken more time than available) would be to train a model on the initial 100 labeled data points, use that to make predictions on a next round of 100 labeled points, correct these by hand, and train a new model on the total of 200 labeled data points and repeat this process.

By plotting the number of labeled data points and the F1 score, I could make an estimate as to how much data I need to label to have a reasonable model. I would then ideally have a good model to make predictions with.

## Answers to challenge questions

### Why are you designing the solution in this way?
The data contains 1000 images and product descriptions. My approach was to focus on the product descriptions, given that there is limited time and that there were 200 images that could not be downloaded. Since there are no labels, I tried to hand-label a set of the data myself and find ways of using a hand-labeled set to get the full data set labeled.

### What are the aspects that you considered when designing?
Since there were no provided labels, I focused on finding a way of doing active learning to create more labels based on an initial hand-labeled subset.

The product descriptions data was also quite messy, there were a few examples that were in a different language, and there were a few examples that contained HTML. I extracted text from the HTML, but could have done translation to work with the foreign language descriptions.

### What are the cases your solution covers, how are they covered and why are they important?
My solution covers preprocessing to take care of the messy product descriptions and aimed to find a solution to get labels.

Preprocessing was used to remove noisy words (such as "free available shipping entire selection"), extract text from HTML, normalize whitespace, remove unnecessary newlines, and lowercase all words so tokenization would become easier. It is important to work with a clean and correct data set for modeling, otherwise it would be garbage in, garbage out.

Getting labels is important because a supervised learning approach is more straightforward than other approaches.

Clustering or using topic modeling was not able to uncover groupings that could be mapped to all product categories. I decided to use categories the groupings did map to as initial labels for my data set to make hand-correction faster.

### What are the cases your solution does not cover and what are the ways you can extend your current solution for them?

My solution does not make use of the product images. To include these, I'd want to build a separate model to classify the images. For image classification I would want to use an approach with a convolutional neural network. I could use the labels I would get from active learning with product descriptions and these would then carry over to the product images.

My solution also does not produce labels for the full dataset. With a full data set I could try out different models, use cross-validation, and where needed regularization to get to one model that would produce accurate predictions for the description data set that could then be used for image classification.