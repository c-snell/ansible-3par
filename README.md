# hpe3par-ansible

The hpe3par-ansible playbooks are a starter kit which provides simple Ansible playbooks and examples, which can be used to write custom playbooks to manage your HPE 3PAR storage arrays. These playbooks use Ansible ```uri``` module to communicate via RESTful calls to the HPE 3PAR WSAPI hosted on the 3PAR storage arrays.

## Requirements
 - Ansible >= 2.1
 - Python >= 2.7.9
 - HPE 3PAR WSAPI enabled
 
## Playbooks
Each playbook corresponds to a storage provisioning operation and talks natively with the HPE 3PAR WSAPI using RESTful calls.

## Variables
Each playbook contains variables that can be static `var` or dynamically `var_prompt` configured. Currently the playbooks take the IP, username and password at runtime but depending on your use case, this can be modified easily by adding variables under the corresponding variable sections. 

>**Note**, *spaces matter in `yaml`!* **Don't use tabs.**

Using your favorite text editor, edit the [/playbooks/create_3par_volume.yml](playbooks/virtual_volume/create_3par_volume.yml) playbook

Look for the vars and vars_prompt.

```yml
#Define static variables here
vars:  #
    rest_api_url_3par: "https://{{ ip_address_3par }}:8080/api/v1" 
#   auth_3par_user: "3paruser"
#   ip_address_3par: "192.168.1.150"

#Run-time variables defined here - Note the 
vars_prompt:
    - name: "auth_3par_user"            #variable name
      prompt: "Enter 3par admin user"   #prompt text
      private: no                       #input visible at runtime
      default: "3paruser"               #include if you want default option defined

    - name: "auth_3par_password"
      prompt: "Enter password"
      private: yes                      #password entry obfuscated at runtime
      default: "3parpass"

    - name: "ip_address_3par"
      prompt: "3PAR Storage System IP address"
      private: no
      default: "192.168.1.150"

    - name: "vol_name"
      prompt: "Enter new volume name"  
      private: no

    - name: "cpg_name"
      prompt: "Enter CPG name"
      private: no
      default: "FC_r6"

    - name: "size_GiB"
      prompt: "Enter Size in GiB"
      private: no
 
 
 ```

### Example how to create a 3PAR Volume using the [/playbooks/create_3par_volume.yml](playbooks/virtual_volume/create_3par_volume.yml) playbook

```bash

ansible@ansible-server:~/playbooks/virtual_volume$ ansible-playbook create_3par_volume.yml
```
Follow the prompts
```bash

Enter 3par admin user: 3paruser
Enter password: 3parpass
3PAR Storage System IP address: 192.168.1.150
Enter new volume name: my_first_ansible_vol
Enter CPG name [FC_r6]:
Enter Size in GiB: 10

PLAY [New 3par Volume] ******************************************************************************

TASK [Gathering Facts] ******************************************************************************
ok: [localhost]

TASK [check if 3par WSAPI is running] ***************************************************************
ok: [localhost]

TASK [Parsing key] **********************************************************************************
ok: [localhost] => {
    "msg": "0-813c20ffd9bc7a4c4af0eec0e3afbdef-b8ed745a"
}

TASK [Create 3par volume] ***************************************************************************
ok: [localhost]

TASK [Parsing Volumes GET] **************************************************************************
ok: [localhost] => {
    "msg": {
        "cache_control": "no-cache",
        "changed": false,
        "connection": "close",
        "content": "",
        "cookies": {},
        "date": "Fri, 02 Feb 2018 23:01:13 GMT",
        "failed": false,
        "location": "https://192.168.1.150:8080/api/v1/volumes/my_first_ansible_vol",
        "msg": "OK (unknown bytes)",
        "pragma": "no-cache",
        "redirected": false,
        "server": "hp3par-wsapi",
        "status": 201,
        "url": "https://192.168.1.150:8080/api/v1/volumes"
    }
}

PLAY RECAP ******************************************************************************************
localhost                  : ok=5    changed=0    unreachable=0    failed=0
```


Once complete, you have successfully created a volume.
