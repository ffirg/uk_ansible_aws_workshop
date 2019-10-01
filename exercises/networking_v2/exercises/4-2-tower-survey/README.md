# Exercise 4-2: Creating a Survey

## Table of Contents

- [Objective](#objective)
- [Guide](#guide)
- [Playbook Output](#playbook-output)
- [Solution](#solution)

# Objective

Demonstrate the use of Ansible Tower [survey feature](https://docs.ansible.com/ansible-tower/latest/html/userguide/job_templates.html#surveys). Surveys set extra variables for the playbook similar to ‘Prompt for Extra Variables’ does, but in a user-friendly question and answer way. Surveys also allow for validation of user input.

# Guide

## Step 1:

Open the web UI and click on the `Templates` link on the left menu.

![templates link](images/templates.png)

Click on the green `+` button to create a new job template (make sure to select `Job Template` and not `Workflow Template`)

| Parameter | Value |
|---|---|
| Name  | CONFIGURE BANNER  |
|  Job Type |  Run |
|  Inventory |  Workshop Inventory |
|  Project |  Workshop Project |
|  Playbook |  ios_banner.yml |
|  Credential |  Workshop Credential |

Scroll down and click the green `save` button.  


## Step 2:

To understand the next step we need to take a step back.  When we loaded this [workshop project](https://github.com/network-automation/tower_workshop) into Tower we have access to a variety of playbooks including this `ios_banner.yml` playbook.  This specific Playbook makes use of the ios_banner network module which looks like the following:

``` yaml
{%raw%}

  - name: ADD THE IOS BANNER
    ios_banner:
      text: "{{ network_banner }}"
      banner: login
{%endraw%}
```

We can set the variable `network_banner` anywhere and overload the default on the Playbook.  We will do this with an Ansible Tower **survey**.

## Step 3:

Click on the blue add survey button

![add survey button](images/addsurvey.png)

Fill out the fields

| Parameter | Value |
|---|---|
| Prompt  | Please enter the login banner information  |
|  Description |  Please type into the text field the desired banner |
|  Answer Variable Name |  network_banner |
|  Answer type |  Textarea |
|  Required |  Checkmark |

It should look like this screenshot:

![workshop survey](images/survey.png)

Click the green +Add button

![add button](images/add.png)

Click the green **save** button to save the survey.  This will exit back to the main job template window.  Scroll down and click the second green **save** button to exit to the job templates window.

## Step 3

Click on the rocket ship to launch the job template.

![rocket launch](images/rocket.png)

The job will immediately prompt the user to set the `network_banner` variable.  Type in the banner message you want for the routers.  Click next to see how the survey rendered the input as extra vars for the Playbook launch.  For this example I used the word ANSIBLE rendered into ASCII art.

Either type in some text, or for something a little more funky:

```bash
$ sudo yum -y install figlet
$ figlet ansible
                 _ _     _
  __ _ _ __  ___(_) |__ | | ___
 / _` | '_ \/ __| | '_ \| |/ _ \
| (_| | | | \__ \ | |_) | |  __/
 \__,_|_| |_|___/_|_.__/|_|\___|

```

Cut and paste the ascii art output into the survey window :)

![survey screen](images/surveyscreen.png)

Click the green **Launch** button to kick off the job.

![launch button](images/launch.png)

Let the job run to completion.  Let the instructor know if anything fails.


## Step 4

Login to one of the routers and see the banner setup

```
$ ssh rtr1
rtr4#exit
```

The banner will appear on login.  Here is an example from the **ANSIBLE** shown above.

![terminal output screen](images/terminal_output.png)


# Solution
You have finished this exercise.  

You have
 - created a job template for configuring a banner on the routers
 - Create a self service survey for the job template to fill out the network_banner variable
 - run the job template on all four routers loading the banner on them

[Click here to return to the lab guide](../../README.md)
