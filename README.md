# vagrant
Usando Vagrant para provisionar uma máquina virtual no virtualBox


Writing a Vagrantfile
By deparkes | April 30, 2018 0 Comments
Vagrantfiles are used to configure vagrant virtual machines. This post goes a little beyond the basics of creating a Vagrantfile, and looks at some of the more advanced topics for writing a vagrantfile to make a more polished machine.

I have posted before about how you can provision your virtual machine via the vagrantfile – for example set which packages should be installed when the machine is created. This post shows how you can also use the vagrantfile to define characteristics of the machine itself including custom machine names, memory and cpu, networking and shared folders.

Naming the Vagrant Machine
When you create a vagrant virtual machine you will see that vagrant has given the virtual machine a fairly generic name. This might be fine for in some cases, but you may also want to have more control over the virtual machine name. There are actually a few different ways to interpret the ‘machine name’:

vagrant has its own name for the virtual machine created
the provider (e.g. virtualbox) will display a name for the virtual machine
the ‘hostname’ as it appears on the command line.
Vagrant’s Own Name
Firstly, let’s take a look at the name vagrant uses for the machine. Default is DIRECTORY_default_TIMESTAMP – this is often fine, but you may want to customise it. You can do this with:
``
 Vagrant.configure("2") do |config|
   config.vm.box = "hashicorp/precise64"
   config.vm.define "custom_vagrant_name"
end
`` 
Provider GUI Name
The name of the vagrant machine can also appear in the provider itself. You may also want to customise this too. This will be provider-specific, so you should check the details for your provider. If you are using virtual box you can use:

Vagrant.configure("2") do |config|
  config.vm.box = "hashicorp/precise64"
  config.vm.provider "virtualbox" do |v|
   v.gui = true
   v.name = "custom_vm_name"
  end
end

The Hostname
The hostname is what your machine will appear as on a network, and what it will say on the command line when you log in.  The host name limited by soem rules or conventions of, mainly that it can be letters seprated by hypen or dot. Here’s an example:

Vagrant.configure("2") do |config|
 config.vm.box = "hashicorp/precise64"
 config.vm.hostname = "my-vagrant-machine"
end
 

Setting Memory and Number of Cores
Setting the hardware configuration can be really useful to do, particularly the memory and number of cores / CPUs. This is something else that is specific to the provider that you are using.

For example, the virtualbox provider can be configured like this:

Vagrant.configure("2") do |config|
  config.vm.box = "hashicorp/precise64"
 config.vm.provider "virtualbox" do |v|
   v.gui = true
   v.memory = 1024
   v.cpus = 2
  end
end

Opening / Forwarding Ports
You can open up ports / allow forwarding to allow you to access a port on your host machine and have all data forwarded to a port on the guest machine. You configure port forwarding with

Vagrant.configure("2") do |config| 
 config.vm.box = "hashicorp/precise64"
 config.vm.network "forwarded_port", guest: 80, host: 8080
end
 
Sharing Folders
Setting up a shared or synced folder makes working with a virtual machine much easier – you can share files between the host and the guest machine. You can do this in vagrant with the ‘synced_folder‘ option:

Vagrant.configure("2") do |config|
 config.vm.box = "hashicorp/precise64"
 config.vm.synced_folder "local_folder", "/guest/folder"
end

Which will sync a folder ‘local_folder’ in the same directory as your vagrantfile with a file in ‘/home/vagrant/guest/folder’. You can also explore more advanced folder syncing options such as NFS, rsync and samba shares.

Bringing It All Together
As an example of some of these more advanced vagrant file concepts here is an example – reference oommf vagrant build.

Vagrant.configure("2") do |config|
 config.vm.box = "hashicorp/precise64"
  
 config.vm.network "forwarded_port", guest: 80, host: 8080
 config.vm.hostname = "my-vagrant-machine"
 config.vm.define "custom_vagrant_name"
 config.vm.synced_folder "local_folder", "/guest/folder"
  
 config.vm.provider "virtualbox" do |v|
 v.gui = true
 v.name = "custom_vm_name"
 v.memory = 1024
 v.cpus = 2
 end
  
end



