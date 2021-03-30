---
layout: default
title: Posting an Experiment
parent: Tutorials
nav_order: 4
---

# Posting an Experiment
{:.no_toc}

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

Make sure you follow your institution or department's protocols and procedures before you post your experiment. For students taking LINGUIST 245B, find more information [here](https://linguistics.stanford.edu/resources/experimental-protocol).

## Publishing the experiment

In order to run your experiment, it has to be available at a public HTTPS URL. One advantage of storing the experiment code on GitHub is that you can set up a respository to make your experiment publicly available.

1. Go to your repository (`https://github.com/<YOUR-GITHUB-USER>/<NAME-OF-REPOSITORY>`)
2. Click on _Settings_ in the upper-right corner
3. Scroll down to _GitHub Pages_
4. Select your main branch (e.g., `main` or `master`)
5. Click _Save_

After building your website (this may take up to a minute), your experiment should then be available at

`https://<YOUR-GITHUB-USER>.github.io/<NAME-OF-REPOSITORY>/experiments/01_implicature_strength/experiment.html`

For example, my experiment is available at:

[https://sebschu.github.io/my-project-solution/experiments/01_implicature_strength/experiment.html](https://sebschu.github.io/my-project-solution/experiments/01_implicature_strength/experiment.html)

(Feel free to use my experiment if you are unsure whether your experiment is properly working.)

## Creating an experiment on Mechanical Turk

We use Submiterator to manage external HITS on Mechanical Turk. If you're planning to run multiple experiments, follow [the instructions here](https://github.com/thegricean/LINGUIST245B/tree/master/setup) to make Submiterator a system wide command. This way, you won't need to copy `supersubmiterator.py` every time you want to run an experiment and you can run commands from any directory. Remember that if you haven't made Submiterator a system wide command, you will also need to include `python supersubmiterator.py` at the beginning of your Submiterator commands.

### Setting up an experiment using Submiterator

1. On your computer, create a new folder inside the folder that contains your experiment (`../<NAME-OF-REPOSITORY>/experiments/01_implicature_strength`) and name it `mturk`. (This folder will contain all MTurk related files and can really live anywhere on your computer, but it's good practice to keep it close to the actual experiment you're running.) 

2. Make sure you have a `.gitignore` file in in the root directory of your project (`<NAME-OF-REPOSITORY>`). `.gitignore` is a text file that tells Git which files or folders to ignore. We want to prevent the accidental uploading of confidential Worker IDs to the web so make sure this file includes `*mturk/` and other confidential files.

3. Inside the `mturk` folder, create a configuration file called `<experiment_name>`.config of the following format: (`<experiment_name>` can be any label for your experiment)
    
    ```
    {
	"liveHIT":"no",
	"title":"a title to show to turkers",
	"description":"a description to show to turkers",
	"experimentURL":"https://www.stanford.edu/~you/path/to/experiment.html",
	"keywords":"language research stanford fun cognitive science university explanations",
	"USonly?":"yes",
	"minPercentPreviousHITsApproved":"95",
	"minNumPreviousHITsApproved":"1000",
	"frameheight":"650",
	"reward":"0.00",
	"numberofassignments":"10",
	"assignmentsperhit": "9",
	"assignmentduration":"1800",
	"hitlifetime":"2592000",
	"autoapprovaldelay":"60000",
	"doesHaveQualification":"<ID_TO_MTURK_QUALIFICATION> or none",
	"doesNotHaveQualification": "<ID_TO_MTURK_QUALIFICATION> or none"
	}
    
    ```

### Testing the experiment in the MTurk Sandbox

1.	Change the configuration file to include the correct title and description of your experiment and the https:// URL to your experiment (this is the github.io URL from above). Make sure that "liveHIT" is set to "no". You can find more information about the options supported by Submiterator [here](https://github.com/sebschu/Submiterator).

2. To post the experiment on MTurk Sandbox, in the terminal, run the following command: 
(Replace `<experiment_name>` with the label that you used for your configuration file and make sure you run this command from the `mturk` folder that contains the configuration file.)

	```
	supersubmiterator posthit <experiment_name>
	```
This should return information about the experiment such as the following:

	```
	--------------------------------------------------------------------------------
    Successfully created hit with 10 assignments!
    Preview: https://workersandbox.mturk.com/mturk/preview?groupId=3DPZKFEUURGUPZ5ZFILM470JUA4I41
   	--------------------------------------------------------------------------------
	```

3. Paste the outputted URL into your browser and complete the experiment. Time yourself to get an estimate for long the experiment takes. 

4. To download your results, run the following command:

	```
	supersubmiterator getresults <experiment_name>
	```
This should return information about your data files such as the following:

	```
	--------------------------------------------------------------------------------
    Completed assignment for HIT "3FHTJGYT8N01D1NB593XCA0K49IGPD": 1/3
    --------------------------------------------------------------------------------
    Writing results to perception-trials.csv ...
    Writing results to perception-assignments.csv ...
    Writing results to perception-system.csv ...
    Writing results to perception-subject_information.csv ...
    --------------------------------------------------------------------------------
	```
These data files should now be in your `mturk` folder.


### Running the experiment

Only post your pilot or main experiment once you're absolutely certain that your experiment runs and that you're recording all the information you need for your analysis. Remember to first load sufficient funds onto your MTurk requester account (there is 20% amazon fee).

#### Posting the experiment

1. In the configuration file change "liveHIT" to "yes", put the total number of participants you want to test and how much you 
want to pay each participant. 

2. Post the experiment on Mechanical Turk: (Replace `<experiment_name>` with the label that you used in step 3).
	
	```
	supersubmiterator posthit <experiment_name>
	```

#### Monitoring the experiment

[MTurk Manage](https://github.com/jtjacques/mturk-manage) is a great tool that you can use to monitor your experiment. 

#### Downloading the data

	You can download your results with the following command:

	```
	supersubmiterator posthit <experiment_name>
	```
	
This should return information about your data files such as the following:

	```
	--------------------------------------------------------------------------------
    Completed assignment for HIT "3FHTJGYT8N01D1NB593XCA0K49IGPD": 1/3
    --------------------------------------------------------------------------------
    Writing results to perception-trials.csv ...
    Writing results to perception-assignments.csv ...
    Writing results to perception-system.csv ...
    Writing results to perception-subject_information.csv ...
    --------------------------------------------------------------------------------
	```
These data files should now be in your `mturk` folder.

#### MTurk related things to keep in mind

- Post experiments on weekdays and early morning to avoid noisy data (before 12pm PST Monday-Thursday).
- Sign up for accounts at Turkernation and/or Turkopticon to interact with Turkers or see how you are being rated.
- Generally try to make Turkers comfortable: Include a progress bar in your experiment for a sense of how long the experiment will take. Pay fairly ($12-14/h). Include clear but concise instructions.
- Monitor your email while HIT is running.

## Creating an experiment on Prolific

To recruit and pay participants, you'll have to create a study on Prolific. This is the digital equivalent of posting a flyer for recruiting participants.

1. [Create a study](https://app.prolific.co/studies/new) on Prolific.

2. Fill in the title and the description of the study in the "Study Details" section (this will be visible to your participants).

3. In the "Study Completion" section, select "I'll redirect them using a URL" 

4. Copy the completion URL that appears.

Keep this window open since you’ll need to go back to it later.

### Setting up an experiment in Proliferate

Once you have a completion URL, you can set up the study in Proliferate. The main purpose of Proliferate is to record, store, and aggregate the data from your participants. You can also use it to montitor the progress of your experiment.

1. On your computer, create a folder in which you'll put the proliferate configuration file.

2. In this folder, create a configuration file called `<experiment_name>`.config of the following format: (`<experiment_name>` can be any label for your experiment.)

    ```json
    {
      "name": "Name of your experiment",
      "notes": "Optional notes #lsa2021",
      "completion_URL": "The completion URL that you copied from Prolific",
      "experiment_URL": "The https:// URL to your experiment (this is the github.io URL from above)"
    }
    ```
    
   For this course, please include `#lsa2021` in the notes (this will be used behind the scenes to merge all the results from everyone, so that there is more data to analyze.)

3. Create the experiment on proliferate: (Replace `<experiment_name>` with the label that you used in step 2.)

    ```bash
    proliferate post <experiment_name>
    ```

    This should return information about the experiment such as the following:

    ```
    --------------------------------------------------------------------------------
    Successfully created experiment!
    Loading info...
    --------------------------------------------------------------------------------
    Name: Name of your experiment.
    Notes: Optional notes #lsa2021
    Created at: 2020-09-11 06:54 PM
    Prolific completion URL: https://app.prolific.co/submissions/complete?cc=0815ABCD
      (Participants will be redirected to this URL after completing the experiment.
      Make sure that this URL matches the completion URL of your study on Prolific.)


    Study URL: https://proliferate.alps.science/experiment/43983215-a648-49fa-bf1a-1deff8f0e7c6
      (Use this URL to publish your study on Prolific.)

    Sandbox URL: https://proliferate.alps.science/experiment/43983215-a648-49fa-bf1a-1deff8f0e7c6/debug
      (Use this URL to test your experiment using the "Preview" function on Prolific.)

    Progress: 0 completed (0 started, 0 completed, 0 abandoned, ∞ requested)
    Experiment location: The https:// URL to your experiment
    --------------------------------------------------------------------------------
    ```

### Testing the experiment

1. Go back to the Prolific create form from step 1 and copy the **Sandbox** URL into the "Study Link" section on Prolific.

2. Click "Preview" at the bottom of the create form, and in the popup window, click "Open study link in a new window". This should load your experiment in a new window.

3. Go through the entire experiment. At the end of it you should be redirected back to Prolific and it should say "Submission received".

4. Make sure that your data was recorded by downloading the sandbox data. In the Terminal, run the following command:

```bash
proliferate getresults --sandbox <experiment_name>
```

**Note**: The `--sandbox` flag causes proliferate to download the debugging results. To download the actual results, omit this parameter (see below).

If your data was properly recorded, this should download several CSV files with your data. Make sure that all the data you need is present in the CSV files. If no data was recorded or some data is missing, check your experiment code and make sure that all data is recorded during the experiment and that all data is sent back to proliferate at the end of the experiment.

If you want to read up on how proliferate turns the JSON data from your experiment into CSV files that can be easily analyzed in R or other statistical software, take a look at the [data processing section of the proliferate documentation](https://docs.proliferate.alps.science/en/latest/data.html).

### Things you'd do if you actually ran the experiment

For the purpose of this course, we'll stop here and not actually publish the experiment on Prolific. But if you were running an actual study and wanted to recruit participants from the Prolific pool, complete the following additional steps. (See also the [proliferate documentation](https://docs.proliferate.alps.science/en/latest/cli/managing-experiments.html))


#### Posting the experiment

If you were actually running the experiment with real participants, you would then publish it on Prolific.

1. Go back to the Prolific create form from step 1 and copy the Study URL into the "Study Link" section on Prolific.

2. Make sure that you select "I’ll use URL parameters" below the Study URL after you pasted the URL. This should append several parameters of the form `?PROLIFIC_PID=...` to your study URL. If these parameters are missing, select another option (e.g., "I’ll add a question in my study") and then click again on "I’ll use URL parameters".

3. In the "Audience" section, select the pre-screening criteria that you want to use.

4. In the "Study cost" section, choose the number of participants you would like to run, enter your estimate of how long the experiment will take, and how much participants get paid for participating in your experiment. In the US, you should pay participants at least $15/hour.

5. Click "Publish" to publish your experiment.

At this point, participants will be able to access your experiment. You can then monitor how many participants have completed or at least started your experiment, as described in the next section.

#### Monitoring the experiment

You can monitor the progress of your experiment with the `info` command:

```bash
proliferate info <experiment_name>
```

See the [proliferate documentation](https://docs.proliferate.alps.science/en/latest/cli/managing-experiments.html#monitoring-an-experiment) for more information on how to interpret the output.


#### Downloading the data

You can download results using the `proliferate getresults` command:

```bash
proliferate getresults <experiment_name>
```

This will download data from all participants as CSV files.

If you want to download the data from debugging the experiment with the Sandbox URL, add the `--sandbox` flag as we did above:

```bash
proliferate getresults  --sandbox <experiment_name>
```
#### Prolific related things to keep in mind

- Approval rate can be set under Custom prescreening > Participation on Prolific. (e.g. a 95% approval rate excluded ~6% of the pool)
- To get Native US English speakers set:   
	First Language --> English  
	Country af Birth --> US    
	Current Country of Residence --> US
- For eye-tracking studies, the following parameters can be set:  
	Vision (under General Health): Yes (to select for participants with normal/corrected-to-normal vision)  
	Webcam (under Other): Yes   
	Operating system (under Other): options are not very fine-grained, but could be useful to select only for participants using newer OSs (e.g. Windows 10 vs. prior releases) given processing demands of online eye-tracking software.