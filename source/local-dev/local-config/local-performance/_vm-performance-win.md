If you notice Nanobox and your apps running a little slowly, there are changes you can make that can drastically improve performance.

## Use netfs for Filesystem Mounting
Whenever you run `nanobox dev start`, Nanobox mounts the current working directory into your Nanobox container. This allows changes made to code on your local machine to immediately appear in Nanobox container. The default `native` method used to mount code can be slow. Switching to `netfs` will greatly improve code read/write speeds in Nanobox.

*"netfs" is a generic term for "network filesystem". The actual technology used in Windows is [CIFS](https://technet.microsoft.com/en-us/library/cc939973.aspx).*

### Using netfs
To use netfs, you'll need to modify your [`config.yml`](/local-dev/nanobox-config-yml). This file can be found at `C:\Users\username\.nanobox\config.yml` or `C:\%userprofile%\.nanobox\config.yml` . If it doesn't already exist, you can create it. Update the `mount-type` setting to `netfs`.

#### netfs in the config.yml
```yaml
mount-type: netfs
```

### Things to Know When Using netfs
#### netfs Requires Admin Privileges
netfs does require admin privileges in order to create the mount. When Nanobox detects the `netfs` setting in your `config.yml`, it will prompt you for your administrative password whenever you start a dev environment.


## Virtual Machine Settings
Nanobox uses [Virtual Box](https://www.virtualbox.org/) to create the Nanobox container on your local machine. Virtual Box Manager (installed with Virtual Box) allows you to configure what system resources are allocated to the container and how those resources are used.

#### Start/Stop Your Nanobox Container
In order to make the recommended changes to your virtual machine configuration, the Nanobox container must first be created. If you haven't created a Nanobox machine yet, just run:

```bash
# Create your nanobox container
nanobox start
```

If your Nanobox has been created, you'll need to stop it in order to make these changes:

```bash
# Stop your nanobox container
nanobox stop
```

### Update Nanobox VM Settings
To access these settings, open Virtual Box Manager and, select the nanobox virtual machine, then click on the "Settings" icon in the top nav.

![Virtual Box VM Settings](vm-performance-win-settings.png)

With the settings window open, click on the "System" tab.


### Memory
Inside System Settings, there's a "Motherboard" tab. Here you're able to adjust how much RAM is allocated to your Nanobox container. Virtual Box Manager will show you how much memory is safe to allocate to your system. The more memory you allocate to your Nanobox machine, the more performant your dev apps will be. However, we recommend staying in the green.

![Virtual Box VM Settings](vm-performance-win-ram.png)

### Processor
Under the "Processor" tab, you're able to specify how many of your system's available CPU cores should be allocated to your Nanobox container as well as how much of each of those cores can be utilized.

The more CPU cores you allocate, the more performant Nanobox will be. Setting your execution cap low will prevent your container from hurting the performance of your host machine, but it can also starve the container of required compute power. Again, Virtual Box Manager will show you what's safe for your machine. We recommend staying in the green.

![Virtual Box VM Settings](vm-performance-win-cpu.png)

### Acceleration
Under the "Acceleration" tab of your system settings, you have the option to select a Paravirtualization Interface (essentially a virtualization emulator). The most efficient interface offered is "KVM".

![Virtual Box VM Settings](vm-performance-win-kvm.png)

Once all your changes are made, click "OK" to save.