FROM ubuntu:16.04
RUN apt-get update && apt-get -y upgrade
RUN apt-get install wget -y
# Get required packages
Workdir /root
Run wget https://docs.google.com/uc?id=0B3X9GlR6EmbnWksyTEtCM0VfaFE&export=download
Run mv uc\?id\=0B3X9GlR6EmbnWksyTEtCM0VfaFE gdrive

Run chmod +x gdrive

Run sudo install gdrive /usr/local/bin/gdrive
