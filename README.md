# Hosting Next.JS application using Catalyst AppSail

To host your Next.js application in Catalyst we would suggest you check our Catalyst AppSail feature. Catalyst AppSail is a **Platform-as-a-Service(PaaS)** feature of Catalyst that helps you host your Standalone applications in Catalyst.

You can find the help documentation for Catalyst AppSail [here](https://docs.catalyst.zoho.com/en/serverless/help/appsail/introduction/).

You can find detailed instructions on how to host a Sample Next.Js application in Catalyst using Catalyst AppSail below:

![]()

---

## Create a Catalyst project in Catalyst Console:

- You can find help documentation for creating a Catalyst Project [here](https://docs.catalyst.zoho.com/en/getting-started/catalyst-projects/#create-a-catalyst-project).
- Kindly follow instructions mentioned in the above help document to create a project in your catalyst console.

---

![]()

## Install Catalyst CLI in your local machine:

- To access your Catalyst project in your local machine you need to install our Catalyst CLI by using the below command in your terminal.

- The above command will install Catalyst CLI in your local machine which you can confirm by using the command **catalyst** which will list all the available commands in your terminal.

---

![]()

## Initialize your Catalyst AppSail in your Local machine using Catalyst CLI:

- Create a folder for your Catalyst project in your local machine and navigate to that folder in your terminal.
- Create a folder called ***build*** inside your project folder
- Now try using the below command to initialize your Catalyst project in your local machine from the catalyst project root folder.

> ``catalyst init``

- After choosing your project the CLI prompts you to choose the features you wish to initialise. Use the arrows keys to navigate to **AppSail** and mark it using **Spacebar** and Press Enter.
- You can then enter **No** for the prompt *"Do you want to get-started with a list of recommended projects?"*
- Next, You can enter **Yes** for the prompt *"Do you want to initialize an AppSail service in this directory?"*
- For the build path prompt kindly choose the folder-path that contains the contents which are required to be deployed in the *Catalyst* console:

  - Example: */Users/catalyst-solutions/Documents/appsail/build*
    ![]()
- You can then create a name for your Appsail feature. For now we would suggest you use the name ***Next***
- For the stack you can choose Node 16.
- Now inside your project root folder you will be able to see the below files

  - .catalystrc
  - catalyst.json
  - app-config.json

Now you have successfully initialized a AppSail feature for your project in your local machine.

---

![]()

## Initialize a Sample Next.JS application inside the Catalyst Project folder:

- Navigate to the catalyst project folder inside your terminal and use the command.

> ``npx create-next-app client``

- Provide the necessary configurations you need for your Next JS application and finish initialization.

Now you have successfully initialized your Next JS application inside Catalyst project folder

---

![]()

## Adding export configurations for your Next JS application:

- Navigate to the **next.config.js** in the *client* folder and replace the given code with the below code snippet.

```
module.exports = {
    distDir : "dist",  
};
```

- The above code snippet will help you export your final production build into a folder called **"dist"** which contains the necessary files to host your Next JS application.

---

![]()

## Configuring your package.json with scripts neccesary to deploy your Next JS app for development/production environment

- Open your ***package.json*** in your *client* folder and update the **"scripts"** key with the following code snippet

```
"scripts": {
    "dev": "next dev",
    "build": "npm install && next build",
    "start": "next start -p ${X_ZOHO_CATALYST_LISTEN_PORT}",
    "lint": "next lint",
    "clean":"cd .. && rm -rf build/*",
    "pack":"cp -r dist public package.json next.config.js ../build && cd ../build && npm install" 
  }
```

- The above code snippet helps you create a production build for your Next JS application and pack that build into the build folder which will be used to deploy your Next JS application to remote console.
- By only moving the necessary build files to remote console the **src** files that's crucial for developing your Next JS application is kept safe from unauthorized access and helps you keep the application size smaller compared to moving the whole application including the client folder to remote console.

---

![]()

## Configuring your app-config.json in your project folder for your AppSail to host your Next JS application:

- After making the necessary changes to the "scripts" key in your ***package.json*** in your client folder you need to call those scripts whenever you test your Next JS application in local/remote environment.
- In order to do that we have to update your ***app-config.json*** in your catalyst project root folder with the following code snippet

```
{
	"command": "npm run start",
	"buildPath": "{catalyst_project_folder_path}/build",
	"stack": "node16",
	"env_variables": {},
	"memory": 256,
	"scripts": {
		"preserve":"cd client && npm run build && npm run clean && npm run pack",
		"predeploy":"cd client && npm run build && npm run clean && npm run pack"
	}
}
```

- In the above code snippet you need to replace the ***{catalyst_project_folder_path}*** with your actual catalyst project root folder path so that we testing your application the Appsail looks for the build files in the path mentioned in the **"buildPath"** key.

---

![]()

## Test your application by deploying to Development environment:

- After completing the above mentioned steps you can now test your Next JS application in your local environment using the command

> ``catalyst serve``

You can find the help documentation for **catalyst serve** command [here](https://docs.catalyst.zoho.com/en/cli/v1/serve-resources/serve-all-resources/)

- After completing the above mentioned steps you can now deploy your Next JS application to the development environment.
- Navigate to your Catalyst project root folder in terminal and use the command

> ``catalyst deploy``

- The above command will run the script file mentioned in the **app-config.json** and create a production build of your Next JS application and deploy the same to the development environment.
- After successful deployment you will be able to see an URL in the terminal which you can use to test your application in development environment.

You can find the help documentation for **catalyst deploy** command [here](https://docs.catalyst.zoho.com/en/cli/v1/deploy-resources/introduction/).
