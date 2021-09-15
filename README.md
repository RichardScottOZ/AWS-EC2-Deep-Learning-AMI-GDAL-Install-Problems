# AWS-EC2-Deep-Learning-AMI-GDAL-Install-Problems
Problems encountered with trying to get this to work

## System Libraries
The default install has none.
If you:
- sudo apt-get install gdal-bin libgdal-dev
- gdal-config --version
- 
1.1.3

This is not supported with a pip install - too old.

### Next
- sudo add-apt-repository -y ppa:ubuntugis/ppa
- sudo apt-get update 
- gdal-config --version

2.2.4 

# tensorflow_p37 environment

