# test

<img
  src="/images/Screenshot_20221029_095552.png"
  alt="Alt text"
  title="Optional title"
  style="display: inline-block; margin: 0 auto; max-width: 300px">

# Genify SMS Templates Pipeline - A Guide

* The process does not cover ‘sender categories’
* Quantify frequency and relevancy!

# Components

1. SMS Clustering Endpoint: EZLoan API on Postman
1. Data Labeling Tool: Datasaur.ai
1. Labeled Data Processing Script: test.py

(bottleneck: is output from Postman directly importable into datasaur or needs processing?)

# Journey

1. Use Postman endpoint to produce sms-clusters and a subsequent list of unique candidate sms
1. Import the candidate sms into datasaur and add both token and category labels
1. Export the labeled sms and run through the data-labeling python script to get labeled sms\_templates in a formatted .json file

# Steps

**_In Postman_**

* Import the EZLoan API
* Under EZloan API, enter your username and authorization under the ‘variables’ section
* In the ‘body’, copy and paste the SMS list (in JSON format)
* Move over to the sms clustering step and press ‘Send’

**Output:** a list of top-n clusters and a subsequent list of unique SMS in a .txt file to be labeled

**Format of sms in the .txt:** List of unique candidate sms (one from each cluster) where each sms is in the following format:

    sender-name::body of the sms

**_In Datasaur_**

Account display name: hello@genify.ai

* Login to the Datasaur account using these credentials:
    *  Username/Email: rb4919@nyu.edu
    *  Password: genify\_SMS\_2233
* Create a new NER project
* Import the text file of the unique candidate SMS
<img
  src="/images/1.png"
  alt="Alt text"
  title="Optional title"
  style="display: inline-block; margin: 0 auto; max-width: 300px">  
* Import two label sets from Github: [sms\_token\_label\_set.json](https://github.com/genifyai/sms-labeling/blob/main/sms_token_label_set.json) and [sms\_category\_label\_set.json](https://github.com/genifyai/sms-labeling/blob/main/sms_category_label_set.json). More labels can be added to these label sets if needed.
<img
  src="/images/2.png"
  alt="Alt text"
  title="Optional title"
  style="display: inline-block; margin: 0 auto; max-width: 300px">
* In the extensions, turn on the ML-assisted labeling option (and any others if needed) to get suggestions. Pick a service provider such as CoreNLP NER and use the ‘predict labels’ option to get suggestions. Limitation: These suggestions, however, are limited to the labels known to the service provider and are not influenced by our own label sets
<img
  src="/images/3.png"
  alt="Alt text"
  title="Optional title"
  style="display: inline-block; margin: 0 auto; max-width: 300px">
  <img
  src="/images/4.png"
  alt="Alt text"
  title="Optional title"
  style="display: inline-block; margin: 0 auto; max-width: 300px">
* While labeling, use keyboard shortcuts. Press Ctrl + / to view all the keyboard shortcuts. Some useful shortcuts:
    * arrows to jump from one word to the next
    * Alt + label-set-number to alternate between label sets. For example, Alt+1 will choose the first label set
    * When labeling a word/token in the sms, press the number associated with that label to assign it via the keyboard
* For each sms, label the tokens/words in the sms using the sms\_token\_label\_set
* To label the sms-category, press on the left of the sms (on the sms number) to select the whole line and then add a category label to the sms using the  sms\_category\_label\_set

Example of labeled SMS:
<img
  src="/images/5.png"
  alt="Alt text"
  title="Optional title"
  style="display: inline-block; margin: 0 auto; max-width: 300px">

* Once labeling is complete, mark the project as complete and export the file as a JavaScript Object Notation (.json) - (DEPRECATED) file

Processing the (DEPRECATED) .json

* For this, a Python script called create-templates.py has been created
* Download this file from Github here
* Save the file in the same directory as your labeled .json file from datasaur
* Inside the script, replace the .json filename with the name of your .json file from datasaur and then save the script


* Run the script on your command line - it will save the final sms\_templates.json file in the same directory as your script
* Open the final templates file to review and ensure that templates are correctly formatted as per the format used by Genify

<img
  src="/images/6.png"
  alt="Alt text"
  title="Optional title"
  style="display: inline-block; margin: 0 auto; max-width: 300px">




Quantify how many sms templates we should get for a particular dataset

Out of x manually identified templates, the algorithm from Kareem enabled us to identify y

If y is above x, and y is relevant - that’s great

Can we give others clients access/permission to label? - without them needing an account? add another user to the account/another labeler has access to particular projects only without having an account of their own - use Genify account but has limited access

Requirements.txt file for the final script

SMS Templates Guide by Ramsha Bilal
