---
description: importing CIS Controls
---

# CIS Controls

To import the CIS Controls, you need to prepare the file first. The easy way is to copy the Excel sheet as-is (`CIS_Controls_Version_8.xlsx`) into the tools folder and run `convert_cis.sh`

Alternatively, you can run the prep script first (`cis/prep_cis.py`) and mention any short string as the packager and then pass the new Excel sheet to the `convert_library.py`
