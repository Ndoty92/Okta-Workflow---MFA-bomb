# Okta-Workflow---MFA-bomb
A way to detect MFA bombing with Otka workflows



Pre-reqs/additional requirements
 - event hook in Okta on "Authentication of user via MFA" (create and save the workflow to get the proper invoke URL for the event hook)

 - user must take action on MFA event. Ignoring the MFA will not trigger the flow

Purpose
 - Evaluate every MFA event (success or failure) for excessive push notifications. 

Filters
 - Filters out Authentication events of NONCE (Fastpass) and password

Actions
 - When qualified MFA event occurs, workflow queryies the system log for push events with the targetID of the user triggering the event, and a predefined previous time span (i.e. last 30 minutes)
 - it counts the number of events.  If the event count exceeds the threshold (i.e. 5) it triggers the following
	- clear user sessions (stops the suspected attack at the Okta level - does not log out of any apps associated with the event)
      - puts the user in a predefined group

Okta can then be configured to block logins for that group, the SIEM tool can be configured to look for user adds to that group and trigger appropriate responses. 
