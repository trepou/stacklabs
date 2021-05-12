# StackLabs

* Get Bill's public ssh key file, store it somewhere on ansible server and adapt ```public_key_path``` variable in the inventory accordingly: 
```
dev_user:
    username: bill
--> public_key_path: "../bill_id_rsa.pub" <--
    default_shell: zsh
```
* Adapt inventory with of Bill's environment hostname. Your ansible server should have root access this host (with all sudo privileges) and python should be installed on the host.


* Download required roles from ansible galaxy:

``` ansible-galaxy install -r requirements.yml```

* Run the playbook: 

```ansible-playbook ansible/playbook.yml -i ansible/inventory.yml```

## If Bill wants another tool

1. add the ```<new_tool>``` to ```requested_tools_as_root``` (or ```requested_tools_as_user``` if it should be installed with Bill's user) list in the inventory.

2. add the ```<new_tool>``` to ```installable_tools``` list in ```vars/installable_tools.yml``` 
    ```
      <new_tool>:
        install_by_role: yes # if it's just a package installation, do not set this line (or set it to false)
        install_role: <role_name> # it can either be a ansible-galaxy role name or a custom role name
        command_to_check_tool_install: <command to check the installation> # this is optional, if not specified "<new_tool> --version" command will be used
    ```

<ins>Notes:</ins>
* if ```<new_tool>``` is just a package to install and its installation can be check with ```<new_tool> --version``` command, you don't even have to add it to ```installable_tools``` list. Just adding it to ```requested_tools_as_root``` is enough!
* if you define a ```<new_tool>``` installable with **ansible-galaxy role**, do not forget to add it to ```requirements.yml``` and download it on your ansible server
* if you define a ```<new_tool>``` installable with a **custom role**, name the role ```<new_tool>``` so you don't have to set ```install_role``` var.