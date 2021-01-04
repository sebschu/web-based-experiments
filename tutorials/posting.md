---
layout: default
title: Posting an Experiment
parent: Tutorials
nav_order: 4
---

# Posting an Experiment

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

[https://sebschu.github.io/my-project/experiments/01_implicature_strength/experiment.html](https://sebschu.github.io/my-project/experiments/01_implicature_strength/experiment.html)

## Creating an experiment on Prolific

To recruit and pay participants, you'll have to create a study on Prolific. This is the digital equivalent of posting a flyer
for recruiting participants.


1. [Create a study](https://app.prolific.co/studies/new) on Prolific.

2. Fill in the title and the description of the study in the "Study Details" section (this will be visible to your participants).

3. In the "Study Completion" section, select "I'll redirect them using a URL" 

4. Copy the completion URL that appears.

Keep this window open since you’ll need to go back to it later.

## Setting up Proliferate

Once you have a completion URL, you can set up the study in Proliferate. The main purpose of Proliferate is to record, store, and
aggregate the data from your participants. You can also use it to montitor the progress of your experiment.

1. On your computer, create a folder in which you'll put the proliferate configuration file.

2. In this folder, create a configuration file called <experiment_name>.config of the following format: (<experiment_name> can be any label for your experiment.)

```json
{
  "name": "Name of your experiment",
  "notes": "Optional notes",
  "completion_URL": "The completion URL that you copied from Prolific",
  "experiment_URL": "The https:// URL to your experiment (this is the github.io URL from above)"
}
```

3. Create the experiment on proliferate: (Replace <experiment_name> with the label that you used in step 2.)

```bash
proliferate post <experiment_name>
```

This should return information about the experiment such as the following:

```
--------------------------------------------------------------------------------
Successfully created experiment!
Loading info...
--------------------------------------------------------------------------------
Name: Name of your experiment
Notes: Optional notes
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

## Testing the experiment

## Posting the experiment

## Downloading the data

