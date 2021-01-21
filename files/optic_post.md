# Using Optic to generate your API Specs
You might have some APIs that, for some reason, don't have their OAS/Swagger file. Well, this post will show how you could save yourself in scenarios like that.

  

You can download Optic from here: https://useoptic.com. What I will show here is pretty much a recipe to fully understand how to use that tool.

  

## Mocking APIs using json-server

  

For mocking purposes, I will use json-server, a great tool for testing and enabling quick APIs; however, it isn't documented using Swagger, so perfect scenario for us, and also the project creators claim that: "Get a full fake REST API with **zero coding** in **less than 30 seconds** (seriously)"

  

Please, refer to json-server Getting started here: https://github.com/typicode/json-server#getting-started

  

## Installing Optic

  

Optic has effortless ways to install, from npm to homebrew, select your preferred one, and install it in your machine.

  

## Creating the Project

  

Please, create an empty folder using your terminal, and enter the following code:

  

    $ api init

  
    /Users/edgar/OneDrive/skalena/projetos/useoptic/json-server
    
    Is this your API's root directory? (yes/no): yes
    
    What is this API named?: json-server
    
    [optic] Added Optic configuration to /Users/edgar/OneDrive/skalena/projetos/useoptic/json-server/optic.yml
    
    Finish setting up your API start task here: http://localhost:34444/apis/1/setup

  

When this process gets finished, the terminal will open up a browser window with some information as the following image:

  http://localhost:34444/apis/1/setup 

![enter image description here](https://github.com/edgars/generiquisses/raw/master/img/1.png)

  

Optic had generated a file called optic.yml and a folder .optic into your current folder. The optic.yml is used to inform which is the process that will be attached/proxied by optic. In our case, we use the following:

    name: "json-server"
    tasks:
      start:
        targetUrl: http://localhost:3000
        inboundUrl: http://localhost:4000

  

In that case, the URL http://localhost:4000 will act as a proxy for the real backend, exposed into http://localhost:3000.

Note: Don't forget to start the json-server with the command: `json-server --watch db.json`
  

Execute the command: `api start` , in order to execute the optic and get ready to receive the HTTP requests, the result will be something like:

     $ json-server api start
    [optic] Review the API Diff at http://localhost:34444/apis/1/review
    [optic] Optic is observing requests made to http://localhost:4000

### Time to execute the HTTP Requests 

First, we will get all post data using the following command: 

    curl --location --request GET 'http://localhost:4000/posts'

The result: 

    [   
        { "id": 1,
         "title": "json-server",
          "author": "typicode"
        }  
     ]
Now, time to insert a post: 

    curl --location --request POST 'http://localhost:4000/posts' \
    
    --header 'Content-Type: application/json' \
    
    --data-raw ' {
    
    "id": 2,
    
    "title": "optics made great stuff",
    
    "author": "edgar"
    
    }'

If you execute the first GET posts, you will see that a new record was inserted. 

Now, it is time to execute a PATCH http method, in order to update a record:

    curl --location --request PATCH 'http://localhost:4000/posts/2' \
    
    --header 'Content-Type: application/json' \
    
    --data-raw ' {
    
    "title": "optics made great stuff - after patch",
    
    "author": "edgar silva"
    
    }'

Just to complete this, let's delete a record, using the HTTP method DELETE. 

    curl --location --request DELETE 'http://localhost:4000/posts/2'

There we are, we had executed the main HTTP verbs in order to operate our mocked API, so let's see what Optic has done for us. The reviews might be exposed using this URL: http://localhost:34444/apis/1/review 

![enter image description here](https://github.com/edgars/generiquisses/raw/master/img/optic-review-http-verbs.png)

You are able to edit the URI params, in order to set them accordingly to what the API is expecting, please see the following image: 

![enter image description here](https://github.com/edgars/generiquisses/raw/master/img/creating_params.png)
When you get the parms done, you can check the whole requests that you want to export to the documentation. 
![enter image description here](https://github.com/edgars/generiquisses/raw/master/img/preparing_to_edit.png)
In the next step, you can edit and add as much information as you can, in order to prepare a great API Spec and Documentation. 
![enter image description here](https://github.com/edgars/generiquisses/raw/master/img/documentation_optic.png)

## Generating the OAS file

You must execute the command: `api generate:oas` , in order to get your yaml file, with your whole OAS specification. You file will be generated inside your .optic folder (`.optic/generated/openapi.yaml`). 

## Checking the File

If you are a Visual Studio Code, Intelij or Eclipse user, you can download the [42Crunch extension](https://42crunch.com/resources-free-tools/), in order to check 2 very important aspects into your OAS file. 

 1. If the overall OAS file structure is ok
 2. Which are your main Security Issues present in that `contract`, it is processed using the Audit Report capability from the 42Crunch platform, which can be used for free for any developer. 
 
Overall OAS file generated and overview inside Visual Studio Code:
![Overall OAS file generated and overview inside Visual Studio Code](https://github.com/edgars/generiquisses/raw/master/img/optic_vscode1.png)

API Security is a very important aspect, using the 42Crunch platform, you can get the chance to get your APIs protected during the entire API Lifecycle, since the envision/design/idea, passing through CI/CD and production. To know more about the platform go to: https://42crunch.com/why-42crunch/.

Even our API from that post looks good, no erros, in terms of security, the Audit score is 2 of 100, so we can say that this API has several security issues.

![enter image description here](https://github.com/edgars/generiquisses/raw/master/img/optic_vscode2.png)

# Conclusion

Optic can be a great alternative for your APIs that for any reason have no OAS(Open API Specification) files. Hope it can help you and happy codding! 
