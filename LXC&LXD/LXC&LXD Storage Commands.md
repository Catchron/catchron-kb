## LXC/LXD Storage Pools
* LXC/LXD Uses two types of storage poools:
  * Directory Storage (creates a disk image based on the size you define in your local directory)
	* Block Storage (Uses a defined block storage)

* List all pools<br>
``lxc storage list``<br>
<img src="https://i.imgur.com/R7Luy6q.gif" width="700"/><br>
The "USED BY" value specifies the number of attributes that are using this storage pool. These attributes can be **profiles**, **images** or **containers**. Check the **lxc storage info** and the **lxc storage show** command.

* Create pool<br>
``lxc storage create [pool] [driver]``<br>
<img src="https://i.imgur.com/e76OoyY.gif" width="700"/><br>
Here we create a new storage pool without specifying a block device. It will create the pool as directory storage.<br>
<img src="https://i.imgur.com/e76OoyY.gif" width="700"/><br>
Here we create new storage pool and specify a block device from our list. The whole device will be used for containers.

* Remove a pool <br>
``lxc storage delete [pool]``<br>
You will need to remove any **profiles**, **images** or **containers**. Check the **lxc storage info** and the **lxc storage show**

* Edit a pool config<br>
``lxc storage edit [pool]``<br>
<img src="https://i.imgur.com/e76OoyY.gif" width="700"/><br>
This opens a yaml file for the storage pool configuration.

* Output pool info<br>
``lxc storage info [pool]``<br>
<img src="https://i.imgur.com/39ggGFA.gif" width="700"/><br>

* Show YAML pool config<br>
``lxc storage show [pool]``<br>
<img src="https://i.imgur.com/vwRGK1P.gif" width="700"/><br>

* Change a config value<br>
``lxc storage set [pool] [key] [value]``

* Retrieve a config value<br>
``lxc storage get [pool] [key]``

* Remove a config value<br>
``lxc storage unset [pool] [key]``
