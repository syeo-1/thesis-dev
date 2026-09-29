# Info on how to set up dev workflow
- ideally, we want to standardize development workflows for geospatial analysis, so this will be an atempt to do so
- the set up assumes the user is either working in a windows or linux environment
- for windows set up go to section [HERE](#Windows-set-up!)
- for linux set up, go to section [HERE](#Linux-set-up!)

## meta info preamble

- the plan is to use docker with miniconda
-  why docker?
    - docker can be used to set up standardized development environments so that everyone can be on the same page for analysis in terms of software packages that people need. In short, a docker container isolates a section of the operating system so that nothing else touches it. This ensures all versions of libraries between people using the same container remain the same, so there should be no issues in sharing code and not being able to run things
- why micromamba?
    - conda is great for managing all the packages for data analysis. However, not all packages may be necessary. To save on space, miniconda also allows you to manually install only the packages that you deem are necessary for your analysis. Micromamba is even smaller by leaving out Python and allows package resolution to happen faster, saving both storage space and time for creating a container

## Windows set up!

- install wsl first
    - I'm assuming people are using windows 11. Currently not sure about earlier version set up :(
    - if you have windows 11, you should have wsl 2 already, but it won't be set up yet. 
    - follow these installation instructions: https://learn.microsoft.com/en-us/windows/wsl/setup/environment
- get docker desktop set up on windows
    - install docker desktop: https://docs.docker.com/desktop/setup/install/windows-install/
        - install Desktop for Windows -x86_64
        - follow recommended installation instructions
- verify installations
    - open a new cmd window or powershell and type `docker -- version` and enter to see docker details. If it's installed it'll give info, otherwise it will return an error
- run the container. Building the image seems to take about 5-6 mins on carleton wifi
    - be sure you're running commands from within the dev-workflow-setup directory!
    - create an image using the dockerfile. Skip to additional set up section if instead you have a dedicated GPU since that may give a performance boost
        - an image is a text file that defines the steps for building an image and an application. (i copy pasted this from docker's documentation)
        - run the following: `docker build -t geo_env`. This build the image to run the application which will contain our analytics code (notebooks, scripts, etc...)
    - run the resulting container
        - in powershell run: `docker run --rm -p 127.0.0.1:8888:8888 -v "$PWD":/workspace geo_env`
- check if jupyter is up and running on your local machine
    - go to the url shown in the terminal. Something along the lines of: http://127.0.0.1:8888/lab. Likely won't be exact.
- additional set up (IMPORTANT!!)
    - check if have nvidia drivers installed
        - run `nvidia-sme` in a powershell terminal. if the command works, you have nvidia drivers and can continue. Otherwise, install the drivers.
            - IMPORTANT NOTE!!!!:  remember the CUDA version output in the headers for the table output. use that number when running the command with CUDA_OVERRIDE. Replace the value in the command with the number you see in the header (or a value below). This version is the highest version your drivers on your computer will support.
    - GPU usage
        - if you have a dedicated gpu, you can use it for saving training time especially with libraries like pytorch, so here's some additional set up to use it. By default the CPU is used when the image is created.
        - to create an image that assumes the existence of a dedicated gpu, run the following build command instead: `docker build --no-cache -t geo_env .`. Be sure you included the dot for the directory!
        - then run it like so: `docker run --rm --gpus all -p 127.0.0.1:8888:8888 -v "${PWD}:/workspace" geo_env`
    - verify your dedicated GPU is being used by the application created from the dockerfile
    


## Linux set up!
![Patrick Building](./patrick-building.jpg)