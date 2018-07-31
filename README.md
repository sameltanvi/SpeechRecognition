
Android Speech Recognition
===================================

>>For a pretrained model demo,

Download the source code, extract & import the folder and run it in Android Studio. 
If you do not have Android Studio you can get it from, https://developer.android.com/studio/

This code demonstrates speech recognition through basic functionalities of Camera. It records, recognises a short one second audio file and then gives control to Camera2 API. The Camera2API then clicks and saves the picture to the file directory. 

Introduction
------------

Objective- Basic app running on Tensorflow model recognising set of predefined words and displaying them on UI.

Application classifies a one second audio clip as a word from the predefined set and displays the identified word on the screen The set of words are ‘on’, ’off’, ’stop’, ’go’, ‘left’, ‘right’, ‘up’, ‘down’, ‘yes’ or ‘no’. 

NB: The audio clip needs to be of only one second and words from the above set only at this stage. 

Before you start,
	>Make sure you have completely understood and followed the tutorial on setting up the environment. Tensorflow won’t run if there are errors in the environment setup. 

	>Download the source code (Download zip/clone it using Git bash - git clone <url>) 
	> Make sure the android running is 6 or more (Android 6 and upwards ask for permissions to record audio before running the app)
	>Gradle 3.0.1 is required (along with python and other dependent technology versions mentioned earlier). 
	>Connect your device and run it on it via emulator. 
	>Speak one second word from the set and the app should work fine! 

**INSTALLATION OF TENSORFLOW


Python download

Make sure your version of python is 3.5 or higher (Tensorflow is not supported otherwise). 
	For checking your python version
	> Open terminal
>Type  ‘python’ and hit enter to open the python shell
>Type the following code
	Import sys
	Print (sys.version)
>The version should be displayed



	

Download python version 3.5 or 3.6 if it’s not already installed in your pc

Version 3.5- https://www.python.org/downloads/release/python-352/

Version 3.6- https://www.python.org/downloads/release/python-362/

2. Python installation

Make sure to tick ‘Add Python 3.5 to PATH’  while installation. 
	


3. Upgrading pip

Make sure you have latest version of pip installed, if not type
	'python -m pip install --upgrade pip'


4. Installing tensorflow

	>Goto command prompt
	>Type
		Pip install tensorflow
OR
Pip3 install tensorflow

	On successful installation you get a screen like this




5. To verify installation

	>Type ‘python’ and hit enter
	This should bring you to the python shell which looks like


	>Type 
		Import tensorflow as tf
	If this gets you to the next line without any errors, your installation is successful.

6. Demo Program using TensorFlow

>In the python shell type
Import tensorflow as tf
Hello = tf.constant (‘Hello’, ‘TensorFlow’)
sess=tf.Session()
//This may produce a warning. 
Print (sess.run(hello)) 

	>If everything went fine, you should see ‘Hello TensorFlow’ as your output


		
This verifies that your installation is correct and you are now ready to use tensorflow in your programs. 


**Training set tutorial

>Download tensorflow source code from https://github.com/tensorflow/tensorflow

>Open command prompt and go to this location:

\tensorflow\tensorflow\examples\speech_commands

#You have to run the train.py script in this folder. 
#This script will start by downloading the dataset. Specify this URL in the --data_url parameter to explicitly download from tensorflow site and not the Google cloud one. 

Type

>python train.py --data_url=”http://download.tensorflow.org/data/speech_commands_v0.01.tar.gz” --data_dir=”.”



#--data_url refers to the link from which the dataset is to be downloaded. Depending on your OS, use either of the following links. Note, this is only a recommendation, either of the below should work fine. 

Windows- http://download.tensorflow.org/data/speech_commands_v0.01.tar.gz 

OR

Mac-https://storage.cloud.google.com/download.tensorflow.org/data/speech_commands_v0.01.tar.gz


#data_dir refers to the directory where the file should be downloaded to. Data_dir=”.” says that it should be downloaded to the current directory itself. 

This may produce a warning if you are using tensorflow on CPU rather than GPU and may look something like this, which can be ignored.






If everything is fine, the download should start and the command prompt should display this.



Note, the tar file is around 1.4GB and hence it may take some time depending on your machine’s speed. 

If you face an issue in downloading the file, download it manually from the links above and place it in your localhost folder. Then, give your server’s link in the data_url parameter and specify the directory on which it is required to download as explained above.



The dataset should start to download as shown below.



After this, the training should start as shown in the picture below. Note, you can see the step number in each line, which tells you how far have you gone in your training. There are about 18000 steps in total. 



Rate 0.001000 shows the learning rate is the model that controls the network’s weight updates. 

Accuracy 11.0% tells how many classes were correctly predicted in this step. It would fluctuate a bit, but should increase gradually as the training progresses. 

Cross entropy 2.511891 is the score that is obtained after comparing vector of scores from current training to correct labels and should downgrade as we go further with the training. 

After every 100 steps, the model stores the stage in a checkpoint.The picture below shows the 100th checkpoint. 



 If by any chance, the machine stops in middle of your training, you can restore from the last checkpoint by passing 
--start_checkpoint=/tmp/speech_commands_train/conv.ckpt-100
As a command line argument. This will start the training from that checkpoint. 

You may also get a logging error as shown below. 


This is because of Windows OS update and has no effect on the training as it continues afterwards. 

After every 400 steps, a confusion matrix is displayed in the console.



Each column represents samples that were predicted to be each label. The labels used in our case are “silence”, “unknown”, “yes”, “no”, “up”, “down”, “left”, “right”, “on”, “off”, “stop” and “go”. So the first column represents all clips that were thought of being silence, second of unknown and so on. 

Each row represents clips by their correct truth labels. The first is silence, second unknown and so on. 

The matrix gives us a good estimate of mistakes the model is making. In the example, we see that there are 4 places in the first row wherein the model makes mistakes of finding words where it was silence. 

Our target is to print a confusion matrix wherein all entries are zero apart from the diagonal line through the center. The confusion matrix helps us identify where the model is making most mistakes and we can help reduce it by providing more data for such circumstances, training with it and lastly resorting to cleaning of categories in order to increase efficiency. 


After the confusion matrix, we see a line as shown below,



We divide about 80% of our data for training, 20% for validation (evaluating accuracy) and 10% of another set for testing. The above logging line shows accuracy of model when run on validation set which should be close to the training accuracy. 

Be observant of the validation accuracy which should increase with the training accuracy, and if not, it means overfitting is taking place i.e network is memorizing inputs during training and it is only learning things about training clips and not broader generalizing patterns. 

We can see that the training proceeds to next step, in our case Step 401 without having to do anything manually. This continues for next 100 steps, after which the checkpoint is stored as explained earlier. And for every 400th step, a confusion matrix is generated. It is worth seeing if the validation accuracy increases with the training accuracy.

Some of the observations recorded randomly:

For 400th step:


The validation accuracy is 24.5%
 

For 1600th step:



The validation accuracy is 59.5%

For 2000th step:



The validation accuracy is 63.7%

For 4800th step:



The validation accuracy is 79.5%

For 6000th step:



The validation accuracy is 81.5%

For 6400th step: 



The validation accuracy is 82.1%

For 6800the step,



The validation accuracy is 83.3%

For 7200th step,



The validation accuracy is 83.5%

For 9600th step,


The validation accuracy is 85.7%

For 10,000th step,



The validation accuracy is 85.8%

For 12400th step,



The validation accuracy is 87.6%

For 16400th step,

The validation accuracy is 88.5%

We can see that the confusion matrix gets more stabilized as steps pass. As explained earlier, our final aim is to have all elements except the diagonal 0 which would mean considerable improvement in the model. 


As we can see, the validation accuracy is increasing along with the training accuracy which means our model is performing good when tested on the data reserved for validation only. This tells us the problem of overfitting is not occuring. 

When the training has finished, you come to 18000th checkpoint which looks like




It then prints out the final accuracy, which in our case is 88.4%




We need to now convert this into compact format to be tested on our phones. 

Type
	>
python tensorflow/examples/speech_commands/freeze.py 
--start_checkpoint=/tmp/speech_commands_train/conv.ckpt-18000 
--output_file=/tmp/my_frozen_graph.pb


If successful, you get a message like
>converted 6 variables to constant ops. 

Now we can test the model by any audio script from the dataset. 
For example, if we take ‘left’ as our keyword we can test it by
		
		
python tensorflow/examples/speech_commands/label_wav.py 
--graph=/tmp/my_frozen_graph.pb
--labels=/tmp/speech_commands_train/conv_labels.txt 
--wav=/tmp/speech_dataset/left/a5d485dc_nohash_0.wav

This prints out the accuracy scores for that keyword

This says it identifies the word as left for 93.78% which is a good score. 

Testing any other random audio file similarly,we can predict the accuracy. 

**Now, put the frozen graph and the labels text file into the android/assets directory and run the application with your trained model. 




