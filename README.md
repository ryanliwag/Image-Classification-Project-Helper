Incomplete

what is finetuning
preprocessing
training
validation
implementation

# Easy Image Classification

This is to help thesis groups to easily train, evaluate and implement a Image classifier by finetuning on vgg16 or vgg19 models.


## Dependencies

## Training

Create a dataset of images on which you would retrain the model with. In my instance I am working with a group to implement a mango sorting system that is reliant on computer vision. Supposedly we need to be able to classify ripness and quality. Mango Dataset was provided by Dr. Edwin Calilung and his associates in Department of Agriculture Philmech. 

Images should be contained in their respective class folders.
Your images should be contained in a folder split between training and validation. I just always go with the 80:20 ratio. you can also add a seperate testing directory to evaluate the model after training. 

I also uploaded my own script in ML-tools folder it helps to randomly sort and prepare my training, validation and testing dataset.

```shell
	python3 sort --srcdir "/images" #folder where images are seperated into classes
				 --destdir "/dataset" 
				 --per_train 70 #percentage of data split into training directory
				 --per_validate 20 #percentage of data split into validation directory
```
You're dataset should have a format as the one seen below.

```shell
  $TRAIN_DIR/dog/image0.jpeg
  $TRAIN_DIR/dog/image1.jpg
  $TRAIN_DIR/dog/image2.png
  ...
  $TRAIN_DIR/cat/weird-image.jpeg
  $TRAIN_DIR/cat/my-image.jpeg
  $TRAIN_DIR/cat/my-image.JPG
  ...
  $VALIDATION_DIR/dog/imageA.jpeg
  $VALIDATION_DIR/dog/imageB.jpg
  $VALIDATION_DIR/dog/imageC.png
  ...
  $VALIDATION_DIR/cat/weird-image.PNG
  $VALIDATION_DIR/cat/that-image.jpg
  $VALIDATION_DIR/cat/cat.JPG
  ...
```
Finally to train the dataset I made a program where it will help me easily retrain and finetune the
vgg16 and vgg19 models. Finetuning only occurs up to the last convolutional block for each model. 
This code took a lot of inspiration from Francois Chollet [blog](https://blog.keras.io/building-powerful-image-classification-models-using-very-little-data.html), read the blog to learn more about how finetuning was implemented here. So just like in  his blog this code goes through 2 steps. First is training the sequential dense layers and the second step is to finetune the last convolutional block with the last few layers.    

```shell
#Example Usage, you can change these parameters however you want
python3 train.py --model_label vgg16 \ 
		     	 --nb_steps_toplayer 20 \ 
		     	 --nb_steps_finetune 50 \
		     	 --training_data_dir "mango_dataset/training" \
		     	 --validation_data_dir "mango_dataset/validation" \
		     	 --nb_classes 3 \
		     	 --img_width 300 \
		     	 --img_height 300 \
		     	 --learning_rate 0.0001 \
		     	 --batch_size 1 \
		     	 --nb_training_samples 3000 \
		         --nb_validation_samples 600 \
		         --directory_location "/home/elements/Desktop" \ #directory to save the ouput
```
So if you are starting this script for the first time, it will take a while due to having to download the model weights at the beginning. Training may be a bit slower maybe due to not caching the bottlneck features, I am not a 100% on this. But depending on your dataset it is still better than training a CNN model from scratch.

After training you should see two directories inside the directory specified. Directory names should be "Charts and Graphs" and "Model and Weights". So inside these directories I included the model, its weights and graph visualization for the training accuracy and loss. 

![training accuracy and loss](https://raw.githubusercontent.com/username/projectname/branch/path/to/img.png)

## Evaluation



## Implementation


```shell

bazel-bin/inception/flowers_train  
--train_dir /home/elements/Desktop/THESIS/transfer learning on tensorflow/train_dir   
--data_dir /home/elements/Desktop/datasets/flags_tfrecords 
--pretrained_model_checkpoint_path /home/elements/Desktop/inception-v3-model/inception-v3/model.ckpt-157585  
--fine_tune=True  
--initial_learning_rate=0.001 
--input_queue_memory_factor=1

```

Following the slim model and its tutorial, the code for train_image_classifier.py can also be used to retrain the inception_v3 model. 


```shell

python train_image_classifier.py 
--train_dir /home/elements/Desktop/THESIS/train_dir 
--dataset_dir /home/elements/Desktop/datasets/flags_tfrecords 
--dataset_split_name=train 
--model_name=inception_v3 
--checkpoint_path /home/elements/Desktop/THESIS/checkpoints/inception_v3.ckpt 
--checkpoint_exclude_scopes=InceptionV3/Logits,InceptionV3/AuxLogits/Logits 
--trainable_scopes=InceptionV3/Logits,InceptionV3/AuxLogits/Logits


```

http://stackoverflow.com/questions/38947658/tensorflow-saving-into-loading-a-graph-from-a-file

http://stackoverflow.com/documentation/tensorflow/5000/save-and-restore-a-model-in-tensorflow#t=201608150606555812336

https://accounts.google.com/o/oauth2/auth?client_id=717762328687-p17pldm5fteklla3nplbss3ai9slta0a.apps.googleusercontent.com&scope=profile+email&redirect_uri=https%3a%2f%2fstackauth.com%2fauth%2foauth2%2fgoogle&state=%7b%22sid%22%3a1%2c%22st%22%3a%228ccf498b1dd9726abb4fc65a9fb48841d2ac8f839a38894253f92439cce9f01d%22%2c%22ses%22%3a%226ca4e02e7ab34b4b8de99685650a0b0e%22%7d&response_type=code

A lot of functions in the scripts provided have been deprecated. SO you will receive errors when using the latest model of tensor flow. but nonetheless the script will still run.

## still updating