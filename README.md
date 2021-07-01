# develop-target-gsheet

## Usage
---

Need to install [singer-python](https://github.com/singer-io/getting-started/blob/master/docs/RUNNING_AND_DEVELOPING.md#a-python-tap) lib first:

`$ pipenv install singer-python`

Need to install **gspread** and **oauth2client** lib:

`$ pipenv install google-api-python-client oauth2client`

Some arguments are **required** to run a tap file:

| Param | Description |
| --- | --- |
| `-c` or `--config` | to access items needed in `config.json` |

This target file best to use along with the tap. To run a target file:

`$ pipenv run python <tap-file-name> -c config.json -s state.json | pipenv run python main.py -c config.json`

Make sure to set all the items in `config.json`, and share spreadsheet access with **client_email** in `client_secret.json` file.

## How to Develop a Target *(gsheet target in this case)*
---

## 1. Pipenv

> [Pipenv](https://docs.python-guide.org/dev/virtualenvs/) is a new community standard application that combines pip & virtual env, and extends their functionality in a single app.

To install pipenv:

`$ pip3 install --user pipenv`

## 2. Singer-Python

> [Singer.io](https://github.com/singer-io) is an open source standard for moving data between databases, web APIs, files, queues, etc. The Singer spec describes how data extraction scripts called **Taps** and data loading scripts called **Targets** should communicate using a standard JSON-based data format over stdout.

Can use [singer-python](https://github.com/singer-io/getting-started/blob/master/docs/RUNNING_AND_DEVELOPING.md#a-python-tap) library to implement singer using Python.

To install **singer-python**:

`$ pipenv install singer-python`

## 3. Gspread & Oauth2client

To install **gspread** and **oauth2client**:

`$ pipenv install google-api-python-client oauth2client`

## 4. Create Scripts

Detail about every script files:

| Files | Description | Reference |
| --- | --- | --- |
| `main.py` |  Is the main file, taken from [singer target template](https://github.com/singer-io/singer-target-template). Basically, this file validate the output data from tap, both the schema & the record, and call `write_to_spreadsheet` func from `src/gsheet_access.py` as final act. | [singer target template](https://github.com/singer-io/singer-target-template) |
| `src/gsheet_access.py` | Set up connection to the spreadsheet file, & update it using data from tap. | |
| `src/config.py` | Contain functions to access items in `config.json`. | |
| `config.json` | Contain configuration items needed to run target: `spreadsheet_id`, `active_sheet`. | [singer config file docs](https://github.com/singer-io/getting-started/blob/master/docs/CONFIG_AND_STATE.md#config-file) |
| `client_secret.json` | Contains credentials to connect with spreadsheet. | [create google service account](https://support.google.com/a/answer/7378726?hl=en) |

## 5. Detail about `config.json`

| Item | Description | Is Required? | Reference |
| --- | --- | --- | --- |
| `spreadsheet_id` | Unique ID of spreadsheet, can retrieve this from the spreadsheet URL. | **true** | |
| `active_sheet` | sheet name the we want to use. | **true** | |