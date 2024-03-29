Role Name
=========

It's a common role for installation ORACLE GRID INfrastructure.

Setup variables: 
 - `grid_home`
 - `nodes`
 - `vote_disk`

Role Variables
--------------
- `stage_dir`  - default dir to download installation source and logs., default /u01/app/stage
- `ora_archive_file_url` - installation source
- `grid_version` - GRID_INSTALLATION version, default '12.2'
- `oracle_password` - password for oracle user, default "0ra6oo"
- `scan_listener`
- `cluster_name`
- `sysasmpassword` -  
- `asmnmppassword` -  
- `nodes` - Node for isntallation GRID, if using DNS ip_addr may be a node name.
```yaml
nodes:
   - {name: 'node1',vip_name: 'node1-vip', ip_addr: '192.168.10.212'}
   - {name: 'node2',vip_name: 'node2-vip',ip_addr: '192.168.10.213'}
```
- `vote_disk` - disks for OCR AND VOTING FILES
```yaml
vote_disk:
  - {disk: '/dev/sdd',label: 'VOTE1'}
  - {disk: '/dev/sdc' ,label: 'VOTE2'}
  - {disk: '/dev/sde', label: 'VOTE3'}
```  
- `public_iface` - Inerface for public network
- `private_iface` - Inerface for private network

Example Playbook
----------------
Install using ansible galaxy : ansible-galaxy install -f git+https://....../grid_common.git


    - hosts: servers
      roles:
        - role: grid_common
          ora_archive_file_url : 'grid.zip'
          cluster_nodes:
             - {name: "node1",vip_name: "node1-vip", ip_addr: "192.168.1.1"}
            -  {name: "node2",vip_name: "node2_vip", ip_addr: "192.168.1.2"}
          voting_disks:
            - {disk: '/dev/sdb',label: 'VOTE1'}
            - {disk: '/dev/sdc' ,label: 'VOTE2'}
            - {disk: '/dev/sdd', label: 'VOTE3'}    
          scan_listener: 'rac1-scan'
          cluster_name: 'cluster-rac1'  
          public_iface: 'ens192' 
          private_iface: 'ens224'
        - role: grid_install    
          ora_archive_file_url:  "..."
        - role: install_patch
          type_install_patch: 'gi_install'
          opatch_update: true
          home:
            type_home: 'GI'
            oracle_version: '12.2'
          patch:
            archive_url: "....."
            temp_dir: '/tmp'
            patch_id: 31305382
            patch_number: [31312468,31309299,31245159,26839277,30888810]
    - role: grid_configure  
