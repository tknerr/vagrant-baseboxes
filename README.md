
# Veewee Basebox Definitions

Some Veewee based basebox definitions for building 'Vagrant' baseboxes and 'Bare OS' baseboxes.

 * 'Vagrant' baseboxes already contain Ruby, Gems, Chef, Puppet and VirtualBox Guest Additions and are for development on your local machine when using Vagrant.
 * 'Bare OS' baseboxes contain nothing but the bare operating system and can be used for bootstrapping with `knife bootrap`

The two kinds of baseboxes are built from the same Veewee definition. This is achieved by templating the `definitions/<my-def>/postinstall.sh.erb` file and conditionally including either `common/custom-install-commands-bare-os.sh.erb` or `custom-install-commands-vagrant.sh.erb`.

See also:
 * https://github.com/jedi4ever/veewee
 * http://vagrantup.com/docs/base_boxes.html

# Usage

Install dependencies using bundler, then build all definitions with the `:build` task (this might take a while):

```
bundle install
rake build
```
 
# Adding Veewee Definitions

Take a look at the pre-defined templates that come with Veewee:

```
veewee vbox templates
```

Pick one of the pre-defined templates and add it to the definitions directory, e.g.:

```
veewee vbox define 'my-ubuntu-12.04' 'ubuntu-12.04-server-amd64'
```

Then rename `definitions/my-ubuntu-12.04/postinstall.sh` to `postinstall.sh.erb` and replace the code which installs gems, ruby, chef and puppet with this:

```
# custom install commands for either bare-os or vagrant basebox installation
# --- begin custom install commands

<%= custom_install_commands %>	

# --- end custom install commands
``` 

When you `rake build` now it will build two baseboxes: `my-ubuntu-12.04-bare-os.box` and `my-ubuntu-12.04-vagrant.box`



