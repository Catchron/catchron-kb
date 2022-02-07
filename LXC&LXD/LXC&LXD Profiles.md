## LXC&LXD Profiles
LXC Profiles are an easy way to apply custom configuration to your containers.

* To list all your current profiles:<br>
``lxc profile list [<remote>:] [flags]``<br>
<img src="https://i.imgur.com/wiVJefp.gif" width="700"/><br>
In this example we can see that there is one **default** profile that was created with ``lxd init`` and that it is being used by 3 containers<br>

* To see the info for a specific profile:<br>
``lxc profile show [<remote>:]<profile> [flags]``<br>
<img src="https://i.imgur.com/hh4xbxc.gif" width="700"/><br>
In this example we are getting the yaml info for the **default** profile and that it is used by three containers. You can apply a custom configuration to this profile but cannot delete or rename it.

* To create a new profile:<br>
``lxc profile create [<remote>:]<profile> [flags]``<br>
<img src="https://i.imgur.com/WlImR5l.gif" width="700"/><br>
In this example we are creating the **test-profile** profile.

* A newly created profile will not have any configuration or devices attached to it:<br>
<img src="https://i.imgur.com/vx7q2A1.gif" width="700"/><br>
In this example we can see both of our current profiles (the **default** and the **test-profile**) and that our newly created profile does not have any devices or custom configuration.

* To edit the configuration of a profile:<br>
``lxc profile set [<remote>:]<profile> <key> <value> [flags]``<br>
<img src="https://i.imgur.com/ep9yLNb.gif" width="700"/><br>
In this example we are setting the RAM limit of the **test-profile** to 1GB

* To apply a profile to a container:<br>
``lxc profile add [<remote>:]<container> <profile> [flags]``<br>
<img src="https://i.imgur.com/PEqWVGQ.gif" width="700"/><br>
In this example we are assigning the **test-profile** to the **eager-liger** container.

* Profiles can be applied to different containers. One container can have multiple profiles assigned to it. If we've set specific configuration to a profile - that container will adopt this configuration.<br>
<img src="https://i.imgur.com/SedzAA0.gif" width="700"/><br>
In this example we are setting the **RAM limit** for the **test-profile** to 1GB. The **eager-lifer** container has the **test-profile** assigned to it. Once we jump to the container and check the available memory we can see that the total is set to 1GB. The profile configuration changes are adopted by the containers in real time.

* To manually edit a profile:<br>
``lxc profile edit [<remote>:]<profile> [flags]``<br>
<img src="https://i.imgur.com/fwiePUx.gif" width="700"/><br>
In this example we are adding a new storage pool under the devices section. All containers that have this profile assigned will also use this storage pool.
