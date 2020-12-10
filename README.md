# Python-Seed
 
## Environment
 
This is the environment (language, IDE, tools) recommended for development
- [Python 3.8](https://www.python.org/downloads/)
- [Visual Studio Code](https://code.visualstudio.com/Download),
   Recommended extensions: [Python](https://marketplace.visualstudio.com/items?itemName=ms-python.python), [Docker](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-docker), [LiveShare](https://marketplace.visualstudio.com/items?itemName=MS-vsliveshare.vsliveshare)
- [Git](https://git-scm.com/downloads)
- [Docker](https://docs.docker.com/engine/install/)
    - [Install Docker Desktop on Windows Home](https://docs.docker.com/docker-for-windows/install-windows-home/)
 
## Setup
 
### Download code repository
1. Make sure you have [Git](https://git-scm.com/downloads) installed on your machine
2. In Github, click on the green button label "Code" in top right of the repository
3. Copy the HTTPS url
4. Open a terminal in the folder where you want the your code repository to be
5. Run: `git clone [HTTPS url]`
 
If you need more help click [Here](https://www.howtogeek.com/451360/how-to-clone-a-github-repository/)
 
### Install required python packages
1. Make sure to have [Python 3.8](https://www.python.org/downloads/) installed on your machine
2. Clone code repository (see section above to know how)
3. Open a terminal in the root of the project
4. Run: `pip install -r requirements.txt`
 
## First Step
 
After you are all set up, you are finally ready to code! ;)
 
But where to start? In the GitHub repository you just cloned, you will find a file named `Src/brain.py`. In this file, you have the function `on_next_move(turn_info)` and it is in this function that you have to code your AI. It takes as parameter a `turn_info` which contains all the information given by the GameSever so that your AI can make a decision on its next move. You can find this model in `Src/Models/turnInformation.py`. The function must return a direction (up, down, left, right). If it fails to do this 3 times in a row, the GameServer will consider your AI to be obsolete. You can find these constants in `Src/Models/direction.py` (If you change these values, your next moves will be considered invalid). Another function found in `brain.py` is `on_finalized(turn_info)`. Once a game is finished, this method is triggered with the final state of the map and nothing needs to be returned to the GameServer.
 
The map you receive is encoded in a list of strings. Some of the encodings are:
- “P0*” : represente the head of the player with the id 0
- “P0” : represente part of the body of the player with id 0
- “p0” : represente part of the neck of the player with id 0
- “W” : A wall

Now you know where to start coding. As a first goal, try to get your AI from point A to point B as simply as possible.
 
Good luck!

## Local game simulation

### Download the local GameServer
1. Make sure you have [Docker](https://docs.docker.com/engine/install/) installed on your machine and __**running**__
2. Login into docker: 
    - With Docker Hub: `docker login`
    - With Github: `docker login https://docker.pkg.github.com`
	- You can login using your password or a personal access tokens
	- If using a personal access tokens:
		1. Create your personal access tokens [Here](https://github.com/settings/tokens)
		2. Make sure you give it the following scope: `read:packages`
		3. Login using the command: `docker login -u [GITHUB_USERNAME] -p [TOKEN_ID]`
3. Pull Docker images:
    - **Windows or MacOs**: `docker pull docker.pkg.github.com/readyaicode-2020/gslocal/gslocal:win_mac`
    - **Linux**: `docker pull docker.pkg.github.com/readyaicode-2020/gslocal/gslocal:linux`
4. Run the following command to know if the images was correctly loaded:
    - `docker images`
    - You should see a repository with the name of your images you just pull 
 
* To know how to clean up your docker environment at the end of the competition, see the last section of this ReadMe

### Execution
 
1. Run code
    1. Open a terminal in the root of the project
    2. Make sure the required packages are installed (see section **Install required python packages**)
    3. Run: `python main.py`
        - If it is not starting a game, sometime you need to restart the  GameServer Local container
    4. You can stop the execution by doing ctrl-c in the terminal
 
2. Run the local GameServer in Docker container
    1. Make sure docker image in install (see section **Setup GameServer Local**)
    2. In a terminal run: 
        - **Windows or MacOs**: `docker run --rm -d -p 5001:5001 --name gs_container docker.pkg.github.com/readyaicode-2020/gslocal/gslocal:win_mac`
        - **Linux**: `docker run --rm -d --network host -p 5001:5001 --name gs_container docker.pkg.github.com/readyaicode-2020/gslocal/gslocal:linux`
        - --rm: Remove the container when stop
        - -d: Hide the outputs, remove if you want see them
    3. You can verify if the container is running with: `docker ps`
 
3. Connect the Dashboard UI to the GameServer Local
    1. Make sure the GameServer Local container is running
    2. Go to the section **Local debug** in the event dashboard
    3. Click on the button **Start**
        - Attention: If you restart the GameServer Local container you will need to repeat this step to reconnect the UI
 
4. In case its not working, here’s a few things to try:
    - You can restart the container with: `docker restart gs_container`
    - You can stop the container with: `docker stop gs_container` and then repeat the steps from `Run the local GameServer in Docker container`
    
## Deploy code
 
1. If you installed and are using new packages, in requirements.txt add the packages names and its version. Otherwise your code will not work in deployment
	- [PACKAGE NAME]==[PACKAGER VERSION]
	- **Attention** Do not use: `pip freeze > requirements.txt`
 
2. Push the code on the Master branch
    1. Open a terminal in the root of the project
    2. Stage all changes: `git add .`
        - You can also stage on file at a time: `git add [file_path]`
    3. Commit changes: `git commit -m "Descriptive message"`
    4. Push code: `git push`
 
3. See the status if your deployment
    1. Go to the Actions tab in the Github repository
    2. See on the latest workflow
        - If it is on going it will have a yellow spinning wheel next to it
        - If it is done successfully it will have a green circle next to it
        - If it fails it will have a red cross next to it. Click on the workflow to see the logs and find out why it failed to deploy
 
3. Wait 2 to 4 minutes after the workflow is done successfully to make sure the deployment is fully completed on are servers 
 
**!ATTENTION!**: Only push on the Master branch when it's really necessary to avoid problems with the deployment pipeline. Problems may last a while and slow down your developpement process.
 
## Clean up
 
### Clean Docker environment
1. Stop and remove container
    1. `docker stop gs_container`
    2. If you remove --rm earlier: `docker rm gs_container`
2. Remove images
    - `docker rmi [images_name]`
    	- Use `docker images` to see the list of images and find its name or id  
3. Logout of docker
    - `docker logout`


