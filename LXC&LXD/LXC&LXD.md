## Installing LXC/LXD

``sudo apt install lxc-utils ``
``sudo snap install lxd --channel=3.0/stable``
``sudo apt remove lxd lxd-client``
``sudo usermod [user] -aG lxd``

## Running LXD

- we run ``lxd init`` in order to configure network access and the storage pool for our containers.<br>
``lxd init``<br>

- ``lxd init`` pompts for the following 6 items:
 1. Enable/Disable [Clustering]()
 2. Configure [Storage Pool]() (new or existing)
 3. Enable/Disable Metal-as-a-Service
 4. Configure [Networking]() (new or existing)
 5. Enable/Disable auto-updates for images
 6. Create a [preseed file]()
 * After these items a yaml file will be generatd which is our **preseed.yaml** file. Later we can use this yaml file if we want to configure another server to be exactly as this configuration. The command to use a preseed file is:<br>
 ``cat preseed.yaml | lxd init --preseed``

## LXC/LXD Storage Pools
* LXC/LXD Uses two types of storage poools:
  * Directory Storage (creates a disk image based on the size you define in your local directory)
	* Block Storage (Uses a defined block storage)

* Create pool<br>
``lxc storage create [pool] [driver]``<br>
<img src="https://i.imgur.com/e76OoyY.gif" width="700"/><br>
Here we create a new storage pool without specifying a block device. It will create the pool as directory storage.<br>
<img src="https://i.imgur.com/e76OoyY.gif" width="700"/><br>
Here we create new storage pool and specify a block device from our list. The whole device will be used for containers.

* Remove a pool <br>
``lxc storage delete [pool]``
* Edit a pool config<br>
``lxc storage edit [pool]``
* Retrieve a config value<br>
``lxc storage get [pool] [key]``
* Output pool info<br>
``lxc storage info [pool]``
<img src="https://i.imgur.com/39ggGFA.gif" width="700"/><br>
* List all pools<br>
``lxc storage list``
* Change a config value<br>
``lxc storage set [pool] [key] [value]``
* Show YAML pool config<br>
``lxc storage show [pool]``
<img src="https://i.imgur.com/vwRGK1P.gif" width="700"/><br>
* Remove a config value<br>
``lxc storage unset [pool] [key]``
