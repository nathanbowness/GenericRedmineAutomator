# Generic Redmine Automator
Template for all Redmine Automation processes, the Generic template includes the Redmine API library. It also provides  
a default setup for the user to input needed information such as API keys, nas mount location and even custom terms.

## Install Process
- Clone this repository into the directory you would like your project to exist in. 
```console
git clone https://github.com/nathanbowness/RedmineAPI.git --recursive
```
- Rename the project to your desired name
- Install all requirements for the RedmineAPI in your environment
```console
cd "your_project_name"/RedmineAPI
pip3 install requirements.txt
```
- Customize the desired sections for your automation task

## Overview

...

## Usage
All changes needed are in the "YourAutomatorName.py" class that you will rename.

1. Change name of Classes
- The "YourAutomatorName_Run.py" should be replaced with your project name
- The "YourAutomatorName.py" should be replaced as well

2. Set Custom Terms for Automator -- '__init__(self, force)'

- Change the custom_terms 'key value-tuple' dictionary, used to ask the user for custom inputs and log them to the config 
file. Multiple 'key and value-tuples' can be put into this dictionary as long as they follow the format: 
```console
    custom_terms = {"your_key": ("your_default_value", "ask user to input" - i.e True/False, "type of input" - i.e. str) ...}
```
- Once a custom term has been inputted by the user and stored in the config, it is accessed by key from a dictionary. So
to use your custom terms in the code, a local variable can be set by: 
```console
    # Custom terms saved to the config after getting user input
    self.custom_values = setup.get_custom_term_values()
    # *** can be multiple custom values variable, just use the key from above to reference the inputted value ***
    self.your_custom_value_name = self.custom_values["your_key"] 
```

3. Set Topic & Status -- '__init__(self, force)'
- Set the issue_title variable to be the desired Subject. This will filter all issues by Subject and activate the 
Automated process for these - if they have the proper status 
- Set the issue_status variable to be the desired Status. This will filter all issues by Status as well ('New' is a 
default)
```console
    self.issue_title = 'your title'  # must be a lower case string to validate properly
    self.issue_status = 'New'
```
**Both of these must be satisfied for the automation task to run on an issue

4. Implement Code to run your Automation Process -- 'respond_to_issue(self, issue)'
- Inside the outlined area show below, implement your code to run the desired task. This task will run on every issue
with the specified Subject and Status
```console
    self.access_redmine.update_status_inprogress(issue, self.botmsg)
##########################################################################################
            print("Implement your process right here")
##########################################################################################
    self.completed_response(issue)
```

5. Change the Completed Response Message -- 'completed_response(self, issue)'
- Once the process has been completed, the author will have the issue assigned back to them with a message. 
This message can be customized to include any information your desire, just change the top line shown below in the 
'completed_response' method.
```console
    issue.redmine_msg = "your finishing message back to the author"
    self.access_redmine.update_issue_to_author(issue, self.botmsg)
```

