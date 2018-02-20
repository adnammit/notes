
# TO DO

#### F1 >> clean

#### F2 >> clean

#### F3 >> TESTING
\>> PF 79: WEB FORM WIDTHS, ASHLAND FORMS 49-50

#### F4 >> clean

#### opp-req [data for jhs, epsg, baybluffs, wellmont]
\>> JHS 464: CONVERT OPPORTUNITY REQUEST TO PROCESS FLOW

#### visn-perms >> that visn transfer thing -- does megan want this?


### NEW STUFF:
```
testwc: display everything i did for test
diws: diff ignoring whitespace, newlines and comments
giff: git diff ignoring whitespace, newlines and comments
giffns: git diff ignoring whitespace, newlines and comments; show filenames only
linkorg [org]: alias for copy_org, links org's data to alr repo site
find_pf_clients: list all process flow clients
sync_pf_clients: copy the data for all pf clients
sync_client_data: copy data for multiple clients
    $ sync_client_data jhs epsg wellmont --link alr
update_dev: update and roll master and the curr feature
diff_dirs: takes 2 dirs as args, diff all files between them
rcp: roll_changed_pkgs
grst: grep in files named *state.dat
```

### BASH STUFF
* add a script to backup changes to ~/backup or dev-lnx (like denny's good_morning)
* get git completion to work
* make the git equivalent of `sall` (grep for multiple expressions)
* do automated update/rebase/roll_out more better -- adapt alex's update script to include master?
    - actually i think you'll always want to use alias roll_changed b/c that will roll stuff you didn't change -- stuff you're deving you should be manually rolling


### TO DO:
* kill only kills the current line, not wrapped text
* find and replace sucks
* autocomplete sucks
* use swirly font for comments
* pirtle says: use apm to install packages through DOS (command prompt) rather than cygwin
* is there a right-click "buffer manager" like with emacs?


### GIT
* what is all this `revs` shit?
* what is up with all this git diff shit -- are there helpful aliases?
* what is up with all this git log shit -- are there helpful aliases?


### RELEASE 2/15
* run scripts for issue #42 (added validate params to "Submitted" states)
* baybluffs and epsg didn't get hit last week:
    - epsg
    - baybluffs


### RELEASE 2/8
* common
* process_flow
* web/maint

* run scripts for issue #42 (added validate params to "Submitted" states) and wellmont 184
    - jhs
    - wellmont























No changes to stash for branch 'test'
Switched to branch 'master'
Your branch is behind 'origin/master' by 6 commits, and can be fast-forwarded.
  (use "git pull" to update your local branch)
No stash found for branch 'master'
remote: Counting objects: 215, done.
remote: Compressing objects: 100% (211/211), done.
remote: Total 215 (delta 171), reused 0 (delta 0)
Receiving objects: 100% (215/215), 77.41 KiB | 9.68 MiB/s, done.
Resolving deltas: 100% (171/171), completed with 94 local objects.
From /src/git/main
   ea65e0b090..514994e5cb  master                -> origin/master
 + b3e049e101...fa4efd184a stage                 -> origin/stage  (forced update)
 * [new branch]            stage_20180125_160547 -> origin/stage_20180125_160547
Updating 2ac8543f79..514994e5cb
Fast-forward
 build/pl_cab.id                                                   |   27 -
 build/roll_out.make                                               |  260 +-
 dat/audit/checklists/Remove_checklist_func.dat                    |   60 +-
 dat/audit/util/Checklist_util_funcs.dat                           |    2 +
 dat/client/bswqa/EPMO_project_columns.dat                         |  538 ---
 dat/client/bswqa/Request_list_customize_view_func.dat             |  933 ------
 dat/client/cadence/EPMO_project_columns.dat                       |  526 ---
 dat/client/cfhp/EPMO_project_columns.dat                          |  631 ----
 dat/client/childrens2/EPMO_customize_items.dat                    |  843 -----
 dat/client/childrens2/EPMO_project_columns.dat                    | 1342 --------
 dat/client/covenanthealth/ppm_lite/EPMO_project_columns.dat       | 1409 ++++++++
 .../strategic_plan/milestone_plan}/EPMO_project_columns.dat       |  460 ++-
 .../strategic_plan/milestone_plan/EPMO_project_report.dat         |   20 +-
 .../strategic_plan/milestone_plan/Folder_view_types.dat           |   18 +
 dat/client/demo2/it_pmo/Folder_view_types.dat                     |    2 +-
 dat/client/ejgh/EPMO_customize_items.dat                          | 1459 ---------
 dat/client/ejgh/EPMO_project_columns.dat                          | 1265 -------
 dat/client/jhs/DB_tabs.dat                                        |   24 +
 dat/client/jhs/Get_epmo_columns.dat                               |   56 -
 dat/client/jhs/PM_tabs.dat                                        |   24 +
 dat/client/jhs/Select_template_step.dat                           |  252 +-
 dat/client/natividad/EPMO_customize_items.dat                     | 1007 ------
 dat/client/natividad/EPMO_project_columns.dat                     |  946 ------
 dat/client/natividad/Folder_view_types.dat                        |    2 +-
 dat/client/parkland/it_pmo/EPMO_customize_items.dat               | 3127 ------------------
 dat/client/parkland/it_pmo/EPMO_list_layout.dat                   |  174 -
 dat/client/parkland/it_pmo/EPMO_project_columns.dat               | 1561 ---------
 dat/client/parkland/it_pmo/EPMO_project_report.dat                |  914 ------
 dat/client/parkland/it_pmo/Folder_view_types.dat                  |    2 +-
 dat/client/parkland/it_pmo/Get_epmo_columns.dat                   |   25 -
 dat/client/parkland/it_pmo/Print_project_views.dat                |    4 +-
 dat/client/riverside/EPMO_project_columns.dat                     |  624 ----
 dat/client/sickkids/EPMO_project_columns.dat                      |  398 ---
 dat/client/teletracking/EPMO_project_columns.dat                  |  638 ----
 dat/client/thr/EPMO_customize_items.dat                           | 2994 -----------------
 dat/client/thr/EPMO_customize_view_func.dat                       |  430 ---
 dat/client/thr/EPMO_project_columns.dat                           | 1265 -------
 dat/client/vcu/EPMO_customize_items.dat                           | 5072 -----------------------------
 dat/client/vcu/EPMO_project_columns.dat                           | 2257 -------------
 dat/client/vcu/EPMO_project_report.dat                            |  895 -----
 dat/client/vcu/Folder_view_types.dat                              |    2 +-
 dat/client/vcu/Get_epmo_columns.dat                               |   28 -
 dat/client/visn9/EPMO_customize_items.dat                         |  561 ----
 dat/client/visn9/EPMO_project_columns.dat                         |  698 ----
 dat/client/visn9/Folder_view_types.dat                            |    2 +-
 dat/client/visn9/Request_customize_launch.dat                     |  114 +-
 dat/client/wellmont/Folder_view_types.dat                         |  276 ++
 dat/common/forms/Form_editor_field_types.dat                      |   10 +-
 dat/common/layout/Grouping_dialog.dat                             |  169 +-
 dat/common/layout/Make_attachment_table.dat                       |  219 +-
 dat/common/library/html_funcs/Html_util_attachments_funcs.dat     |   48 +-
 .../reports/EPMO_actions_dialog.dat}                              |   28 +-
 dat/common/reports/EPMO_customize_items.dat                       |  147 +-
 dat/common/reports/EPMO_get_actions.dat                           |   31 +
 dat/common/reports/EPMO_project_columns.dat                       |  347 +-
 dat/common/reports/EPMO_project_report.dat                        |  109 +-
 dat/common/reports/Get_epmo_columns.dat                           |   64 +-
 dat/common/reports/Make_project_measure_column.dat                |   80 +-
 dat/common/reports/Simple_projects_list.dat                       |   53 +-
 dat/common/requests/Request_customize_launch.dat                  |  112 +-
 dat/common/requests/Request_dialog.dat                            |  184 +-
 dat/common/tabs/Tab_list.dat                                      |   24 +
 dat/common/uber_table/Create_uber_table.dat                       |  466 +--
 dat/db/Add_folder_project_view.dat                                |  149 +-
 dat/eoc/Inspection_download_email_dialog.dat                      |   20 +-
 dat/org_settings.dat                                              |   17 +-
 dat/ppm_lite/EPMO_project_columns.dat                             |  904 +----
 dat/ppm_lite/Folder_view_types.dat                                |   60 +-
 dat/process_flow/On_proj_launch_func.dat                          |   89 +-
 dat/process_flow/Pdf_util.dat                                     |   80 +-
 dat/process_flow/Pre_post_func_check_exclude.dat                  |    8 +-
 dat/process_flow/Pre_post_funcs.dat                               |   29 +-
 dat/process_flow/Process_flow_util.dat                            |  183 +-
 dat/process_flow/Request_flow_util.dat                            |   41 +-
 .../process_flow_util/Test_add_process_flow_permission.dat        |  157 +
 .../unit_test/process_flow_util/Test_move_to_next_state.dat       |  339 ++
 dat/process_flow/unit_test/test.sh                                |   13 +-
 dat/strategic_plan/measure_plan/EPMO_action_dialog.dat            |  187 --
 dat/strategic_plan/measure_plan/EPMO_customize_items.dat          |  403 +--
 dat/strategic_plan/measure_plan/EPMO_customize_view_func.dat      |  475 ---
 dat/strategic_plan/measure_plan/EPMO_project_columns.dat          | 2165 +-----------
 dat/strategic_plan/measure_plan/EPMO_project_report.dat           |  851 -----
 dat/strategic_plan/measure_plan/Folder_view_types.dat             |    2 +-
 dat/strategic_plan/measure_plan/Get_epmo_columns.dat              |   28 -
 dat/strategic_plan/milestone_plan/Create_epmo_custom_top.dat      |  178 +
 dat/strategic_plan/milestone_plan/EPMO_project_columns.dat        | 1405 +-------
 dat/strategic_plan/milestone_plan/Folder_view_types.dat           |   10 +-
 property/Sort_criteria.cpp                                        |   29 +-
 web/__app__/proc/Remove_local_subobject.dat                       |  201 +-
 web/__app__/proc/eoc/Inspection_download_email.dat                |   26 +-
 web/__app__/proc/ppm_lite/Gather_epmo_data.dat                    |   15 -
 web/__app__/proc/strategicplan/measure_plan/Gather_epmo_data.dat  |   83 +-
 .../proc/strategicplan/milestone_plan/Gather_epmo_data.dat        |   45 +-
 web/__app__/proc/unit_test/test.sh                                |    2 +-
 web/__app__/proc/unit_test/test_local_subobject_scripts.dat       |  686 ++++
 web/__lib__/Populate_epmo_object.dat                              |   45 +-
 web/__maint__/client/baybluffs/suggestions/Submitted_state.dat    |   18 +-
 web/__maint__/client/epsg/process_flow_request.pls                |   15 +-
 web/__maint__/client/epsg/request_states/Submitted_state.dat      |   81 +-
 .../client/jhs/governance_requests/Approved_as_jdi_state.dat      |   83 +-
 .../client/jhs/governance_requests/Governance_review_state.dat    |   11 +-
 web/__maint__/client/jhs/governance_requests/Submitted_state.dat  |   43 +-
 web/__maint__/client/jhs/process_flow_request.pls                 |   62 +-
 web/__maint__/client/wellmont/pi_requests/Submitted_state.dat     |   12 +-
 web/__maint__/util/eoc/Find_renamed_deleted_bldgs_depts.pls       |  281 ++
 web/__web__/forms/custom/Insert_reviewer_fld.dat                  |   51 -
 web/__web__/forms/custom/request_new.dat                          |   48 +-
 web/__web__/forms/custom/request_scoring.pg                       |   90 +-
 web/__web__/forms/custom/request_scoring_submit.pg                |   36 +-
 web/__web__/forms/custom/validate_request_data.xml.p              |  176 +-
 110 files changed, 6526 insertions(+), 39664 deletions(-)
 delete mode 100755 build/pl_cab.id
 delete mode 100644 dat/client/bswqa/Request_list_customize_view_func.dat
 delete mode 100644 dat/client/cadence/EPMO_project_columns.dat
 delete mode 100644 dat/client/childrens2/EPMO_customize_items.dat
 delete mode 100644 dat/client/childrens2/EPMO_project_columns.dat
 create mode 100644 dat/client/covenanthealth/ppm_lite/EPMO_project_columns.dat
 rename dat/client/{pinnacle => covenanthealth/strategic_plan/milestone_plan}/EPMO_project_columns.dat (72%)
 rename dat/{ => client/covenanthealth}/strategic_plan/milestone_plan/EPMO_project_report.dat (97%)
 create mode 100644 dat/client/covenanthealth/strategic_plan/milestone_plan/Folder_view_types.dat
 delete mode 100644 dat/client/ejgh/EPMO_customize_items.dat
 delete mode 100644 dat/client/ejgh/EPMO_project_columns.dat
 delete mode 100644 dat/client/jhs/Get_epmo_columns.dat
 delete mode 100644 dat/client/natividad/EPMO_customize_items.dat
 delete mode 100644 dat/client/natividad/EPMO_project_columns.dat
 delete mode 100644 dat/client/parkland/it_pmo/EPMO_customize_items.dat
 delete mode 100644 dat/client/parkland/it_pmo/EPMO_list_layout.dat
 delete mode 100644 dat/client/parkland/it_pmo/EPMO_project_columns.dat
 delete mode 100644 dat/client/parkland/it_pmo/EPMO_project_report.dat
 delete mode 100644 dat/client/parkland/it_pmo/Get_epmo_columns.dat
 delete mode 100644 dat/client/teletracking/EPMO_project_columns.dat
 delete mode 100644 dat/client/thr/EPMO_customize_items.dat
 delete mode 100644 dat/client/thr/EPMO_customize_view_func.dat
 delete mode 100644 dat/client/thr/EPMO_project_columns.dat
 delete mode 100644 dat/client/vcu/EPMO_customize_items.dat
 delete mode 100644 dat/client/vcu/EPMO_project_columns.dat
 delete mode 100644 dat/client/vcu/EPMO_project_report.dat
 delete mode 100644 dat/client/vcu/Get_epmo_columns.dat
 delete mode 100644 dat/client/visn9/EPMO_customize_items.dat
 delete mode 100644 dat/client/visn9/EPMO_project_columns.dat
 create mode 100644 dat/client/wellmont/Folder_view_types.dat
 rename dat/{strategic_plan/milestone_plan/EPMO_action_dialog.dat => common/reports/EPMO_actions_dialog.dat} (90%)
 create mode 100644 dat/common/reports/EPMO_get_actions.dat
 create mode 100644 dat/process_flow/unit_test/process_flow_util/Test_add_process_flow_permission.dat
 create mode 100644 dat/process_flow/unit_test/process_flow_util/Test_move_to_next_state.dat
 delete mode 100644 dat/strategic_plan/measure_plan/EPMO_action_dialog.dat
 delete mode 100644 dat/strategic_plan/measure_plan/EPMO_customize_view_func.dat
 delete mode 100644 dat/strategic_plan/measure_plan/EPMO_project_report.dat
 delete mode 100644 dat/strategic_plan/measure_plan/Get_epmo_columns.dat
 create mode 100644 dat/strategic_plan/milestone_plan/Create_epmo_custom_top.dat
 create mode 100644 web/__app__/proc/unit_test/test_local_subobject_scripts.dat
 create mode 100644 web/__maint__/util/eoc/Find_renamed_deleted_bldgs_depts.pls
 delete mode 100644 web/__web__/forms/custom/Insert_reviewer_fld.dat
fatal: ambiguous argument 'test': unknown revision or path not in the working tree.
Use '--' to separate paths from revisions, like this:
'git <command> [<revision>...] -- [<file>...]'
fatal: Not a valid object name test
fatal: ambiguous argument 'origin..test': unknown revision or path not in the working tree.
Use '--' to separate paths from revisions, like this:
'git <command> [<revision>...] -- [<file>...]'
Total 0 (delta 0), reused 0 (delta 0)
To /src/git/main.git
 * [new branch]            test_20180130_170036 -> test_20180130_170036
Branch test_20180130_170036 set up to track remote branch test_20180130_170036 from origin.
To /src/git/main.git
 - [deleted]               test
No changes to stash for branch 'master'
Switched to a new branch 'test'
No stash found for branch 'test'
Total 0 (delta 0), reused 0 (delta 0)
To /src/git/main.git
 * [new branch]            test -> test
Branch test set up to track remote branch test from origin.








ssh://dev-src.portland.perflogic.com:20022/src/git/main.git
