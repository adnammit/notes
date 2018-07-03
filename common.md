# COMMON

### 732: UPDATING FILTER ON REQUEST LIST CLEARNS REQUESTS FROM REPORT
#### >> f2

* change tabs to use common request tab instead of wellspan's custom one
* this eliminates references to "Request Views" in the side bar but it doesn't appear they're using them
    - wellspan has custom files to manage `find_context_value("request_views")`
        * Generate_request_tree and Edit_request_views
    - `request_views` are saved in client data in subfolders of the same name -- I checked and didn't see any there
    - common loads reports from `find_context_value("request_list_views")`

looking at DB_tabs.dat:

### uses custom Project_requests_tab.dat:
ca_dsrip (scvmc)
ny_dsrip (scvmc)
riverside
scvmc
visn22
visn4

#### uses common Project_requests_tab
abington
barrett
cadence
cfhp
fintrack
forrestgeneral
healthquest
lifebridge
mercy
om
oums
rcrmc
sfgh
sickkids
skf
ski/ski2
supplychain
transformation
vcu
common (db)
dsrip


#### uses Requests_tab_new
ashland
jhs
parkland/it_pmo
solutions/ppm_lite
teletracking
uhs/it_pmo
visn10
visn9
wellmont
wellspan (as of this issue's changes - used to use custom)
eoc
ppm_lite
rounds



| FILES | notes |
|:--|:--|
|wellspan/DB_tabs|don't use wellspan/Project_requests_tab -- use common Request_tab_new instead|
