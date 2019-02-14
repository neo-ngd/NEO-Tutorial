
# NEO Smart Contract Workshop (Part 1)
*by [Steve](https://github.com/HandsomeJeff) for NEO*

This workshop assumes intermediate knowledge of the command line.

This portion involves installing and setting up the environment.
This is part 1 of the workshop. [Part 2](https://github.com/HandsomeJeff/neo-python-workshop/blob/master/part2_neopy.md) is for command line operations. This portion involves interacting with neo-python command line.

___

### Workshop Details
**When**: Sunday, 1 Jul 2018. 1:30 PM - 5:30 PM.</br>
**Where**: 太库北京孵化器</br>
**Who**: NEO

### Questions
Please raise your hand any time during the workshop or email your questions to [me](mailto:yefan0072001@gmail.com) later.

### Errors
For errors, typos or suggestions, please do not hesitate to [post an issue](https://github.com/HandsomeJeff/NEO-smart-contract-workshop). Pull requests are very welcome! Thanks!

___

## Task 0 - Install Pre-requisites
Before you start, make sure you have the following:
1. **Linux/Mac Operating System**
  - Tested on Ubuntu (v16.04 and newer), Mint v18.3, CentOS v7.5, macOS Sierra
2. **Docker**
3. **Python3.6**
4. **leveldb**

For Docker, try running the following in terminal:
```
sudo apt-get install curl
sudo curl -sSL https://get.docker.com/ | sh
sudo apt-get update && apt-get upgrade
sudo service docker start
```

If you don't have Python 3.6, and have no idea how to install it, run the following commands:
```
sudo apt-get install software-properties-common python-software-properties
sudo add-apt-repository ppa:deadsnakes/ppa
sudo apt-get update
sudo apt-get upgrade
sudo apt-get install python3.6 python3.6-dev python3.6-venv libleveldb-dev libssl-dev g++
```
*For CentOS users, you can refer to this [guide](https://tecadmin.net/install-python-3-6-on-centos/) on how to install Python 3.6*

## Task 1 - Getting Started

In this session, we'll be setting up a local private blockchain which allows us to upload and execute smart contracts.

This will be hosted entirely on our own machines!

The process will go in the following order:
1. Setting up the Docker Container
2. Running NEO Python


#### 1.1 Setting up the Docker Container
Pull the latest image from Docker hub <br> `sudo docker pull cityofzion/neo-privatenet`.

![docker pull](assets/docker_pull.png)

Be patient! On the first time round it might take a while to start up.

*The Docker image we're pulling is hosted on currently overseas servers. If you're downloading it from a country like China without any special internet tools, it may take hours to load the full image.*

Start the container with the command <br>
`sudo docker run --rm -d --name neo-privatenet -p 20333-20336:20333-20336/tcp -p 30333-30336:30333-30336/tcp cityofzion/neo-privatenet`.

*You can open bash within the container with* <br> `sudo docker exec -it neo-privatenet /bin/bash`.

*Stop the container with* <br> `sudo docker rm -f neo-privatenet`.

#### 1.2 Running NEO Python
*If you don't have git, run `sudo apt-get install git`*

<!-- Navigate to a directory of your choice and download the neo-python repository with <br> `git clone https://github.com/CityOfZion/neo-python.git` -->

Navigate to a directory of your choice and download the neo-python repository at https://github.com/CityOfZion/neo-python/releases/tag/v0.7.1.

*Note: we are using v0.7.1 because we've received feedback that the lastest version is unable to start properly.*

Unzip the folder. Then enter it with `cd neo-python-0.7.1`.

<!-- Go into the neo-python folder with `cd neo-python`. -->

Create and start a virtual environment so our setup does not interfere with the rest of our machine.
```
sudo python3.6 -m venv venv
source venv/bin/activate
```

Install neo-python.
```
sudo pip install -r requirements.txt
sudo pip install -e .
```

Start neo-python with `np-prompt -p`.
You should see something like this:

![task 1 screenshot](assets/initialising.png)

You are now ready to move on to [Part 2](https://github.com/HandsomeJeff/neo-python-workshop/blob/master/part2_neopy.md).

## Acknowledgements

Special Thanks to [Peter Lin](https://github.com/peterlinx), [Jon](https://github.com/jonathanlimwj) and [Chris Hager](https://github.com/metachris).
