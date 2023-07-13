# Rekomendasi pasar barang bekas di Batam
In this code pattern, we walk you through a working example of a web app that utilizes multiple Watson services to create a better customer care experience.

Using the Watson Discovery Smart Document Understanding (SDU) feature, we will enhance the Discovery model so that queries will be better focused to only search the most relevant information found in a typical owner's manual.

Using Watson Assistant, we will use a standard customer care dialog to handle a typical conversation between a customer and a company representitive. When a customer question involves operation of a product, the Assistant dialog skill will communicate with the Discovery service using an Assistant search skill.


# What is an Assistant Search Skill?
An Assistant search skill is a mechanism that allows you to directly query a Watson Discovery collection from your Assistant dialog. A search skill is triggered when the dialog reaches a node that has a search skill enabled. The user query is then passed to the Watson Discovery collection via the search skill, and the results are returned to the dialog for display to the user.

Click here for more information about the Watson Assistant search skill.
![gambar](https://github.com/alwayne23/capstone_projek/assets/138902919/7267d31a-97b8-4a4f-8f56-34403d999a06)

  1. The document is annotated using Watson Discovery SDU
  2. The user interacts with the backend server via the app UI. The frontend app UI is a chatbot that engages the user in a conversation.
  3. Dialog between the user and backend server is coordinated using a Watson Assistant dialog skill.
  4. If the user asks a product operation question, a search query is issued to the Watson Discovery service via a Watson Assistant search skill.

# langkah langkah
  1. Clone the repo
  2. Create Watson services
  3. Configure Watson Discovery
  4. Configure Watson Assistant
  5. Add Watson service credentials to environment file
  6. Run the application


# 1. clone the repo
https://github.com/IBM/watson-assistant-with-search-skill
# 2. create watson service
Create the following services:

    Watson Assistant
    Watson Discovery

The instructions will depend on whether you are provisioning services using IBM Cloud Pak for Data or on IBM Cloud.

Click to expand one:
  1. IBM Cloud Pak for Data
  2. IBM Cloud
# 3. configure watson discovery
Start by launching your Watson Discovery instance. How you do this will depend on whether you provisioned the instance on IBM Cloud Pak for Data or on IBM Cloud.

Click to expand one:
  1. IBM Cloud Pak for Data
  2. IBM Cloud
Create a project and collection

The landing page for Watson Discovery is a panel showing your current projects.

Create a new project by clicking the New Project tile.
![gambar](https://github.com/alwayne23/capstone_projek/assets/138902919/a6f28a2f-d817-4b1d-8d12-c0877a8b10f0)
Give the project a unique name and select the Document Retrieval option, then click Next.
![gambar](https://github.com/alwayne23/capstone_projek/assets/138902919/6bd6c47a-fd1d-40dc-9b04-192e0d88c53a)
For data source, click on the Upload data tile and click Next.
![gambar](https://github.com/alwayne23/capstone_projek/assets/138902919/0339b965-b223-49ce-a349-bd3406944f90)
Enter a unique name for your collection and click Finish.
# import the document
On the Configure Collection panel, click the Drag and drop files here or upload button to select and upload the ecobee3_UserGuide.pdf file located in the data directory of your local repo.
![gambar](https://github.com/alwayne23/capstone_projek/assets/138902919/cfa1e7b8-a244-4d46-ae3f-819b600c28a1)
Once the file is loaded, click the Finish button.

Note that after the file is loaded it may take some time for Watson Discovery to process the file and make it available for use. You should see a notification once the file is ready.
# acces the collection 
To access the collection, make sure you are in the correct project, then click the Manage Collections tab in the left-side of the panel.
![gambar](https://github.com/alwayne23/capstone_projek/assets/138902919/88d34e4a-4748-4318-a5a1-f3f075f9c99e)
Click the collection tile to access it.
![gambar](https://github.com/alwayne23/capstone_projek/assets/138902919/56b9fee5-9bf2-4a3a-89cd-bc9574fbbd41)
Before applying Smart Document Understanding (SDU) to our document, lets do some simple queries on the data so that we can compare it to results found after applying SDU. Click the Try it out panel to bring up the query panel.
![gambar](https://github.com/alwayne23/capstone_projek/assets/138902919/90e8008d-0015-46c5-b6b9-e24b250d7274)
Enter queries related to the operation of the thermostat and view the results. As you will see, the results are not very useful, and in some cases, not even related to the question.
# Annotate with SDU
Now let's apply SDU to our document to see if we can generate some better query responses.
![gambar](https://github.com/alwayne23/capstone_projek/assets/138902919/c459a7a4-9f99-4918-adfd-790365433aa9)
From the Define structure drop-down menu, click on New fields.
![gambar](https://github.com/alwayne23/capstone_projek/assets/138902919/8235ec83-e052-4721-98df-a2c59a34a9f0)
From the Identify fields tab panel, click on User-trained models, the click Submit from the confirmation dialog.

Click the Apply changes and reprocess button in the top right corner. This will SDU process.

Here is the layout of the SDU annotation panel:
![gambar](https://github.com/alwayne23/capstone_projek/assets/138902919/e35a3c37-f30b-4a82-b475-3104eece376f)
The goal is to annotate all of the pages in the document so Discovery can learn what text is important, and what text can be ignored.

    [1] is the list of pages in the manual. As each is processed, a green check mark will appear on the page.
    [2] is the current page being annotated.
    [3] is a graphic display of the same page, but allows you to select regions that you can label.
    [4] is the list of labels you can assign to the graphic page.
    Click [5] to submit the page to Discovery.
    Click [6] when you have completed the annotation process.

To add/change a label, select the new label, then click on the text areas in the graphic page to apply it.

As you go though the annotations one page at a time, Discovery is learning and should start automatically updating the upcoming pages. Once you get to a page that is already correctly annotated, you can stop, or simply click Submit [5] to acknowledge it is correct. The more pages you annotate, the better the model will be trained.

For this specific owner's manual, at a minimum, it is suggested to mark the following:

    The main title page as title
    The table of contents (shown in the first few pages) as table_of_contents
    All headers and sub-headers (typed in light green text) as a subtitle
    All page numbers as footers
    All circuitry diagram pages (located near the end of the document) as a footer
    All licensing information (located in the last few pages) as a footer
    All other text should be marked as text.

Click the Apply changes and reprocess button [6] to load your changes. Wait for Discovery to notify you that the reprocessing is complete.
Next, click on the Manage fields [1] tab.
![gambar](https://github.com/alwayne23/capstone_projek/assets/138902919/350d7fb5-7e65-4f4d-af6e-f5a81c205114)

    [2] Here is where you tell Discovery which fields to ignore. Using the on/off buttons, turn off all labels except subtitles and text.
    [3] is telling Discovery to split the document apart, based on subtitle.
    Click [4] to submit your changes.
Return to the Activity tab. After the changes are processed (may take some time), your collection will look very different:
![gambar](https://github.com/alwayne23/capstone_projek/assets/138902919/830a095e-30bb-4222-b317-8a878972714b)
Return to the query panel (click Try it out) and see how much better the results are.
![gambar](https://github.com/alwayne23/capstone_projek/assets/138902919/d7126bbf-fbf3-4fe8-9888-db7441623184)
# 4. Configure Watson Assistant
The instructions for configuring Watson Assistant are basically the same for both IBM Cloud Pak for Data and IBM Cloud.

One difference is how you launch the Watson Assistant service. Click to expand one:

  1. IBM Cloud Pak for Data
  2. IBM Cloud
# belom dijelasin langkah langkahnya :D
# 5. Add Watson service credentials to environment file
Copy the local env.sample file and rename it .env:

cp env.sample .env

You will need to Update the .env file with the credentials from your Assistant service. First you will need the Assistant ID which can found by:

    Click on your Assistant panel
    Clicking on the three dots in the upper right-hand corner and select Settings.
    Select the API Details tab.

You will also need the credentials for your Assistant service. What credentials you will need will depend on if you provisioned Watson Assistant on IBM Cloud Pak for Data or on IBM Cloud. Click to expand one:
  1. IBM Cloud Pak for Data
  2. IBM Cloud
# 6. Run the application

    Install Node.js runtime or NPM.
    Start the app by running npm install, followed by npm start.
    Use the chatbot at localhost:3000.

Sample questions:

    How do I override the scheduled temperature
    How do I turn on the heater
    how do I set a schedule?
# Sample Output
![gambar](https://github.com/alwayne23/capstone_projek/assets/138902919/e700f800-8dd5-4c92-975d-1cfd55de1c75)
# Access to results in application


# Learn more

    Artificial Intelligence Code Patterns: Enjoyed this Code Pattern? Check out our other AI Code Patterns
    AI and Data Code Pattern Playlist: Bookmark our playlist with all of our Code Pattern videos
    With Watson: Want to take your Watson app to the next level? Looking to utilize Watson Brand assets? Join the With Watson program to leverage exclusive brand, marketing, and tech resources to amplify and accelerate your Watson embedded commercial solution.
# License
This code pattern is licensed under the Apache License, Version 2. Separate third-party code objects invoked within this code pattern are licensed by their respective providers pursuant to their own separate licenses. Contributions are subject to the Developer Certificate of Origin, Version 1.1 and the Apache License, Version 2.

Apache License FAQ
