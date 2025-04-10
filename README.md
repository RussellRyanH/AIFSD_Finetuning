# How to use my fine-tuning files

Note: I designed most of this project to work locally on my machine. The notebooks should also function when run in Google Collab, but some of the import statements may need to be adjusted. I have attempted to make note of this in the notebooks where applicable.

The CodeBLEU calculation is the only notebook that must be run on Google Collab.

## 1. Set up local project structure

Ensure that you have all of the following notebooks in the same directory

- Data_Processing.ipynb
- CodeT5_Finetuning.ipynb
- Model_Evaluation.ipynb

Make sure the following folders are in the same directory as the Jupyter notebooks

- data
- evaluation_data
- processed_data
- Trained_Model

If running my code locally, create a new python environment and run

``` pip install -r requirements.txt ```

in order to install all of the project dependencies. The version of python I used for executing the files locally was Python 3.13.2

## 2. Prepare the datasets

Ensure that the data folder has the following files:

- ft_test.csv
- ft_train.csv
- ft_valid.csv

Now open Data_Processing.ipynb

Execute this notebook in order to process and flatten the data. 

Note: One of the steps of this process is filtering out any examples that have problems. For example, there are 47 examples that I caught where the target statement is not actually an if statement - instead it just has a variable whose name starts with "if". There are also a few examples that use a regular expression, but the pattern is not enclosed in a raw string. These examples (when using Python 3.13.2) would raise a SyntaxWarning due to unrecognized escape sequences in the regex pattern, so I removed them from the dataset. When I was doing my testing, if I was using a different version of Python, sometimes this SyntaxWarning was not raised, so these examples would remain in the dataset. Overall, this is fine, but if you run my code using a different version of Python, you might get slightly different results because these examples may be left in the dataset.

After running all cells in Data_Processing.ipynb, the processed datasets should be saved as csv files in the processed_data folder

## 3. Train the model

Open CodeT5_Finetuning.ipynb

NOTE: If you decide to run my notebooks in Google Collab, you will need to uncomment the lines installing the correct versions of the different libraries, and the step to initialize WandB can be skipped.

Run the notebook. This is where the model will be trained on the fine-tuning data. 

The trained model and the tokenizer will be saved to the Trained_Model folder in order to be used in the model evaluation step

I have upload my trained model to Google Drive. Anyone with a W&M email should be able to access it at 

https://drive.google.com/drive/folders/1xU4C2Q5oFpwMxMNzsZ2V-OwUDPPuHplf?usp=drive_link

Which should allow you to load my model to be able to recreate my results.

## 4. Prepare for model evaluation

Open Model_Evaluation.ipynb

Note: SImilarly to the above, if you are running these notebooks in google collab, there are extra lines for you to uncomment that will allow the installation of the correct libraries

This notebook will load the model from the Trained_Model folder, and will generate the predictions based on the examples in the test set. 

Then the test data along with the model's predictions are saved into the evaluation_data folder to be used in subsequent analysis.

## 5. Evaluate the Model

Open CodeBLEU_Evaluation.ipynb IN GOOGLE COLLAB

I could not get the CodeBLEU calculation to function on my local machine, so this section must be done in google Collab.

Upload the test_data.csv file created in the previous section to Google Drive

When running the notebook, be sure to update the path to test_data.csv in order to reflect the configuration of your Google Drive.