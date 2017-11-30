# PROCESS FLOW

### 107: USER CAN LAUNCH PROJECT FROM REQUEST USER CAN'T OPEN
* megan says: any user with write access to the request folder can launch a project from a request
    * shouldn't they be restricted by permission to that state?
    * if there is no Active state defined, there are probably not any permissions on them, so anyone can re-launch
* objectives:
    * only allow users to launch requests in folders they have write access to
    * figure out a way to give user (at least) read access the project
* check this for a client who has permissions but doesn't have a defined Active state (Wellmont)

| CI | FILES | notes |
|:--:|:--|:--|
||process_flow/On_proj_launch_func | fix typo, add perms for curr user to the new project |
||common/requests/Request_customize_launch  | source perms differently, fix perms check    |
||wellmont/Request_customize_launch  |   ""      |
||visn9/Request_customize_launch  |   ""      |
||maint/client/jhs/gov_reqs/Active_state|I called this "Approved", not "Active" -__-|
|n/a|common/team/project_permissions|this is how you do an add permissions thing|

** RELEASE: run jhs script for Active state
