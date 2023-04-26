# Deploying React app to Azure App Services

# Overview
We are building an app for the FARM-Tanzania website and need
to host this app somewhere to be publically accessable with low
to none running costs, pretty high availability and easy deployment.

In come: cloud services.

Now, in this document I will explain how to deploy a simple
React app to Azure cloud services. Why Azure? No reason at all.
We could just as easily deploy this app to Google Cloud Platform
or AWS, but here we use Azure.

# Azure App Services
Azure App Services (also names Azure Web Apps) is a cloud computing
based platform for hosting websites, created and operated by Microsoft.
It is a platform as a service which allows publishing Web apps. It has
a free tier which is more than sufficient for our need, thus if we are
not careless, hosting our app will be free.

# My app
For this tutorial, I created a very simple app using the [create-react-app](https://create-react-app.dev/)
function and adjusted it a little. I have very limited knowledge with
React, so I'm sure for someone with a better understanding about
React and UI, making this look nice should not be an issue.

# Hosting
I hosted my app at the following link using Azure web apps:
https://farm-africa.azurewebsites.net/

# Tutorial
Here follows a step-by-step guide on how to do this:

## Step 1: Setup Azure project
For this step you will have to setup a billing account using
a valid credit card and create a project linked to the billing
account. You can do this in the Azure portal: 
https://portal.azure.com

## Step 2: Create web app
Go to App Services and click on Create:

![App Services](assets\Step2-01.png)

Give your app a name (this will also be the URL of the app),
select any resource group, select Node 18 LTS as the runtime stack
and select any region. Under pricing plan, select the free tier, then click
on review + create and again on create.

## Step 3: Download publish profile
After the app is created and deployed (it does take a few seconds, so be patient),
click on 'Go to resource' and then click on 'Download publish profile' in the top bar.
This will download a text file which you can open with a text editor. You can also now
test that your app is deployed by clicking on the link under 'Default domain'. You should
see a website as follows:

![Base website](assets\Step3-01.png)

## Step 4: Upload code to GitHub
Creat a GitHub repo and upload the source code to that repo. You can use this code, which is
currently used for the site mentioned above, or create your own source code. If you are using
my code, please skip step 6 as this is already included in my code.

## Step 5: Add Azure publish profile to GH secrets
On your repo, go to 'Settings', 'Secrets and variables', 'Actions' and click on
'New repository secret'. Here, make your secret name 'AZURE_WEBAPP_PUBLISH_PROFILE'
and paste the full content of the downloaded publish profile to the secret value and click
on save.

## Step 6: Create workflow
If you are using my code, this step is not needed as it is already included in my code.
In your repo, click on 'Actions' and then 'New workflow'. Here, click on 'Configure' under
'Deploy Node.js to Azure Web App'. In the file editor that opens, make the following changes:
1. Set AZURE_WEBAPP_NAME to the name of the app you deployed in Step 2
2. Find the step named 'Upload artifact for deployment job' and change path from . to build
3. Click on commit changes and again on commit changes

## Step 7: Add startup command to web app
Now, go back to your web app in the Azure portal and go to 'Configuration' and 'General settings'.
Here, paste `pm2 serve /home/site/wwwroot --no-daemon --spa` under 'Startup Command' and click on Save.

## Step 8: Done!
Your webapp should now be updated. Go back to your public website and refresh (note it may take a few mins
if your app is large). Before commiting new changes to your GitHub repo, remember to first pull (since you
have created a new file in step 6). Ofcourse, this is not needed if you skipped step 6.