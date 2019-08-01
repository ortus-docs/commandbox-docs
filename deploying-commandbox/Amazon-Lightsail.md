Amazon Lightsail is a service that lets you spin up managed service quickly with a shell script.  

# Setting up a Server

Here is an example script to get you up and running with CommandBox. 

```bash
sudo apt-get update
#install java for commandbox
yes | sudo apt-get install default-jre

#taken straight from ortus docs commandbox install
curl -fsSl https://downloads.ortussolutions.com/debs/gpg | sudo apt-key add -
echo "deb http://downloads.ortussolutions.com/debs/noarch /" | sudo tee -a /etc/apt/sources.list.d/commandbox.list
sudo apt-get update && sudo apt-get install commandbox

#might need this later for webpack
yes | sudo apt install npm
#clone your repo to the /app directory, just like Ortus does
sudo git clone https://github.com/coldbox-templates/elixir-vuejs.git /app

#start server and expose it to the world
cd /app && sudo box start --debug host=0.0.0.0 port=80
sudo box install

```
