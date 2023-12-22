Manual Steps to build the application
---------------------------------------
![Preview](Images/jenkins.png)
* Install docker
```
sudo apt install
curl -fsSL https://get.docker.com -o install-docker.sh
sh install-docker.sh
sudo usermod -aG docker ubuntu
# exit the server
# and login the server again
docker info

```
![Preview](Images/jenkins1.png)
![Preview](Images/jenkins2.png)

* Clone the github repository to the server [Refer Here](https://github.com/rajkumarqt/ProjectsRelated/tree/main/devsecopsproject) for the project repository.
* 
* To build the docker image run the below command
```
docker image build -t <your-image-name>:latest .
```