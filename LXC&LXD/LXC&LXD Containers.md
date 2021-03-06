## LXC&LXD Container Commands

* To launch a new Container<br>
``lxc launch [<remote>:]<image> [<remote>:][<name>] [flags]``<br>
<img src="https://i.imgur.com/qdnU720.gif" width="700"/><br>
In this example we are creating a container based on the public image of **images:alpine/3.12**. When we do not specify any other flags or storage pools, this container will be created in the **storage pool** tagged with the **default** profile with a **random generated name** (in this case **guiding-moth**)

* To list containers<br>
``lxc list [<remote>:] [<filter>...] [flags]``<br>
<img src="https://i.imgur.com/koFBGGs.gif" width="700"/><br>
In this example we are simply listing for all containers across all storage pools and profiles<br><br>
<img src="https://i.imgur.com/D5XIwU7.gif" width="700"/><br>
In this example we are listing all containers and passing the ``-c`` flag to specify the columns we want to see (Name, Storage Pool, Profiles, State)

* To copy an image locally:<br>
``lxc image copy [<remote>:]<image> <remote>: [flags]``<br>
<img src="https://i.imgur.com/9hyiiiV.gif" width="700"/><br>
In this example we are copying the **images:alpine/3.12** to our **local** storage cache and giving it an alias name **alpine-3.12**.

* To check your container configuration:<br>
``lxc config show [<remote>:][<container>] [flags]``<br>
<img src="https://i.imgur.com/7vUj2of.gif" width="700"/><br>
In this example we are using a local container name. The output gives us the yaml file which is generated when we setup the container. The output shows the different keys/values that are set for this container

* To see a specific key/value for a container:<br>
``lxc config get [<remote>:][<container>] <key> [flags]``
* To change a specific key/value for a container:<br>
``lxc config set [<remote>:][<container>] <key> <value> [flags]``<br>
<img src="https://i.imgur.com/mKA6Rh3.gif" width="700"/><br>
In this example we are changing the **boot.autostart** key to **false** - which will prevent the container from starting at boot. We are then checking the state for the **boot.autostart** key to confirm.<br>
Check the list of the [currently supported keys](https://linuxcontainers.org/lxd/docs/master/instances/#key-value-configuration) at the **linuxcontainers.org** site.

* To change a specific key/value back to its default state:<br>
``lxc config unset [<remote>:][<container>] <key> [flags]``<br>
Example: ``lxc config unset ethical-rooster boot.autostart``<br>

* To list the devices configured for your container:<br>
``lxc config device list [<remote>:]<container|profile> [flags]``<br>
* To get the values for container device configuration keys:<br>
``lxc config device get [<remote>:]<container|profile> <device> <key> [flags]``<br>
* To show full device configuration for containers or profiles:<br>
``lxc config device show [<remote>:]<container|profile> [flags]``<br>
<img src="https://i.imgur.com/XyKbDaK.gif" width="700"/><br>
In this example we are listing the devices for the **ethical-rooster** container - and those devices are **root**.<br> We are then checking which storage pool the root devices for the container is connected to - the storage pool named **lxd**.<br> Finally we are listing all the configuration for the device.

  * What does the **root** device mean.

* To access the **shell** on your server:<br>
``lxc exec [<remote>:]<container> [flags] [--] <command line>``<br>
<img src="https://i.imgur.com/V7T2Hca.gif" width="700"/><br>
In this example, we are accessing the **bash shell** on our **renewing-mackerel** container which uses **Ubuntu 18.04** Image.<br>
  * The shell will depend on the distro image you are using. For Alpine Example:<br> ``lxc exec ethical-rooster -- ash``
* Using the same command you can also pass commands to the container without actually accessing its **shell**:<br>
<img src="https://i.imgur.com/aqIA7NS.gif" width="700"/><br>
In this example we are directly installing **Nginx** to our **Ubuntu 18.04** container.
* If you want to chain multiple commands (with ``&&``) or pipe them (with ``|``) you will need to pass the ``sh -c`` argument and enclose the commands in quotes.<br>
``lxc exec [<remote>:]<container> [flags] [--] sh -c '<command line>'``<br>
<img src="https://i.imgur.com/yqAq7Bg.gif" width="700"/><br>
In this example we are chainin the two commands to **start** the **nginx** service and **enable** it on boot.

* To stop a container:<br>
``lxc stop [<remote>:]<container> [[<remote>:]<container>...] [flags]``<br>
Example:<br>
``lxc stop renewing-mackerel``

* To delete a container:<br>
``lxc delete [<remote>:]<container>[/<snapshot>] [[<remote>:]<container>[/<snapshot>]...] [flags]``<br>
Example:<br>
``lxc delete renewing-mackerel``

* To edit a file in a container:<br>
``lxc file edit [<remote>:]<container>/<path> [flags]``<br>
<img src="https://i.imgur.com/6Zd02NV.gif" width="700"/><br>
In this example we are editing the **default** file at its location on the **web01** container.

* To pull a file from a container to your host machine:<br>
``lxc file pull [<remote>:]<container>/<path> [[<remote>:]<container>/<path>...] <target path> [flags]``<br>
<img src="https://i.imgur.com/TKewPS7.gif" width="700"/><br>
In this example we are pulling the same default file to our home folder.

* To push a file from your host into a container:<br>
``lxc file push <source path> [<remote>:]<container>/<path> [[<remote>:]<container>/<path>...] [flags]``<br>
<img src="https://i.imgur.com/M4wlJcz.gif" width="700"/><br>
In this example we are pushing the **intrasite** file to our **web01** container in the ``/etc/nginx/sites-available/`` directory.

* To remove a file from a container:<br>
``lxc file delete [<remote>:]<container>/<path> [[<remote>:]<container>/<path>...] [flags]``<br>
<img src="https://i.imgur.com/Ae7mbEa.gif" width="700"/><br>
In this example we are deleting the **intrasite** file from our **web01** container.
