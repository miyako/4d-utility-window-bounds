# 4d-utility-window-bounds
JSON window bounds manager that is multiple screen aware

### About

4D has a built-in system to restore the window size and position from when it was closed, but it has issues with changes in the number of connected screens or their resolution. this is an attempt to correct that.

c.f. `Open form window`, 4D Window Bounds

The component exports a singleton pseudo-class (shared project method) with 2 member methods.

* Bounds.set(*identifier*)
* Bounds.get(*identifier*)

*identifier* for table form: "[tableName]formName"  
*identifier* for project form: "formName"

Window bounds are stored in 

* {fk user preferences folder}/*hostProjectName*/Bounds/[*tableName*]*formName*.json
* {fk user preferences folder}/*hostProjectName*/Bounds/[{projectForm}]*formName*.json

### Examples

To save the window bounds

```4d
If (Form event code=On Unload)
	
	$Bounds:=Bounds
	
	$Bounds.set("TEST")
	
End if 
```

To restore window bounds

```4d
$Bounds:=Bounds

$frames:=$Bounds.get("TEST")

/*
	
	frames (object)
	
	.x:       x pos adjusted for the current menu bar screen (in case the screen width has shrinked)
	.y:       y pos adjusted for the current menu bar screen (in case the screen height has shrinked)
	.left:    x pos on the screen when the window was closed (ratio to screen width)
	.top:     y pos on the screen when the window was closed (ratio to screen height)
	.width:   form width when the window was closed
	.height:  form height when the window was closed
	.screen:  screen where the window was closed
	
*/

If ($frames.x=0) & ($frames.y=0)
	$window:=Open form window("TEST"; Plain form window)
Else 
	$window:=Open form window("TEST"; Plain form window; $frames.x; $frames.y)
End if 

DIALOG("TEST")
CLOSE WINDOW($window)
```

Normally you just need the *x* and *y* coordinates. *left* and *top* are used internally. *screen* is for information.

The *set()* method calculates the *top* and *left* ratio for the current form for the current screen.

The *get()* method calibrates the *x* and *y* for the current screen setup. if the stored *x* and *y* coordinates exist in the current screen setup, it is returned "as is". otherwise, *left* and *top* are converted to *x* and *y* for the main screen.
