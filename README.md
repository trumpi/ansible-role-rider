Rider (https://www.jetbrains.com/rider)
=========

This role installs Rider and configured plugins. It has been tested on Linux Mint 18 but should work on most 
distributions. By default it installs Rider Early build RC-171.4456.1432 and no additional plugins

By default Rider is installed under the user's home directory and _become_ is not needed.

Requirements
------------

None


Role Variables
--------------

    rider_version: 2020.1.4
    rider_download_mirror: https://download.jetbrains.com/resharper/
    rider_plugin_download_mirror: "https://plugins.jetbrains.com/plugin/download?updateId="
    rider_plugins: []
    rider_download_directory: /tmp
    rider_user_dir: "~{{ (rider_install_user is defined) | ternary(rider_install_user, ansible_user_id) }}"
    rider_install_directory: "{{ rider_user_dir | expanduser  }}/Tools"

    # calculated
    rider_install_file: "Rider-{{ rider_version }}.tar.gz"
    rider_download_url: "{{ rider_download_mirror }}{{ rider_install_file }}"
    rider_location: "{{ rider_install_directory }}/rider-{{ rider_version }}"
    rider_desktop_file_location: "{{ rider_user_dir | expanduser }}/.local/share/applications/rider-{{ rider_version }}.desktop"

* rider_plugins is a list of names which get appended to rider_plugin_download_mirror to form a full download
* Defining rider_install_user allows the role to install under a different user, however become is required 


Dependencies
------------

None

Example 
-------

__Example playbook__


    - hosts: localhost
      connection: local
    
    roles:
      - henriklyngaard.rider
      
__Exmaple inventory for plugins__

The below IDs have been found by going to https://plugins.jetbrains.com/gogland and searching for the plugin. 
Once found copy the link location for the desired version and use the _updateId=XXXXX_ part at the end        
      
    rider_plugins:
      # ignore 1.7.6
      - 32828
      # bash support 1.6.5.171
      - 31610
      # ansible 0.9.4
      - 27616
      # docker 2.5.3
      - 33621
      # markdown 2017.1.20170302
      - 33092      
      
 Alternatively upload the required plugins to a webserver and adjust _rider_plugin_download_mirror_ and 
 _rider_plugins_ accordingly
      
      
License
-------

MIT

Change log
----------

* 1.0: Initial version
