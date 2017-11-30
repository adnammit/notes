# JHS

```
DEV
$__maint__/plscript __maint__/client/jhs/process_flow_request.pls jhs 28.25 28.26 28.27 28.28 28.29 28.30 28.32 28.33 28.35 28.47 28.56 28.57 28.58 28.59 28.60 28.61

TEST/STAGE/LIVE
__maint__/plscript __maint__/client/jhs/process_flow_request.pls jhs 28.25 28.26 28.27 28.28 28.29 28.30 28.32 28.33 28.35 28.39 28.37 28.38 28.41 28.40 28.36 28.42
```

### 460: UPDATE FIELDS ON HARDCODED OPPORTUNITY REQUEST FORM
* there is only num_type for these old-school forms, no dollar type.
* ugh, webform -- why you no show???

| CI | FILES | notes |
|:--:|:--|:--|
||web/forms/jhs/opportunity_request_fields | remove business impact, add hard and soft dollars |
||web/forms.auth|whitelist any server with 'repo' in it |
||web/custom_org.dat|add repo to server/org parsing|
||web/forms/jhs/opportunity_request|debugging|


roll_out ppm_lite process_flow db pt pm pl_pm min_site common pl_plugin bpa site site_dat



### visn9 transfer thing

| CI | FILES | notes |
|:--:|:--|:--|
||clinical_cancellation_actions|add to Transfer exclude|
|n/a|request_dialog|check for write access -- write access granted|
