# IVR-NumberChange
IVR to change extension number on the fly on a FreePBX server

# Presumptions
- The new extension numbers start 4295xxxx
- VG224 analogue voice gateway has default extensions starting 422xx
- You have custom audio recordings, enter-new-number, enter-new-number, number-must-start-with-429

# FreePBX Settings
set "NoOp Traces in Dialplan" to 1 in advanced settings

Create Custom destintation for the number change context
Target: ivr-changenumber,s,1

Create Custom trunk called "NumChange"
Dial String local/s@macro-hangupcall to catch any calls that might use it by mistake

Create an outbound route matching a dial pattern for your new new extension numbers, ime using 4295.
Make sure the route type is intra-Company
Trunk Sequence will be the custom trunk

# How it works

When the dialed number matches the outbound route the [macro-dialout-trunk-predial-hook] gets triggered
This then checks if the outbound route was "NumChangeRoute" if it was then the context [number-change] is called

To change a number, set an extension or ivr that dials the custom destination
This prompts the user to enter the new number, checkes if it starts 4295

In my setup im using a Cisco VG224 as well as sip phones, so i also check if the call came from the vg224 by checking the cid (set on the vg224)
