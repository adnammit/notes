# PPM_LITE

### 423: RESOLVE DATE INCONSISTENCY IN EMAIL LOGS PANE
#### --- f3 / no roll
#### --- RUN: `__maint__/plscript __maint__/util/repair_email_logs.pls jhs -d`

* the date that is attached to a log item is in the user's time zone, but it should be in the time zone of the org for consistency.
* **problem:** when sending emails from the email status pane, emails show up initially but then disappear
    - it's like they're getting added to the local cache but aren't getting saved on the server
    - I think that recent emails are being stored via the new system but the status pane is looking for emails in the wrong/old place -- NOPE, they are getting save after all
    - in web/lib/Email_obj_util, the call to load the emails in `load_email_object` isn't loading the last few months b/c segment names are not being updated past august.
* Solution:
    - turns out that seg names weren't getting saved in the email_obj, and so the segments (which do exist) aren't getting loaded
    - fixed the email_obj_util so that whenever a segment is added, the obj is saved
    - however this doesn't fix past months. if a client is using the flag and has up through dec, it's fine
    - find all orgs using the flag and get them updated code asap:
        * jhs: ok up through dec

* Testing:
    - tool-side send_email and form/send_email look good but you still need to check `__lib__/send_email`

| CI | FILES | notes |
|:--:|:--|:--|
||common/library/send_email | this is where the timestamp is made -- convert here |
||web/lib/send_email | this is where the timestamp is made -- convert here |
||web/forms/send_email | this is where the timestamp is made -- convert here |
||common/status/email_log_pane | calls the old stuff somehow. clean up this nasty code too | <--- nah, you don't need to change anything
||web/forms/jhs/submit_opportunity_requests | send_email isnt' getting passed a param, so it's not logging |
|NEW|web/maint/util/repair_email_logs.pls|script to add names of missing segments back in|
||web/lib/object/email_obj_util|fix add_segment_name method to save when a new segment is added|
|n/a|web/cron/client/perflogic/send_release_request_reminders.pls|no changes -- just testing|
