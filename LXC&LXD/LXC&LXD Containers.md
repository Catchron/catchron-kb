## LXC&LXD Container Commands

* To launch a new Container<br>
``lxc launch [<remote>:]<image> [<remote>:][<name>] [flags]``<br>
<img src="https://i.imgur.com/qdnU720.gif" width="700"/><br>
In this example we are creating a container based on the public image of **images:alpine/3.12**. When we do not specify any other flags or storage pools, this container will be created in the **storage pool** tagged with the **default** profile with a **random generated name** (in this case **guiding-moth**)

* To list containers<br>
``lxc list [<remote>:] [<filter>...] [flags]``<br>
<img src="https://i.imgur.com/koFBGGs.gif" width="700"/><br>
In this example we are simply listing for all containers across all storage pools and profiles
<img src="https://i.imgur.com/D5XIwU7.gif" width="700"/><br>
In this example we are listing all containers and passing the ``-c`` flag to specify the columns we want to see (Name, Storage Pool, Profiles, State)

* To copy an image locally:<br>
``lxc image copy [<remote>:]<image> <remote>: [flags]``<br>
<img src="https://i.imgur.com/9hyiiiV.gif" width="700"/><br>
In this example we are copying the **images:alpine/3.12** to our **local** storage cache and giving it an alias name **alpine-3.12**.

* To check your container configuration
``lxc config show [<remote>:][<container>] [flags]``<br>
<img src="https://i.imgur.com/7vUj2of.gif" width="700"/><br>
In this example we are using a local container name. The output gives us the yaml file which is generated when we setup the container.
