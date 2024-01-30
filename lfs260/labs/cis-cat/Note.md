

- Get CIS CAT tools
    - Copy the link from the email sent by CIS

```bash

# Wget the link from the email sent by CIS
wget https://learn.cisecurity.org/e/799323/l-799323-2019-11-15-3v7x/2mnnf/1921873092/h/3gd508xx9wdz3KnfQ6CrqpG47yEn6xFpGFR6Glayics

# reveil the zip file
mv 3gd508xx9wdz3KnfQ6CrqpG47yEn6xFpGFR6Glayics cis-cat.zip

# unzip the file
unzip cis-cat.zip

cd Assessor/

# The tool requires Java 11
sudo apt-get install openjdk-11-jdk -y
JAVA_PATH=/usr/lib/jvm/java-11-openjdk-amd64/bin/

# -i for interactive mode
sudo bash Assessor-CLI.sh -i

# input the options
# Yield a html report
ls /Assessor/reports

```

