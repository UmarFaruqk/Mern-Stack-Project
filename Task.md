### MERN WEB STACK
MERN stack is a collection of technologies that help developers build robust and scalable web applications using JavaScript. The acronym “MERN” stands for MongoDB, Express, React, and Node.
1. MongoDB- A document-based, No-SQL database used to store application data in a form of documents.
2. ExpressJS- A server side Web Application framework for Node.js.
3. ReactJS- A frontend framework developed by Facebook. It is based on JavaScript, used to build User Interface (UI)
4. Node.js- A JavaScript runtime environment. It is used to run JavaScript on a machine rather than in a browser

=> In order to complete this project you will need an AWS account and a virtual server with Ubuntu Server OS and we also going to use Windows Terminal 
=> Create and instance on your AWS console and connect to your terminal then cd into the directory where you downloaded your .pem key and use the following command *$ ssh -i *your.pem key* *ubuntu@ your IP address* ![reference image](/Images/pic1.PNG)
=> The Task is to deploy a simple *To-Do* application that creates To-Do list.![reference image](/Images/pic40.PNG)

# 1.BACKEND CONFUGURATION
i. We update ubuntu ![reference image](/Images/pic2.PNG)
ii. Then upgrade ubuntu ![reference image](/Images/pic3.PNG)
iii. Next we get the location of Node.js from <Ubuntu repositories> using the following command *curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -* and you should see this ![reference image](/Images/pic4.PNG)
iv. Install Node.js *sudo apt-get install -y nodejs* you should this ![reference image](/Images/pic5.PNG) the command above installs both *node.js* and *npm* so verify the installation by using ![reference image](/Images/pic6.PNG)
# 2. APPLICATION CODE Setup
i. Create a directory called Todo and verify the directory is created then change directory to the Todo directory ![reference image](/Images/pic%207.PNG)
ii. Now initialize your project so a new file named *package.json* will be created ![reference image](/Images/pic8.PNG) then *ls* to confirm that you have package.json file is created ![reference image](/Images/pic9.PNG)

1. Install ExpressJS => Remember that Express is a framework for Node.js, therefore a lot of things developers would have programmed is already taken care of out of the box. Therefore it simplifies development, and abstracts a lot of low level details.
i. To use express install npm then create a file named *index.js* and run *ls* to confirm, you should see this  ![reference image](/Images/pic10.PNG)
ii. Install the *dotenv* module using *npm install dotenv*
iii. open the index.js file using *vim index.js* then type the following code and save ![reference image](/Images/pic11.PNG) notice we specified port *5000* in the code don't worry we'll talk about is later save and exit using *:wq*
iv. It's time to start our server to see if it works make sure you're in the *ToDo* directory then run *node index.js* you should see this ![reference image](/Images/pic12.PNG)
v. Now we need to open this port in our EC2 security group edit your inbound rule for port ![reference image](/Images/pic41.PNG) 
then open your  browser and try to access your server's Public IP or Public DNS name followed bt port 5000 *http://<PublicIP-or-PublicDNS>:5000* you should see this ![reference image](/Images/pic13.PNG) 
2. Routes => There are three actions our To-Do application needs to be able to do:
i. Create a new task
ii. Display list of all tasks
iii. Delete a completed task
Each task will be associated with some particular endpoint and will use different standard HTTP request methods: POST, GET, DELETE.
For each task, we need to create *routes* that will define various endpoints that the *To-do* app will depend on.
=> Now we create a folder called routes the *cd* into the directory then create create a file called *api.js* ![reference image](/Images/pic14.PNG) then paste the following codes ![reference image](/Images/pic42.PNG)
=> Now comes the interesting part, since the app is going to make use of Mongodb which is a NoSQL database, we need to create a model.
A model is at the heart of JavaScript based applications, and it is what makes it interactive.
We will also use models to define the database schema. This is important so that we will be able to define the fields stored in each Mongodb document.
To create a Schema and a model, install mongoose which is a Node.js package that makes working with mongodb easier.

=> change the directory to folder using *cd ..* then run *npm install mongoose* ![reference image](/Images/pic15.PNG)
=> Now you create a directory named *models* then create a file inside it named *todo.js* ![reference image](/Images/pic16.PNG) and paste the following code using *vim todo.js* save abd exit ![reference image](/Images/pic17.PNG)
=> Now go to the Routes directory open *api.js*  using *vim api.js* delete the code in it using *:%d* and paste in ![reference image](/Images/pic18.PNG) then save and exit

# MongoDB Database
We need a database where we will store our data. For this we will make use of mLab. mLab provides MongoDB database as a service solution (DBaaS), so to make life easy, you will need to sign up for a shared clusters free account, which is ideal for our use case.<https://www.mongodb.com/products/try-free/platform/atlas-signup-from-mlab> Follow the sign up process, select AWS as the cloud provider, and choose a region near you.
=> Complete the get started checklist
=> Allow access to MongoDB database from anywhere *(Not secure, but it is ideal for testing)*
=> Create a MongoDB database and collection inside mLab
=> In the *index.js* file, we specified *process.env* to access environment variables, but we have not yet created this file.
=> create a file in your *Todo* directory and name it *.env* using *touch .env* then *vim .env* 
=> Add the connection string to the database in it but before then simply delete the existing content in the file by using *:%d* like this*DB = 'mongodb+srv://<username>:<password>@<network-address>/<dbname>?retryWrites=true&w=majority'* ![reference image](/Images/pic19.PNG)

=> Now we need to update the *index.js* to reflect the use of *.env* so that the Node.js can connect to the database but before then enter the *index.js* using *vim* delete the previous content using *:%d* and enter the following code ![reference image](/Images/pic20.PNG) 
=> now run the *node index.js* and you  should see the ![reference image](/Images/pic22.PNG) 
=> Now we have our backend configured

# Testing Backend Code Without Frontend Using RESTful API

So far we have written backend part of our To-Do application, and configured a database, but we do not have a frontend UI yet. We need ReactJS code to achieve that. But during development, we will need a way to test our code using RESTfulL API. Therefore, we will need to make use of some API development client to test our code.
=> In this project, we will use Postman to test our API. <https://www.postman.com/downloads/> 
=> You should test all the API endpoints and make sure they are working. For the endpoints that require body, you should send JSON back with the necessary fields since it’s what we setup in our code.
=> Also make sure you set your key *Content-Type* as *application/json* 
=> Then create a POST, GET, and DELETE request to your API on *http://<public IP or public DNS>:5000/api/todos* as shown in the pictures ![reference image](/Images/pic23.PNG), ![reference image](/Images/pic24.PNG), ![reference image](/Images/pic25.PNG)
=> By now you have tested backend part of our To-Do application and have made sure that it supports all three operations we wanted: HTTP GET, HTTP POST and HTTP DELETE request.

# 3. Frontend Creation
=> Now we create a user interface for a Web client (browser) to interact with the application via API. We will use the *npx create-react-app client* command to scaffold our app in the Todo directory          ![reference image](/Images/pic26.PNG), this will create a folder in your *Todo* directory called *client*

# 4. Running React App
=> Before testing the react app we need to install some dependencies like *concurrently* and *nodemon* with the following command ![reference image](/Images/pic27.PNG)
=> Now *vim package.json* in the *Todo* folder and edit it with the following code ![reference image](/Images/pic28.PNG) you can choose to add your name as the author as I did mine 
=> *cd client* directory *vim package.json* and add *"proxy": "http://localhost:5000"*
=>  The whole purpose of adding the proxy configuration  above is to make it possible to access the application directly from the browser by simply calling the server url
=> Now go back to the *Todo* directory and run *npm run dev*
but in order for you to be able to access your application you have to open port 3000 on EC2 by adding a new security group rule ![reference image](/Images/pic41.PNG) your app should start running on *localhost:3000* 

# 5. Creating Your React Components
One of the advantages of react is that it makes use of components, which are reusable and also makes code modular. For our Todo app, there will be two stateful components and one stateless component.
=> *cd client* directory then *cd src* now *mkdir components* then create the following file *touch Input.js ListTodo.js Todo.js*
=> *vim Input.js* and copy the following code ![reference image](/Images/pic32.PNG) save and exit using *:wq*
=> Now *cd ../..* back to *Todo* folder to install Axios which is a Promise based HTTP client for the browser and node.js then run *npm install axios* 
=> Go back to the components directory *cd src/components* and *vim ListTodo.js* and paste the following code ![reference image](/Images/pic34.PNG) exit and save the do the same for *Todo.js* file and paste the following ![reference image](/Images/pic35.PNG)
=> Now go back to the *cd src* and *vim App.js* then paste the following code ![reference image](/Images/pic36.PNG) save and exit
=> *vim App.css* and paste the following code ![reference image](/Images/pic37.PNG) 
=> Do the same for *index.css* and paste the following code ![reference image](/Images/pic38.PNG)
=> Go back to your *Todo* directory *cd ../..* and run *npm run dev* assuming no errors when saving all these files then our To-Do app should be ready and fully functional and you should see this ![reference image](/Images/pic39.PNG) and this ![reference image](/Images/pic40.PNG)

## Thank you for following me this far, whatever you don't understand feel free to contact me.