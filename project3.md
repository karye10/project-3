## Kays Documentation of Project 3

### STEP 1 – BACKEND CONFIGURATION

*Update ubuntu*

`sudo apt update`

*Upgrade Ubuntu*

`sudo apt upgrade`

*Let’s get the location of Node.js software from Ubuntu repositories.*

`curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -`

*Install Node.js on the server*

`sudo apt-get install -y nodejs`

Verify the node installation with the command below

`node -v`

Verify the node installation with the command below

`npm -v`

### Application code setup

*Create a new directory for your To-Do project:*

`mkdir Todo`

Run the command below to verify that the Todo directory is created with ls command

`ls`

Change to the Todo directory

 `cd Todo`

 Next, you will use the command npm init to initialise your project, so that a new file named package.json will be created. This file will normally contain information about your application and the dependencies that it needs to run. Follow the prompts after running the command. You can press Enter several times to accept default values, then accept to write out the package.json file by typing yes.


`npm init`

![NPM INIT](/project-3/images/Npm_init.PNG)

Run the command ls to confirm that you have package.json file created.

### INSTALL EXPRESSJS
---

`npm install express`

Now create a file index.js with the command below

`touch index.js`

Run ls to confirm that your index.js file is successfully created

Install the dotenv module

`npm install dotenv`

Open the index.js file with the command below

`vi index.js`

Type the code below into it and save. 

![index_js](/project-3/images/index_js.PNG)

Notice that we have specified to use port 5000 in the code. This will be required later when we go on the browser.

Now it is time to start our server to see if it works. Open your terminal in the same directory as your index.js file and type:

`node index.js`

*If everything goes well, you should see Server running on port 5000 in your terminal.*

![port_5000](/project-3/images/port_5000.PNG)

Now we need to open this port in EC2 Security Groups. Refer to Project 1 Step 1 – Installing the Nginx Web Server. There we created an inbound rule to open TCP port 80, you need to do the same for port 5000, like this:

![security rules](/project-3/images/security_rules.PNG)

Open up your browser and try to access your server’s Public IP or Public DNS name followed by port 5000:

[node server](http://<PublicIP-or-PublicDNS>:5000)

![welcomeToExpress](/project-3/images/WelcomeToExpress.PNG)

### Routes

There are three actions that our To-Do application needs to be able to do:

1. Create a new task
2. Display list of all tasks
3. Delete a completed task

Each task will be associated with some particular endpoint and will use different standard HTTP request methods: POST, GET, DELETE.

For each task, we need to create routes that will define various endpoints that the To-do app will depend on. So let us create a folder routes

`mkdir routes`

`cd routes`

Now, create a file api.js with the command below

`touch api.js`

Open the file with the command below

`vi api.js`

Copy below code in the file.

![Api JS](/project-3/images/api_js.PNG)

*Moving forward let create Models directory.*

### MODELS

*Now comes the interesting part, since the app is going to make use of Mongodb which is a NoSQL database, we need to create a model.*

To create a Schema and a model, install mongoose which is a Node.js package that makes working with mongodb easier.

Change directory back Todo folder with cd .. and install Mongoose

`npm install mongoose`

Create a new folder models:

`mkdir models`

Change directory into the newly created ‘models’ folder with

`cd models`

Inside the models folder, create a file and name it todo.js

`touch todo.js`

*Tip: All three commands above can be defined in one line to be executed consequently with help of && operator, like this:*

`mkdir models && cd models && touch todo.js`

Open the file created with `vim todo.js` then paste the code below in the file:

![toDo js](/project-3/images/toDo_js.PNG)

Now we need to update our routes from the file api.js in ‘routes’ directory to make use of the new model.

In Routes directory, open api.js with `vim api.js`, delete the code inside with `:%d` command and paste there code below into it then save and exit

![New Api js](/project-3/images/new-api_js.PNG)

*The next piece of our application will be the MongoDB Database*

### MONGODB DATABASE

We need a database where we will store our data. For this we will make use of mLab. mLab provides MongoDB database as a service solution (DBaaS), so to make life easy, you will need to sign up for a shared clusters free account, which is ideal for our use case. Sign up here. Follow the sign up process, select AWS as the cloud provider, and choose a region near you.

Complete a get started checklist as shown on the image below

![mongoDb first image](/project-3/images/mongodb_get_started.PNG)

*Allow access to the MongoDB database from anywhere (Not secure, but it is ideal for testing)*
*IMPORTANT NOTE
In the image below, make sure you change the time of deleting the entry from 6 Hours to 1 Week*

![ip_access_list](/project-3/images/mongodb_access_list.PNG)

Create a MongoDB database and collection inside mLab

![collection](/project-3/images/mongodb_collection.PNG)

In the index.js file, we specified process.env to access environment variables, but we have not yet created this file. So we need to do that now.
Create a file in your Todo directory and name it .env.

`touch .env`
`vi .env`

Add the connection string to access the database in it, just as below:

Ensure to update 'username', 'password', 'network-address' and 'database' according to your setup
Here is how to get your connection string

![Connect](/project-3/images/mongodb_connect.PNG)

Now we need to update the index.js to reflect the use of .env so that Node.js can connect to the database.

Simply delete existing content in the file, and update it with the entire code below.
To do that using vim, follow below steps

1. Open the file with vim index.js
2. Press esc
3. Type :
4. Type %d
5. Hit ‘Enter’

The entire content will be deleted, then,

0. Press i to enter the insert mode in vim

0. Now, paste the entire code below in the file.

![alt text](/project-3/images/new_index_js.PNG)

Using environment variables to store information is considered more secure and best practice to separate configuration and secret data from the application, instead of writing connection strings directly inside the index.js application file.
Start your server using the command:

`node index.js`

*You shall see a message ‘Database connected successfully’, if so – we have our backend configured.*

 Now we are going to test it.
Testing Backend Code without Frontend using RESTful API

So far we have written backend part of our To-Do application, and configured a database, but we do not have a frontend UI yet. We need ReactJS code to achieve that. But during development, we will need a way to test our code using RESTfulL API. Therefore, we will need to make use of some API development client to test our code.
In this project, we will use Postman to test our API

You should test all the API endpoints and make sure they are working. For the endpoints that require body, you should send JSON back with the necessary fields since it’s what we setup in our code.
Now open your Postman, create a POST request to the API [post request](http://<PublicIP-or-PublicDNS>:5000/api/todos). This request sends a new task to our To-Do list so the application could store it in the database.

*Note: make sure your set header key Content-Type as application/json*

![post reques](/project-3/images/postman_post.PNG)

Create a GET request to your API on [get request](http://<PublicIP-or-PublicDNS>:5000/api/todos.) This request retrieves all existing records from out To-do application (backend requests these records from the database and sends it us back as a response to GET request).

![get request](/project-3/images/postman_get.PNG)

### STEP 2 – FRONTEND CREATION
---

Since we are done with the functionality we want from our backend and API, it is time to create a user interface for a Web client (browser) to interact with the application via API. To start out with the frontend of the To-do app, we will use the create-react-app command to scaffold our app.
In the same root directory as your backend code, which is the Todo directory, run:
`npx create-react-app`

This will create a new folder in your Todo directory called client, where you will add all the react code.
Running a React App
Before testing the react app, there are some dependencies that need to be installed.

Install concurrently. It is used to run more than one command simultaneously from the same terminal window.

`npm install concurrently --save-dev`

Install nodemon. It is used to run and monitor the server. If there is any change in the server code, nodemon will restart it automatically and load the new changes.

`npm install nodemon --save-dev`

In Todo folder open the package.json file. Change the highlighted part of the below screenshot and replace with the code below.

Configure Proxy in package.json

Change directory to ‘client’

`cd client`

Open the package.json file

`vi package.json`

![package json](/project-3/images/packageJson_ScriptEdit.PNG)

Add the key value pair in the package.json file "proxy": "http://localhost:5000".

The whole purpose of adding the proxy configuration in number 3 above is to make it possible to access the application directly from the browser by simply calling the server url like http://localhost:5000 rather than always including the entire path like http://localhost:5000/api/todos

Now, ensure you are inside the Todo directory, and simply do:

`npm run dev`

*Your app should open and start running on localhost:3000
Important note: In order to be able to access the application from the Internet you have to open TCP port 3000 on EC2 by adding a new Security Group rule. You already know how to do it.*

Creating your React Components

From your Todo directory run

`cd client`

move to the src directory

`cd src`

Inside your src folder create another folder called components

`mkdir components`

Move into the components directory with

`cd components`

Inside ‘components’ directory create three files Input.js, ListTodo.js and Todo.js.

`touch Input.js ListTodo.js Todo.js`

Open Input.js file

`vi Input.js`

copy and paste the following

![alt text](/project-3/images/Input_js_code.PNG)

To make use of Axios, which is a Promise based HTTP client for the browser and node.js, you need to cd into your client from your terminal and run yarn add axios or npm install axios.

Move to the src folder

`cd ..`

Move to clients folder

`cd ..`

Install Axios

`npm install axios`

### FRONTEND CREATION (CONTINUED)

Go to ‘components’ directory

`cd src/components`

After that open your ListTodo.js

`vi ListTodo.js`

in the ListTodo.js copy and paste the following code

![list Todo](/project-3/images/List_Todo_js.PNG)

Then in your Todo.js file you write the following code

![Todo js](/project-3/images/toDo_js.PNG)

We need to make little adjustment to our react code. Delete the logo and adjust our App.js to look like this.

Move to the src folder

`cd ..`

Make sure that you are in the src folder and run

`vi App.js`

After pasting, exit the editor.
In the src directory open the App.css

`vi App.css`

Then paste the following code into App.css:

![App CSS](/project-3/images/App_css.PNG)

Exit

In the src directory open the index.css

`vim index.css`

Copy and paste the code below:

![index css](/project-3/images/index.css)

Go to the Todo directory

`cd ../..`

When you are in the Todo directory run:

`npm run dev`

Assuming no errors when saving all these files, our To-Do app should be ready and fully functional with the functionality discussed earlier: creating a task, deleting a task and viewing all your tasks.

### Congratulations

In this Project #3 you have made a simple To-Do and deployed it to MERN stack. You wrote a frontend application using React.js that communicates with a backend application written using Expressjs. You also created a MongoDB backend for storing tasks in a database.

