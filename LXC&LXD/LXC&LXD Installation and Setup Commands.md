## Installing LXC/LXD

``sudo apt install lxc-utils``<br>
``sudo snap install lxd --channel=3.0/stable``<br>
``sudo apt remove lxd lxd-client``<br>
``sudo usermod [user] -aG lxd``<br>

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

* To check your current configuration
