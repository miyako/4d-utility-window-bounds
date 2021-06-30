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
