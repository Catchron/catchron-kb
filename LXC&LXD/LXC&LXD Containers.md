## LXC&LXD Container Commands

* To launch a new Container<br>
``lxc launch [<remote>:]<image> [<remote>:][<name>] [flags]``<br>
<img src="https://i.imgur.com/qdnU720.gif" width="700"/><br>
In this example we are creating a container based on the public image of **images:alpine/3.12**. When we do not specify any other flags or storage pools, this container will be created in the **storage pool** tagged with the **default** profile with a **random generated name** (in this case **guiding-moth**)

* To copy an image locally:
lxc image copy [<remote>:]<image> <remote>: [flags]
<img src="https://i.imgur.com/9hyiiiV.gif" width="700"/><br>
In this example we are copying the **images:alpine/3.12** to our **local** storage cache and giving it an alias name **alpine-3.12**.
