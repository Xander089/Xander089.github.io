---
layout: page
permalink: "/course/materials"
---

- content drawer create material
- naming convention for material is: MM_Name
- open and flag true the property Use Material Attributes
- add landscape layer blend and add N layers
- create a mat function and do stuff in it
- link the function to attributes and the latter to mat output
- right click on material, create instance MI_Instance
- shift 2 landscape and assign the instance kandscape material property to the landscape material instance
- in 5.6 click landscape paint layers + micro button create from material instance this will add the material layers
- create layer info, weight blended layer
- built albedo function
- duplicate function each layer
- the material instance will inherit all the material funcion parameters
- material functions: way to group functionalities in order o not make the graph too complicated
- create layer info for each layer in landscape


### Mouse Controls

|Control|Action|
|---|---|
|RMB drag on background|Pan Material expression graph|
|LMB drag on background|Box select|
|Rotate Mouse Wheel|Zoom in and out|
|LMB + RMB drag|Zoom in and out|
|LMB click on object|Select expression/comment|
|Ctrl + LMB on object|Toggle selection of expression/comment|
|LMB click + drag|Move current selection/comment|
|Ctrl + LMB drag|Box select (add to current selection)|
|LMB drag on connector|Create connection (release on connector)|
|Ctrl + LMB drag from connection|Move connection (release on same type connector)|
|Shift + single-click LMB on connector|Marks the connector, perform again to connect 2 connectors|
|RMB on background|Bring up New Expression menu|
|RMB on object|Bring up Object menu|
|RMB on connector|Bring up Object menu|
|Alt + LMB on connector|Break all connections to connector|
|Ctrl + LMB on connector|Add a new node along connector|


### Keyboard Controls

|Control|Action|
|---|---|
|Ctrl + B|Find in Content Browser|
|Ctrl + C|Copy selected expressions|
|Ctrl + S|Save Material|
|Ctrl + V|Paste|
|Ctrl + D|Duplicate selected objects|
|Ctrl + Y|Redo|
|Ctrl + Z|Undo|
|Delete|Delete selected objects|
|Spacebar|Force update all Material expression previews|
|Enter|(same as clicking apply)|

### Hotkeys

|Control|Action|
|---|---|
|A|Add|
|B|BumpOffset|
|C|Comment|
|D|Divide|
|E|Power|
|F|MaterialFunctionCall|
|I|If|
|L|LinearInterpolate|
|M|Multiply|
|N|Normalize|
|O|OneMinus|
|P|Panner|
|R|ReflectionVector|
|S|ScalarParameter|
|T|TextureSample|
|U|TexCoord|
|V|VectorParameter|
|1|Constant|
|2|Constant2Vector|
|3|Constant3Vector|
|4|Constant4Vector|
|Shift + C|ComponentMask|
