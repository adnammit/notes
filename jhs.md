# JHS

### GOVERNANCE REQUEST SCRIPT:
```
DEV
$__maint__/plscript __maint__/client/jhs/process_flow_request.pls jhs 28.25 28.26 28.27 28.28 28.29 28.30 28.32 28.33 28.35 28.47 28.56 28.57 28.58 28.59 28.60 28.61

EVERYWHERE ELSE
__maint__/plscript __maint__/client/jhs/process_flow_request.pls jhs 28.25 28.26 28.27 28.28 28.29 28.30 28.32 28.33 28.35 28.39 28.37 28.38 28.41 28.40 28.36 28.42
```


### 481: ADD OPTION IN CHANGE LOG TO DISPLAY COMMENTS/NOTES
#### --- f1 / no roll

| CI | FILES | notes |
|:--:|:--|:--|
||common/forms/form_editor_change_log_editor | add option for "Comments" column |
||common/forms/Create_change_log | add handling for column |



### 420: CHANGE LOG CONFIGURATION
#### --- f1 / no roll
* figure out how to make change log reportable
* check configuration
	- currently there is no change log entry for:
		* active
		* info req
	- breakdown of assigned to for each state:
		* Submitted/Reverted/Under Review: 		Project Reviewer skill
		* Not Approved: 						Project Reviewer skill
        * Data Gov Review:                      Data Gov Reviewer skill
        * Rev Cycle Review:                     Rev Cycle Reviewer skill
        * ROI Rev, Not Approved, Resubmitted:   Roi Reviewer (prop)
        * Governance Review:                    Governance Reviewers (prop)
        * Procurement Complete:                 Procurement skill
		* Approved:								Project Reviewer skill
		* Approved JDI:							Procurement skill
		* Active: 								N/A
        * Information Requested:                N/A
        * On Hold:                              Project Reviewer skill


* TO DO:
    - governance request "assigned to" doesn't work -- guessing its the priority of funcs
    - check ROI not approved and resubmitted
    - megan says: "ROI not approved was incorrect (listed as ROI reviewer but should be submitter since the submitter is responsible for resubmitting the ROI)"
    - make "Assigned To" and "Elapsed Days" (I think) reportable


* TESTING:
    - noticed that there is no Revenue Cycle Reviewer on live

| CI | FILES | notes |
|:--:|:--|:--|
||maint/client/jhs/gov_reqs/Gov_review_state | tweak change_log params |
||process_flow/macro_replace_util | accommodate user synths |
||common/forms/create_change_log|make "comments" optional, add "N/A" to elapsed days|
||common/forms/form_editor_change_log_editor|make "comments" optional, add "N/A" to elapsed days|
||common/forms/form_editor_field_types|add `make_reportable_column` and `set_reportable_col_props` functions to field specs|
||proc/gather_custom_form_properties|add support for `make_reportable_column` and `set_reportable_col_props`|



### 468: ADD COLUMN FOR DAYS BETWEEN ACTIONS LISTED
#### +++ f2 >> roll >> master

* adding column as an option in custom form editor
* calculate duration in total days, not work days
* center justify col?


| CI | FILES | notes |
|:--:|:--|:--|
|x|common/forms/form_editor_change_log_editor | add option for "Elapsed Time" column |
|x|common/forms/Create_change_log | add handling for time column |



### 477: CHANGES TO OPP REQUEST OVERWRITTEN IN LAST RELEASE
#### +++ f2 >> master

* i think the issue was that i did not push my changes up after the hotfix
* hotfixed to stage and test, pushed changes on dev up to master as well
* __other issue:__ emails not sent out when submitting though the tool -- debugging in f2

| CI | FILES | notes |
|:--:|:--|:--|
|x|proc/Add_request | fix whitespace |
|x|jhs/request_ok_action | add emails |
|x|common/requests/Request_dialog | whitespace fix, and also why aren't we setting the obj of `ok_action`? |



### 460: UPDATE FIELDS ON HARDCODED OPPORTUNITY REQUEST FORM
* there is only num_type for these old-school forms, no dollar type.
* ugh, webform -- why you no show???

| CI | FILES | notes |
|:--:|:--|:--|
|x|web/forms/jhs/opportunity_request_fields | remove business impact, add hard and soft dollars |
|x|client/jhs/opportunity_request_fields|remove business impact, add hard and soft dollars |
|x|client/jhs/opportunity_request_view|ignore project lock for permissions check, try to check for printing but it no work |
|x|common/layout/make_dollar_field|vtm field doesn't have a money filter when it's on the web -- why?|
|no|web/forms/make_dollar_field.dat|new dollar field |
|no|web/forms/html.dat|add dollar field |
|no|web/forms/make_fields.dat|add dollar field |
|no|web/forms/jhs/opportunity_request|debugging|
