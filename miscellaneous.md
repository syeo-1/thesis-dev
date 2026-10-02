# miscellaneous
- some info specific to my set up that I'd like to keep track of. also other potentially useful things to keep track of

## commands

- from docker desktop, first click on the bottom left whale icon to start the docker enginer. Then in powershell, from within the thesis-dev directory use the command in the following point
- run the following to get up and running on my local development. Use the explicit path `docker run --rm --gpus all -p 127.0.0.1:8888:8888 -v "C:\Users\seans\Documents\Carleton-masters\thesis-code-programming\thesis-dev:/workspace" geo_env`
    - the above ensures that the bind mount is properly set up so that all changes when I'm working on files within the container are saved locally
- after running that command, in vscode use CMD+Shift+P to go to use the dev containers extension to attach to a running container. Choose geo_env.
- open the workspace folder (/workspace) in geo_env which should have the files that have been bind mounted. This is specified in the command from the section `...-v "local_path:mount_location"...`

- fresh docker build
`docker build --no-cache -t geo_env .`

## adding in new libraries or packages to the container

- for now, for python packages, add them as necessary via pip from within the container
    - that way, it's quick to do strict development work
- rebuild images at night since to truly add a package to the container requires a rebuild of the package which can take an hour to run
    - this requires editing the environment.yml file (in this case environment-gpu.yml) and then rerunning the build image command
        - that command is: `docker build --build-arg ENV_FILE=environment-gpu.yml -t geo_env`

## potentially useful resources

https://github.com/Abdallah-M-Ali/Mineral-Prospectivity-Mapping-ML/blob/main/Data_preprocessing.py

## other data
- I took canada land boundary data from 2021 census: https://www12.statcan.gc.ca/census-recensement/2021/geo/sip-pis/boundary-limites/index2021-eng.cfm?Year=21
- also, this data set to cover lakes and water bodies within Canada: https://www.hydrosheds.org/products/hydrolakes
- The above two data sources are used to ensure negative label points are on land and within Canada