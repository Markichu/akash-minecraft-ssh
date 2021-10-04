This repository outlines how to launch your own highly optimised Minecraft server using the Akash decentralized cloud (DeCloud).

This repo will use our [custom made Docker image](https://github.com/Markichu/docker-minecraft-server-ssh) that is a fork of the popular [itzg/docker-minecraft-server](https://github.com/itzg/docker-minecraft-server) but with added SSH support.

![minecraft-server](https://user-images.githubusercontent.com/31204091/135333948-a56c84c2-b462-491f-a5c3-c0c666fe3a14.png)

Join our testing Minecraft server! It is running the recommeded deploy.yaml, come and see the performance with your own eyes.

    cluster.provider-2.prod.ewr1.akash.pub:32366

## Example Deploy.yaml

For our example, we have created an ultra-optimised deploy.yaml that can be modified to suit your needs. Below we will outline all the environment variables that we used to make this a performance beast.

`TYPE=AIRPLANE` Firstly, we start by running the server using an [Airplane](https://airplane.gg/) jar. Airplane is a very performant fork of [PaperMC](https://papermc.io/), you can read more about it [here](https://blog.airplane.gg/about/).

`VIEW_DISTANCE=6` Reducing the servers player view distance helps to reduce the number of chunks loaded, especially with high player counts.

`ENABLE_AUTOPAUSE=TRUE` Adds an auto-pause feature to the server so that when there are no players on the server it enters a _sleep_ mode to lower CPU usage. The two arguments `AUTOPAUSE_TIMEOUT_EST=300` and `AUTOPAUSE_TIMEOUT_INIT=300` reduce this wait time to 5 minutes. `MAX_TICK_TIME=-1` is also needed for this to function correctly.

`USE_AIKAR_FLAGS=TRUE` Optimizes the JVM options to use the so-called [Aikar Flags]( https://aikar.co/2018/07/02/tuning-the-jvm-g1gc-garbage-collector-flags-for-minecraft/). These flags are an industry standard in JVM performance for Minecraft server and can significantly improve the performance of a server.

For our example deployment, using the recommended resource settings, we have found that this runs very smoothly and using Akashlytics we can see that the estimated monthly cost is ~3 AKT. At the time of writing this puts the monthly cost at just under $10.

## Deploying the Server

You can deploy this script however you wish, through the [CLI as shown in the Akash docs]( https://docs.akash.network/cli/deployment) or other. 

We recommend trying out [Akashlytics UI](https://akashlytics.com/deploy) as it very intuitive and very easy compared to using the CLI tool. Be warned though, Akashlytics is still in beta and may be prone to bugs, be sure to back up your wallet before using it.

## Interacting with the Server

### Getting Server IP

The server’s IP, along with the relevant ports can be retrieved by using following CLI command:

    akash provider lease-status --node $AKASH_NODE --from $AKASH_KEY_NAME --dseq $AKASH_DSEQ --provider $AKASH_PROVIDER

It can also be found under the details section when using Akashlytics by clicking in the buttons which have the port mappings on them.

![ss+(2021-09-30+at+07 01 03)](https://user-images.githubusercontent.com/31204091/135351146-7ddce74b-f4ec-48f7-85ce-bc0212f82163.png)

To connect to your Minecraft server, simply connect using the given provider followed by the external port.

### Viewing Server Console

To view the server console using CLI please use the following command:

    akash provider lease-logs --node $AKASH_NODE --from $AKASH_KEY_NAME --dseq $AKASH_DSEQ --provider $AKASH_PROVIDER

To view the console using Akashlyrics, simply view the Logs section under your deployment.

### Using SSH and Accessing Admin Console

To access the console, you must connect to the server IP on the SSH port using your favourite SSH client. For windows, we recommend [PuTTY](https://www.putty.org/).

After connecting, you will need to login using the SSH credentials you set in the environment variables. If you did not set any credentials, the default username and password will be `user` and `pass`.

You will now be in the Minecraft server directory, to access the admin console execute the following command:

    rcon-cli --port 25575 --password minecraft

You can now run any Minecraft command as an admin.

If you cannot connect to SSH, ensure you set the `SSH=TRUE` environment variable, without this the SSH server will not run.
 
### Accessing Files with SCP

To access the file structure of your deployment, you will need to connect using the SSH port with your favourite SCP client. For windows we recommend [WinSCP](https://winscp.net/eng/index.php). Ensure that you set the file as SCP and set the port as the external SSH port.

By default you should arrive in the Minecraft server directory, from here you can modify any file related to your deployment.

The primary use of this is to backup the world of the server to your local PC. This should be done regularly as if the container crashes, all world data will be lost. 

To start the server with your own world, please see the [relevant section](https://github.com/itzg/docker-minecraft-server#downloadable-world) on the Docker image repo.

## Adjusting Minecraft Docker Image

### Increasing Performance ###

If you find that your server is lagging too much or just lacking performance, try changing the resources section in the deploy.yaml.

We recommend first attempting to increase the RAM used by increasing `memory: size: XGi` in the resources section and the `MEMORY=XGi` environment variable. When increasing these, you should make the overall resource allocation a gigabyte or two more than overall resource allocation or your deployment might run out of RAM.

If you are still facing issues after attempting to increase the RAM, then increase the number of CPU units allocated to the deployment under `cpu: units: X` under the resources section.

### Adding More Functionality ###

If you want to add more features to your deployment please see the respective repositories for explanations of their respective environment variables.

For SSH-related modifications please see our fork at [markichu/docker-minecraft-server-ssh]( https://github.com/Markichu/docker-minecraft-server-ssh).

For adding mods, or anything else please see the upstream repo at [itzg/docker-minecraft-server](https://github.com/itzg/docker-minecraft-server).

#### Useful Environment Variable Sections

* [Starting the server with your own world](https://github.com/itzg/docker-minecraft-server#downloadable-world)
* [Running a CurseForge Modpack](https://github.com/itzg/docker-minecraft-server#running-a-server-with-a-curseforge-modpack)
* [Running a different server type](https://github.com/itzg/docker-minecraft-server#server-types)

## Support itzg

This Akash deploy would not be possible without the work of [itzg](https://github.com/itzg) and all the other brilliant contributors that have worked on [itzg/docker-minecraft-server](https://github.com/itzg/docker-minecraft-server). Itzg deserves plenty of credit for the original Docker image, consider [buying them a coffee]( https://www.buymeacoffee.com/itzg) or [donating to them on paypal]( https://paypal.me/itzg).
