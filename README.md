# Rasa_Covid19

This chatbot provides information for covid 19 data extracting using below url

    https://api.rootnet.in/covid19-in/stats/history



Training the dialogue model
The biggest change in how Rasa Core model works is that custom action 'actions' now needs to run on a separate server. That server has to be configured in a 'endpoints.yml' file. This is how to train and run the dialogue management model:


Start the custom action server by running:
	
	rasa run actions


Open a new terminal and train the Rasa Core model by running:
	
	rasa train

Talk to the chatbot once it's loaded after running:
	
	rasa shell


# Dockerization with Heroku
 
Create a folder in home/user/ directory and name it HerokuIntegration and copy all the files of Rasa_Covid19.

        nano Dockerfile

        FROM ubuntu:20.04 ## docker image we have to pull over here
        ENTRYPOINT []
        RUN apt-get update && apt-get install -y python3 python3-pip && python3 -m pip install --no-cache --upgrade pip && pip3 install --no-cache rasa==2.5.0 --use-feature=2020-resolver ## installing rasa for our docker container so we don't need a seprate environment for this
        ADD . /app/
        RUN chmod +x /app/start_services.sh
        CMD /app/start_services.sh ## This file is used to run all teh applications

        nano start_services.sh

        cd app/
        ## Start rasa server with nlu model
        rasa run --model models --enable-api --cors "*" --debug \ -p $PORT  ## this will create a complete environment inside docker container

Creating git repository.

        git init

        git add . 
    
        git commit -m "Initialize the project"

Heroku set up withdocker container.

        heroku login
        
        /snap/bin/heroku create darshpatel # creating heroku repository
        
        /snap/bin/heroku container:login
        
        /snap/bin/heroku container:push web
        
        /snap/bin/heroku container:release web 

Testing the bot through an Heroku API.

        nano main.py
        
        import requests
        url = 'http://<url to heroku app>/webhooks/rest/webhook' #change rasablog with your app name
        myobj = {
        "message": "hi",
        "sender": "Darsh Patel",
        }
        x = requests.post(url, json = myobj)
        print(x.text)
        
        python3 main.py # by running python script the bot will be live on the heroku app and connecting through an api which we just created.
