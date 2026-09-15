

# Task 1: Deploy and Host a React App
<a name="module-one"></a>


|  |  | 
| --- |--- |
| **Time to complete** | 10 minutes  | 
| **Services used** | [AWS Amplify](https://aws.amazon.com/amplify/)  | 
| **Requires** |  +  A [GitHub account](https://github.com/)  <br />+  GitHub [SSH connection](https://docs.github.com/en/authentication/connecting-to-github-with-ssh)  <br />+  [Nodejs](https://nodejs.org/en/download) and [npm](https://www.npmjs.com/)    | 
| **Get help** |  +  [Troubleshooting Amplify](https://docs.amplify.aws/react/build-a-backend/troubleshooting/)  <br />+  [Learn about Hosting](https://docs.amplify.aws/react/deploy-and-host/hosting/)    | 

## Overview
<a name="overview"></a>

AWS Amplify offers a Git-based CI/CD workflow for building, deploying, and hosting single-page web applications or static sites with serverless backends. When connected to a Git repository, Amplify determines the build settings for both the frontend framework and any configured serverless backend resources, and automatically deploys updates with every code commit. 

In this task, you will start by creating a new React application and pushing it to a GitHub repository. Then, connect the repository to AWS Amplify web hosting and deploy it to a globally available content delivery network (CDN) hosted on an **amplifyapp.com** domain. Finally, you will demonstrate continuous deployment capabilities by making changes to the React application, pushing a new version to the main branch, and observing how it automatically invokes a new deployment. 

## What you will accomplish
<a name="what-you-will-accomplish"></a>
+ Create a React application 
+ Initialize a GitHub repository 
+ Deploy your app with AWS Amplify 
+ Implement code changes and redeploy your app 

## Implementation
<a name="implementation"></a>

### Step 1: Create a new React Application
<a name="create-a-new-react-application"></a>

1. Set up the React environment

   In a new terminal window, run the following command to use Vite to create a React application: 

   ```
   npm create vite@latest notesapp -- --template react
   cd notesapp
   npm install
   npm run dev
   ```  
![The resource creation interface.](https://docs.aws.amazon.com/hands-on/latest/build-react-app-amplify-graphql/images/full-stack-react-deb-resource-creation.png)

1. View your application

   In the terminal window, choose the **Local** **link**.   
![The resource creation interface.](https://docs.aws.amazon.com/hands-on/latest/build-react-app-amplify-graphql/images/local-link.png)

### Step 2: Create the GitHub repository and commit code
<a name="create-the-github-repository-and-commit-code"></a>

Before you begin: 
+ You need a GitHub account. If you don't have one, [sign up here](https://github.com/). 
+ If you've never used GitHub on your computer, generate and add an SSH key to your account. For instructions, see [Connecting to GitHub with SSH](https://docs.github.com/en/authentication/connecting-to-github-with-ssh). 

1. Initialize GitHub repository

   **Sign in** to GitHub at [https://github.com/](https://github.com/).   
![The navigation bar showing you need a github account. if you don't have one, sign up here., and if you've never...](https://docs.aws.amazon.com/hands-on/latest/build-react-app-amplify-graphql/images/basic-navigation-bar-you-need-github.png)

1. Create a repository

   In the **Start a new repository** section, make the following selections: 

   For **Repository name**, enter **notesapp**, and choose the **Public** radio button. 

   Then select, **Create a new repository**.   
![The create a new repository window with a name and public selected.](https://docs.aws.amazon.com/hands-on/latest/build-react-app-amplify-graphql/images/xojnw-efac-resource-creation-interface.png)

1. Push the new repo

   **Open** a new terminal window, **navigate** to your app's root folder (**notesapp**), and run the following commands to initialize a git and push the application to the new GitHub repo: 
**Note**  
Replace the **SSH GitHub URL** in the command with your SSH GitHub URL.

   ```
   git init
   git add .
   git commit -m "first commit"
   git remote add origin git@github.com:<your-username>/notesapp.git 
   git branch -M main
   git push -u origin main
   ```  
![Terminal showing the commands running.](https://docs.aws.amazon.com/hands-on/latest/build-react-app-amplify-graphql/images/push-to-new-repo.png)

### Step 3: Install the Amplify Packages
<a name="install-the-amplify-packages"></a>

1. Configure your local repository

   **Open** a new terminal window, **navigate** to your app's root folder (**notesapp**), and run the following command: 

   ```
   npm create amplify@latest -y
   ```  
![Terminal showing npm create command.](https://docs.aws.amazon.com/hands-on/latest/build-react-app-amplify-graphql/images/npm-create.png)

1. Review the Amplify project structure

   Running the previous command will scaffold a lightweight Amplify project in the app’s directory.   
![The review and confirmation interface.](https://docs.aws.amazon.com/hands-on/latest/build-react-app-amplify-graphql/images/confirmation-interface.png)

1. Push your changes to GitHub

   In your open terminal window, run the following commands to push the changes to GitHub: 

   ```
   git add .
   git commit -m 'installing amplify'
   git push origin main
   ```  
![Interface element requiring manual review.](https://docs.aws.amazon.com/hands-on/latest/build-react-app-amplify-graphql/images/push-changes.png)

### Step 4: Deploy your app with AWS Amplify
<a name="deploy-your-app-with-aws-amplify"></a>

In this step, you will connect the GitHub repository you just created to AWS Amplify. This will enable you to build, deploy, and host your app on AWS. 

1. Create the Amplify App

   **Sign in** to the AWS Management console in a new browser window, and **open** the AWS Amplify console at [https://console.aws.amazon.com/amplify/apps](https://console.aws.amazon.com/amplify/apps). 

   Choose **Create new app**.   
![The resource creation interface.](https://docs.aws.amazon.com/hands-on/latest/build-react-app-amplify-graphql/images/resource-creation-interface-1.png)

1. Connect to your GitHub repository

   On the **Start building with Amplify** page, for **Deploy your app**, select **GitHub**, and select **Next**.   
![Interface element requiring manual review.](https://docs.aws.amazon.com/hands-on/latest/build-react-app-amplify-graphql/images/mzti-baf-interface.png)

1. Authorize and select your respository

   When prompted, **authenticate** with GitHub. You will be automatically redirected back to the Amplify console. Choose the **repository** and **main branch** you created earlier. Then, select **Next**.   
![The selection interface.](https://docs.aws.amazon.com/hands-on/latest/build-react-app-amplify-graphql/images/frqwznor-selection-interface.png)

1. Configure build settings

   Leave the default **build settings** and select **Next**.   
![The configuration settings interface.](https://docs.aws.amazon.com/hands-on/latest/build-react-app-amplify-graphql/images/configuration-settings-interface-1.png)

1. Deploy your application

   Review the inputs selected, and choose **Save and deploy**.   
![The configuration settings interface.](https://docs.aws.amazon.com/hands-on/latest/build-react-app-amplify-graphql/images/save-and-deploy.png)

1. Verify your deployment

   AWS Amplify will now build your source code and deploy your app at https://...amplifyapp.com, and on every git push your deployment instance will update. It may take up to 5 minutes to deploy your app. 

   Once the build completes, select the **Visit deployed URL** button to see your web app up and running live.   
![The interface controls and buttons.](https://docs.aws.amazon.com/hands-on/latest/build-react-app-amplify-graphql/images/interface-controls-buttons-1.png)

### Step 5: Automatically deploy code changes
<a name="automatically-deploy-code-changes"></a>

In this step, you will make some changes to the code using your text editor and push the changes to the main branch of your app. 

1. Update your application code

   On your local machine, navigate to the notesapp/src/App.jsx file, and update it with the following code. Then, save the file. 

   ```
   import reactLogo from "./assets/react.svg";
   import "./App.css";
   function App() {
     return (
       <div className="App">
         <header className="App-header">
           <img src={reactLogo} className="logo react" alt="React logo" />
           <h1>Hello from Amplify</h1>
         </header>
       </div>
     );
   }
   export default App;
   ```  
![App.jsx file in Finder.](https://docs.aws.amazon.com/hands-on/latest/build-react-app-amplify-graphql/images/app-jsx.png)

1. Push your code changes

   In your terminal window, run the following command to push the changes to GitHub: 

   ```
   git add .
   git commit -m 'changes for amplify'
   git push origin main
   ```  
![Interface element requiring manual review.](https://docs.aws.amazon.com/hands-on/latest/build-react-app-amplify-graphql/images/interface-interface-element-1.png)

1. Update your deployed application

   AWS Amplify will now **build** your source code and **deploy** your app.   
![Interface element requiring manual review.](https://docs.aws.amazon.com/hands-on/latest/build-react-app-amplify-graphql/images/interface-2.png)

1. View your updated application

   Navigate back to the Amplify console, and select the **Visit deployed URL** button to view your updated app.   
![The interface controls and buttons.](https://docs.aws.amazon.com/hands-on/latest/build-react-app-amplify-graphql/images/interface-controls-buttons-1.png)

## Conclusion
<a name="conclusion"></a>

You have deployed a React application in the AWS Cloud by integrating with GitHub and using AWS Amplify. With AWS Amplify, you can continuously deploy your application in the Cloud and host it on a globally available CDN. 