# OSIC-MSR-tool

OSIC MSR (monthly status report) reporting tool

_This tool is in ALPHA state - use at your own risk (although there's probably not much risk)._
It was written as a quick _hack_, so it could undoubtedly use cleanup.

This was written using Python 3.5, and has been tested with Python 2.7.
If you're (still) using python-2, then use the requirements.py2.txt file for dependencies.

## Installation:

 - Clone this repository
 - create a virtualenv for this tool (recommended);
 - run `python -m pip install -U -r requirements.txt`
   - use the appropriate requirements file for your python version
 - copy the config file, `msr.ini.example` to `msr.ini` and modify for your group/uses.
   - optionally, update / modify the template file to suit you.
 - make a `data` directory
   - alternately, you could make a `~/.config/MSR/data` directory

#### Installation options:
  - you can place your `data` and `templates` directory in `~/.config/MSR`;
  - ...or anyplace you specify your path (see below)

## Configuration:

`report.py` uses a **config** file,  `msr.ini`.
This should have the `gerrit_id`s of your project's members.
You can also specify which fields from the Stackalytics API, and Gerrit API you wish to summarize (i.e., make available to the template).

Output is handled by a **Jinja2** template.
The default is a text template.
You can specify the default template file name.
Text, HTML, or docx templates should be possible.
Get creative, and make a pull request.

Projects which contain **sandbox** in their name are not summarized at this time (for now, this is not configurable).

## Operation:

The output, as generated by the **Jinja2** temmplate, is output to stdout, so typical usage might be something like this:

    $ report.py > april.txt

You can then edit the output for any prose / paragraphs called for in the template.

Report.py gets activity data per user from the Stacalytics API, and the Gerrit API and by default stores data as a YAML file in the data area.
The organization of the data area is by *YYYY/Mth* name.
Open activity is pulled from the Gerrit API.
Individual filenames are named by `YYYY.MM-#days.gerrit_id`
with an extension of `yaml` for data from the Stackalytics API,
and `.open.yaml` for open items fetched from the Gerrit API.

For example, my data file for April 2016 would be named:

    ./data/2016/Apr/2016.4.1-30.yarkot.yaml

At some point, a date or date range might be offered as an option.
Currently, data is collected for the previous month.

If a data file is found in the data path, then it is used, otherwise the appropriate API is hit, and the data stored in the appropriate path.
The first existence of a data area in your path is used.
The month, and potentially the year directory are added, as needed.
If your data path base directory doesn't exist, it's an error, and `report.py` exits.

# TODO's

- add options, as driven by use;
- add option for specific month;
  - add option for date range;
- add option to force update from API;
- add option to roll-up data (e.g. from manageer collecting from multiple groups);
- investigate collecting blueprint activity from launchpad;
  - [https://help.launchpad.net/API/launchpadlib]()
- add an environment variable to set msr.ini file path
