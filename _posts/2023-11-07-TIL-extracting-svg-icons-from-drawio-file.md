---
layout: post
title: TIL - extracting SVG icons from a Draw.io file
---

*In this TIL, I will share the details on how you can extract the SVG icons from a Draw.io library and "export them" to SVG and PNG*

Today I bumped into a repository that contains a few Draw.io libraries. The libraries contain icons that can be used to create your diagrams in Draw.io. In my case, I would like to get the icons so that I can end up incorporating them in a project that I wrote some time ago to create diagrams with code.

The Draw.io library file is an XML file with a JSON structure within the root node. Yep... Below, is a sample of the contents of Draw.io library file.

```xml
<mxlibrary>[
  {
    "data": "data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHZpZXdCb3g9IjAgMCA0OCA0OCI+PGRlZnM+PHN0eWxlPi5jbHMtMXtmaWxsOiM1OTU5NTk7fTwvc3R5bGU+PC9kZWZzPjx0aXRsZT5icm9rZW4tbGluazwvdGl0bGU+PGcgZGF0YS1uYW1lPSJMYXllciAyIiBpZD0iTGF5ZXJfMiI+PGcgaWQ9ImJyb2tlbi1saW5rIj48cGF0aCBkPSJNMTIsMTcuMjVIMHYtMS41SDEyWk0yMi4wMzEsMzQuNUgyNi4yNWwtOC4wNjIsOC4xNTZhNy41MTEsNy41MTEsMCwwLDEtMi43NjYsMS43ODJBOS4xNDksOS4xNDksMCwwLDEsMTIuMjgxLDQ1YTguNzUzLDguNzUzLDAsMCwxLTMuMDkzLS41NjIsNy41ODIsNy41ODIsMCwwLDEtMi43MTktMS43ODJMNS40MzgsNDEuNTMxYTguMTQ4LDguMTQ4LDAsMCwxLDAtMTEuNjI1TDEyLDIzLjI1djQuMzEyTDcuNTk0LDMxLjk2OWE1LjIwOCw1LjIwOCwwLDAsMCwwLDcuNUw4LjYyNSw0MC41QTQuOTY5LDQuOTY5LDAsMCwwLDEyLjI4MSw0MmE1LjA4NCw1LjA4NCwwLDAsMCwzLjc1LTEuNVpNMTcuMjUsMTJoLTEuNVYwaDEuNVpNNDEuNDM4LDYuNDY5YTguMjM1LDguMjM1LDAsMCwxLDEuODI4LDIuNzE5LDguMTUsOC4xNSwwLDAsMSwuNjA5LDMuMDkzLDguNSw4LjUsMCwwLDEtLjYwOSwzLjE0MSw4LjEsOC4xLDAsMCwxLTEuODI4LDIuNzY2TDM0LjUsMjUuMTI1YTcuODkyLDcuODkyLDAsMCwxLTUuODEyLDIuMzQ0LDcuODkxLDcuODkxLDAsMCwxLTUuODEzLTIuMzQ0TDIxLjc1LDI0YTIuOTkyLDIuOTkyLDAsMCwxLS4zNzUtLjQ2OWMtLjEyNi0uMTg3LS4yNS0uMzc1LS4zNzUtLjU2MkwyMi45NjksMjFsMS45NjksMS45NjlhNS40MzgsNS40MzgsMCwwLDAsNy41LDBsNi45MzctNi45MzhhNS4zMjEsNS4zMjEsMCwwLDAsMC03LjQwNkwzOC4yNSw3LjVBNC45NzksNC45NzksMCwwLDAsMzQuNTk0LDZhNS41MTcsNS41MTcsMCwwLDAtMy43NSwxLjVMMjcsMTEuMzQ0VjcuMTI1bDEuNjg4LTEuNjg3QTguNDU3LDguNDU3LDAsMCwxLDM0LjU5NCwzYTguMTMxLDguMTMxLDAsMCwxLDUuODEyLDIuNDM4Wk0zMC43NSwzNmgxLjVWNDhoLTEuNVpNNDgsMzAuNzV2MS41SDM2di0xLjVaIiBjbGFzcz0iY2xzLTEiLz48L2c+PC9nPjwvc3ZnPg==",
    "w": 48,
    "h": 48,
    "title": "image-1",
    "aspect": "fixed"
  },
  {
    "data": "data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHZpZXdCb3g9IjAgMCA0MS45MDcgNDgiPjxkZWZzPjxzdHlsZT4uY2xzLTF7ZmlsbDojNTk1OTU5O308L3N0eWxlPjwvZGVmcz48dGl0bGU+Y2FsZW5kYXI8L3RpdGxlPjxnIGRhdGEtbmFtZT0iTGF5ZXIgMiIgaWQ9IkxheWVyXzIiPjxnIGlkPSJjYWxlbmRhciI+PHBhdGggZD0iTTM4LjkwNywzYTIuODM5LDIuODM5LDAsMCwxLDIuMTU2Ljg5MUEyLjk2NywyLjk2NywwLDAsMSw0MS45MDcsNlY0NWEyLjg4NiwyLjg4NiwwLDAsMS0yLjk5MiwzSDIuOTkyQTIuODg0LDIuODg0LDAsMCwxLDAsNDVWNkEyLjk2MiwyLjk2MiwwLDAsMSwuODQ0LDMuODkxLDIuODM2LDIuODM2LDAsMCwxLDMsM0g5VjBoM1YzSDI5LjkwN1YwSDMzbC0uMDkzLDNabTAsOUgzVjQ1SDM4LjkwN1pNMTIsMjUuNjg4YTYuNDA3LDYuNDA3LDAsMCwxLTMuNDY5LjkzN1YyMy4wNjJhMy41NjEsMy41NjEsMCwwLDAsMi40ODUtLjg5LDIuOTU2LDIuOTU2LDAsMCwwLC45ODQtMi4zaDMuNDY5djIwLjI1SDEyWk05LDloM1Y2SDlaTTI3LjQ2OSwzNS40MzhIMTguNTYyVjMxLjk2OWw4LjkwNy0xMi4wOTRoMy40NjlWMzEuOTY5SDM0LjV2My40NjlIMzAuOTM4djQuNDA2SDI3LjQ2OVptLTUuMzkzLTMuNDY5aDUuMzkzbC0uMDYxLTcuMTI1Wk0yOS45MDcsOWgzVjZoLTNaIiBjbGFzcz0iY2xzLTEiLz48L2c+PC9nPjwvc3ZnPg==",
    "w": 42,
    "h": 48,
    "title": "image-2",
    "aspect": "fixed"
  }
]</mxlibrary>
```

Once I was familiar with the structure of the library file, I proceeded to create a quick Python script to process the file(s) and "extract" the images from within it. In my case, I want to save the images as SVG and PNG files. You can find the latest version of the script below here - [https://gist.github.com/ajmaradiaga/7d0d5a7a528db46f08b810d575d15916](https://gist.github.com/ajmaradiaga/7d0d5a7a528db46f08b810d575d15916). 

Python 🐍 script:

```python
import base64
import json
import xml.etree.ElementTree as ET
import os

import cairosvg

process_files = [
    'Library-1.drawio',
    'Library-2.drawio'
]

for file in process_files:

    file_name = os.path.basename(file)
    directory_name = file_name.replace(".drawio", "")

    # Parse the XML file and get the mxlibrary node
    tree = ET.parse(file)
    root = tree.getroot()

    if root.tag != "mxlibrary":
        print(f"Error: {file} is not a valid mxlibrary file")
        continue

    # Extract the JSON array from the mxlibrary node
    json_array = json.loads(root.text)

	# Create the directories where the output images will be saved
    os.makedirs(f"{directory_name}/svg", exist_ok=True)
    os.makedirs(f"{directory_name}/png", exist_ok=True)

    # Iterate through every item in the JSON array
    for item in json_array:
        
        if "data" in item and "title" in item:

            # Extract the data and title from the item
            data = item["data"]
            title = item["title"]

            if "data:image/svg+xml;base64," in data:
                # Extract the base64 encoded SVG data
                svg_data = data.replace("data:image/svg+xml;base64,", "")
                # Decode the base64 encoded SVG data
                svg_bytes = base64.b64decode(svg_data)
                # Convert the SVG bytes to a string
                svg_string = svg_bytes.decode("utf-8")
                
                try:
                    # Save the SVG to a file
                    cairosvg.svg2svg(bytestring=svg_string, write_to=f"{directory_name}/svg/{title}.svg")

                    # Convert the SVG to PNG
                    cairosvg.svg2png(bytestring=svg_string, write_to=f"{directory_name}/png/{title}.png")
                except Exception as e:
                    print(f"Error converting {title} to PNG: {e}")
            
```

I hope you find this useful in case you ever run into a similar requirement :-)