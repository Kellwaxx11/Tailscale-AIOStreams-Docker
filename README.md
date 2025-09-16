# Tailscale-AIOStreams-Docker
Simple guide to selfhost AIOStreams stremio addon and expose it to the internet using Tailscale Funnel

**Instructions (WIP)**

1- in Tailscal admin console page, under access controls tab, tailnet policy file make sure to add "tag:container" under tagOwners as well as under "nodeAttrs" target, and the funnel is enabled
it might look different depending on your tailnet policy file configuration, but mine looks like this on that part

    "tagOwners": {
		    "tag:container": ["autogroup:admin"],
      },
	  "nodeAttrs": [
	  	{
		  	"target": ["autogroup:member", "tag:container"],
	  		"attr":   ["funnel"],
  		},
  	],
2- creat OAuth clients in Tailscale settings, enable write for All, and save that Client Secret code.
3- create a folder in your home directory, name it whatever you like to, for example aiostreams

    mkdir aiostreams
    
4- create **compose.yaml**, using your favorite editor, and copy the content of mine
dont forget to change the OAuth key to the one you got from step 2
also feel free to rename the container to your liking, but make sure to change it everywhere in the file to match it
and the hostname as it will be the one showing in your Tailscale as a new device

5- create the **config/serve-config.json** to enable Tailscale Funnel and proxy
copy the content of mine and save it


6- make .env file for aiostreams, example can be found in the official github deployment 
guide https://github.com/Viren070/AIOStreams/wiki/Deployment

    curl -o .env https://raw.githubusercontent.com/Viren070/AIOStreams/main/.env.sample

make sure to change the BASE_URL to the domain name you will get from Tailscale at the end
it should look somehting like  

    https://CONATINER_HOST_NAME.your-tailscale-domain
    for example 
    https://ts-aiostreams.tailxxxxxx.ts.net

7- create and start the docker container (while in the same folder as compose.yaml)

    sudo docker compose up -d


If all good you should see a new node (device) in your tailscale admin page names **ts-stremio** or to the name you changed hostname to

To use your self hosted addon , copy the domain name from Tailscale admin page, under machines, next to your new device 
it should look something like

    ts-aiostreams.tailxxxxxx.ts.net
