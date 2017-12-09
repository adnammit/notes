# JHS

### GOVERNANCE REQUEST SCRIPT:
```
DEV
$__maint__/plscript __maint__/client/jhs/process_flow_request.pls jhs 28.25 28.26 28.27 28.28 28.29 28.30 28.32 28.33 28.35 28.47 28.56 28.57 28.58 28.59 28.60 28.61

EVERYWHERE ELSE
__maint__/plscript __maint__/client/jhs/process_flow_request.pls jhs 28.25 28.26 28.27 28.28 28.29 28.30 28.32 28.33 28.35 28.39 28.37 28.38 28.41 28.40 28.36 28.42
```


### 420: CHANGE LOG CONFIGURATION
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

* TESTING:
    - noticed that there is no Revenue Cycle Reviewer on live

| CI | FILES | notes |
|:--:|:--|:--|
||maint/client/jhs/gov_reqs/Gov_review_state | tweak change_log params |
||process_flow/macro_replace_util | accommodate user synths |



### 460: UPDATE FIELDS ON HARDCODED OPPORTUNITY REQUEST FORM
* there is only num_type for these old-school forms, no dollar type.
* ugh, webform -- why you no show???

| CI | FILES | notes |
|:--:|:--|:--|
||web/forms/jhs/opportunity_request_fields | remove business impact, add hard and soft dollars |
||client/jhs/opportunity_request_fields|remove business impact, add hard and soft dollars |
||client/jhs/opportunity_request_view|ignore project lock for permissions check, try to check for printing but it no work |
||common/layout/make_dollar_field|vtm field doesn't have a money filter when it's on the web -- why?|
|no|web/forms/make_dollar_field.dat|new dollar field |
|no|web/forms/html.dat|add dollar field |
|no|web/forms/make_fields.dat|add dollar field |
|no|web/forms/jhs/opportunity_request|debugging|
