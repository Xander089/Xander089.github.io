---
layout: page
permalink: "/course/lods"
---

LODs can be added to a static mesh in order to automatically change mesh depending on the occupied screen size of an object.

To change Level of Details of a static mesh:

- Open the mesh
- Go to Details
- Filter "LOD"
- Flag Custom in order to edit multiple LOD at a time
- The default LOD is LOD0
- Under Lod Settings, Import, you can add a second LOD by importing the desired .fbx
- Once imported LOD1 will appear
- you can repeat this process to add even further LODs
- Unflag Auto Compute LOD distances: now you can edit "Screen Size" for each LOD independently
- Screen Size represents the percentage of screen at which the mesh of a specific LOD will be replaced on screen
