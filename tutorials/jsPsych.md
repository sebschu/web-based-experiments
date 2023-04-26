---
layout: default
title: jsPsych Tutorial
parent: Tutorials
nav_order: 6
---

# jsPsych

## Table of contents
1. [Getting started](#getting-started)
    1. [What is jsPsych?](#what-is-jspsych)
    2. [Setting up a directory](#setting-up-a-directory)
    3. [Basic HTML setup](#basic-html-setup)
    4. [Basic jsPsych integration](#basic-jspsych-integration)
2. [Creating the experiment logic](#creating-the-experiment-logic)
    1. [Plugins](#plugins)
    2. [Adding trials](#adding-trials)
        1. [Consent and instructions](#consent-and-instructions)
        2. [Main experiment](#main-experiment)
        3. [Adding custom CSS](#adding-custom-css)
        4. [The `data` object ](#the-data-object)
        5. [Adding dynamic functionality](#adding-dynamic-functionality)
        6. [Linking custom JavaScript](#linking-custom-javascript)
        7. [Repeating trials](#repeating-trials)
        8. [Adding trials of a different type](#adding-trials-of-a-different-type)
        9. [Preloading media](#preloading-media)
        10. [Surveys](#surveys)
        11. [Wrapping up](#wrapping-up)


## Getting started

### What is jsPsych?
<a href="https://www.jspsych.org/7.3/" target="_blank">JsPsych</a> is a JavaScript library that can be used to simplify the process of creating behavioral experiments as web apps. You'll still need knowledge of JavaScript, HTML, and CSS, and there's no GUI (i.e. you can't drag and drop trials into place), but a lot of the basic procedures that make up behavioral experiments can be handled with just a few lines of code. Each of these procedures is handled by a jsPsych *plugin*, the base code for which can be imported directly into your experiment.

JsPsych is open source and actively developed on <a href="https://github.com/jspsych/jsPsych" target="_blank">GitHub</a>. If you use jsPsych for research that you're planning to publish, you can cite <a href="https://link.springer.com/article/10.3758/s13428-014-0458-y" target="_blank">this paper</a> (deLeeuw, 2015).

### Setting up a directory
On the 245B GitHub page, you can download code for a simple, fully functional jsPsych experiment, but let's a good idea to build that from scratch here in order to understand the logic of a jsPsych experiment. 

To get started, you need a directory where all the experiment files will live. To run an experiment, you minimally need an `.html` file and a `.js` file. Although not strictly necessary, you will also often find that it makes sense to have a `.css` file so you can easily customize styling and a second `.js` file so that you can write helper functions without cluttering the file containing your main experiment logic. 

So, open up your terminal, navigate to the parent directory where you want your experiment to live, and generate these blank files. To do this, you can execute the following lines of code in your terminal:

``` 
> cd [user]/[whatever]/[wherever]/[directory]
> mkdir exp_tutorial
> cd exp_tutorial
> touch experiment.html experiment.js experiment.css util.js
```

For good measure, you can now type `ls` to make sure all the files you just generated actually exist. Nice. Now open up that directory in your text editor (VS Code, Sublime, Atom, etc.)

### Basic HTML setup
The first thing you need to do is write a basic HTML file. This file is what you open in the web browser to test your experiment. It's sort of like the home base. An HTML file minimally includes a "doctype" declaration, an `<html>` tag, a `<head>` tag, and a `<title>` tag. We tell the HTML file which JS scripts to execute with `<script>` tags, and link stylesheets with `<link>` tags. In this case, a good starting place for the HTML file would be something like: 

``` html
<!DOCTYPE html>
<html>
    <head>
        <!-- link the CSS file you made-->
        <link href="experiment.css" rel="stylesheet" type="text/css" />

        <!-- Tell the file to run some .js files -->
        <script src="util.js"></script>
        <script src="experiment.js"></script>

        <!-- This is the name that will appear on the browser tab. -->
        <title>Language Experiment</title>
    </head>
</html>
```

Now you can open up this file in Chrome to start testing your experiment. (Or whatever browser you use. But Chrome is a good option, because it has nice developer tools, which can be accessed by pressing `opt-cmd-i`.) At this point, you shouldn't see anything, but you also shouldn't be getting any errors in your console.



### Basic jsPsych integration
There are three ways that we can get our HTML file to incorporate elements of the jsPsych library: linking to CDN-hosted files, hosting the files locally, or using Node and NPM. (If you're pretty familiar with web development, you're welcome to use this third option, but we won't get into it here.) The most straightforward option is to include a call to an externally CDN-hosted version of jsPsych. To do that, you would add these lines to the head of your HTML document:

``` html
<!-- basic jsPsych functionality -->
<script src="https://unpkg.com/jspsych@7.3.2"></script>

<!-- default jsPsych stylesheet -->
<link href="https://unpkg.com/jspsych@7.3.2/css/jspsych.css" rel="stylesheet" type="text/css" />
```

The most up-to-date link for each plugin can be found on that plugin's page in the documentation. (More on this later.) The CDN method works in most cases, and is straightforward to set up, but it doesn't allow much customization. Another way to link files from the jsPsych library is to download a distribution and host it locally. If you need to do any customization, this is a better option. You can download a distribution from <a href="https://github.com/jspsych/jsPsych/releases">GitHub here</a> or directly from <a href="https://www.github.com/jspsych/jspsych/releases/latest/download/jspsych.zip">this link</a>. After you've downloaded it, just move it to your current working directory (probably `exp_tutorial`). Then, in the head of your HTML doc, you can supply a relative path directly to the relevant scripts, e.g.:

``` html
<!-- basic jsPsych functionality -->
<script src="jspsych/dist/jspsych.js"></script>
<!-- default jsPsych stylesheet -->
<link href="jspsych/dist/jspsych.css" rel="stylesheet" type="text/css" />
```
*N.B. The browser will read .js files in order, so you'll want jsPsych files to precede your custom files, since you need to make reference to their content.*

To start populating the experiment with content, pull up the `experiment.js` file in your text editor. To create a jsPsych experiment, we need to initialize jsPsych (effectively constructing an object of the jsPsych class). 

``` javascript
const jsPsych = initJsPsych();
```

The logic of the experiment is created through an array of individual procedures constructed using jsPsych plugins. When the experiment runs, it works through that array in order to execute each chunk of logic. The conventional name for this array that contains the logic is `timeline`, so also create an empty array with this name. 

``` javascript
const jsPsych = initJsPsych();
let timeline = [];
```

The last thing we have to do for basic setup is to tell jsPsych to run through the logic defined in the timeline. To do so, we simply pass `timeline` as an argument to the `run` method:

``` javascript
const jsPsych = initJsPsych();
let timeline = [];
// push experiment logic the timeline here...
// ......

jsPsych.run(timeline)
```


The next step, which will take the bulk of your time, is to populate the timeline.

## Creating the experiment logic

### Plugins
As mentioned above, each chunk of the procedure is created with a jsPsych plugin. See <a href="https://www.jspsych.org/7.3/plugins/list-of-plugins/">this link</a> for a complete list of available plugins along with a short description of what each does. Helpfully, each plugin has a relatively detailed manual page, including information about what the plugin does, what information needs to be supplied for the plugin to work, default values for unspecified parameters, the link to the CDN-hosted version, examples of the plugin in action, etc. For example, here's the manual page for the <a href = "https://www.jspsych.org/7.3/plugins/audio-keyboard-response/">audio keyboard response</a> plugin. These manual pages will be *very useful* in developing experiments.

To use a plugin to create a chunk of experiment logic, just create an object with the plugin's required parameters specified and assign that to a variable. There is always a parameter called `type`, where the name of the plugin goes. After we generate a trial, it must be pushed to the timeline in order to be included in the experiment. This will look like:

``` javascript
const trial = {
    type: jsPsychPluginName,
    parameter1: "abc",
    parameter2: "def",
    ...
};
timeline.push(trial);
```

Each plugin you use needs to be linked to the `head` of your HTML file. Do this just like you loaded the main jsPsych file. Here's an example for the audio keyboard response plugin:

``` html
<!-- If you're using the CDN -->
<script src="https://unpkg.com/@jspsych/plugin-audio-keyboard-response@1.1.2"></script>
<!-- If you're hosting the file locally -->
<script src="jspsych/dist/plugin-audio-keyboard-response.js"></script>
```

### Adding trials
#### Consent and instructions
Now that we understand how plugins work, we can start actually adding trials to the experiment. Generally, the first thing a participant will see is a consent form. A good candidate plugin for this trial would be "html button response", which serves up some text in html form and then allows participants to proceed by clicking a button. Here's the <a href="https://www.jspsych.org/7.3/plugins/html-button-response/">documentation</a>. 

First thing to do, as always, is make sure to link the appropriate .js file in the head of the html document if it's not there already. In this case:

``` html
<script src="https://unpkg.com/@jspsych/plugin-html-button-response@1.1.2"></script>
<!-- or -->
<script src="jspsych/dist/plugin-html-button-response.js"></script>
```

Aside from the `type` parameter, we can tell from the manual that we will need to provide values for a `stimulus` parameter, which will include the text and `choices`, which provides text for the buttons. There are a bunch of other parameters to explore, but these aren't strictly necessary.

Let's assign this trial to a variable called `irb`. 

``` javascript
const irb = {
    // Which plugin to use
    type: jsPsychHtmlButtonResponse,
    // What should be displayed on the screen
    stimulus: '<p><font size="3">We invite you to participate in a research study on language production and comprehension. Your experimenter will ask you to do a linguistic task such as reading sentences or words, naming pictures or describing scenes, making up sentences of your own, or participating in a simple language game. <br><br>There are no risks or benefits of any kind involved in this study. <br><br>You will be paid for your participation at the posted rate.<br><br>If you have read this form and have decided to participate in this experiment, please understand your participation is voluntary and you have the right to withdraw your consent or discontinue participation at anytime without penalty or loss of benefits to which you are otherwise entitled. You have the right to refuse to do particular tasks. Your individual privacy will be maintained in all published and written data resulting from the study. You may print this form for your records.<br><br>CONTACT INFORMATION: If you have any questions, concerns or complaints about this research study, its procedures, risks and benefits, you should contact the Protocol Director Meghan Sumner at (650)-725-9336. If you are not satisfied with how this study is being conducted, or if you have any concerns, complaints, or general questions about the research or your rights as a participant, please contact the Stanford Institutional Review Board (IRB) to speak to someone independent of the research team at (650)-723-2480 or toll free at 1-866-680-2906. You can also write to the Stanford IRB, Stanford University, 3000 El Camino Real, Five Palo Alto Square, 4th Floor, Palo Alto, CA 94306 USA.<br><br>If you agree to participate, please proceed to the study tasks.</font></p>',
    // What should the button(s) say
    choices: ['Continue']
};

// push to the timeline
timeline.push(irb)
```

Now, if you open up your .html file in a browser, it should display the consent form and show a button that says "Continue". Nice. This is the fundamental mechanism by which experiments are built in jsPsych. Pretty much everything else you do will be some version of this procedure. 

Now that we have the participant's consent, we should add some instructions telling them what they'll be doing in this experiment (which, by the way, will be an auditory recognition memory experiment). Since participants will make keyboard responses for the following trials, we don't want them to have to move their hands from the mouse to the keyboard during the first trial, so let's use a keyboard response trial rather than a button response trial for the experiment instructions. For that we can use the <a href="https://www.jspsych.org/7.3/plugins/html-keyboard-response/">html keyboard response</a> plugin. Make sure to add the plugin to your html file:

``` html
<script src="jspsych/dist/plugin-html-keyboard-response.js"></script>
<!-- (Or CDN. You know the drill by now.) -->
```

Referring to the manual, we can see that this plugin has many of the same parameters as the html button response plugin, but where `choices` is an array of acceptable keyboard responses. Let's have participants proceed by hitting the space bar. 

``` javascript 
const instructions = {
    type: jsPsychHtmlKeyboardResponse,
    stimulus: "In this experiment, you will hear a series of words. If it's your first time hearing the word, press 'D' for NEW. If you've already heard the word during the task, press 'K' for OLD. Try to respond as quickly and accurately as you can.<br>When you're ready to begin, press the space bar.",
    choices: [" "]
};
timeline.push(instructions);
```

As a matter of convenience, it'd be nice to see what data jsPsych is recording from each trial. To do this, we can add an anonymous function to the initialization. Don't worry too much about exactly what's going on here (yet), but you can change your `initJsPsych` statement to this in order to show the data at the end of the experiment:

``` javascript 
const jsPsych = initJsPsych({
    on_finish: function () {
        jsPsych.data.displayData('csv');
      }
  });
```

Now when you finish the experiment, you'll see a page showing everything that's getting recorded.

#### Main experiment
Now we can get to the meat of the experiment. In each trial, we need to serve up some audio and then collect a keyboard response from a participant. A good fit for this, naturally, is the plugin <a href="https://www.jspsych.org/7.3/plugins/audio-keyboard-response/">audio keyboard response</a>. Add it to your html file:

``` html
<script src="jspsych/dist/plugin-audio-keyboard-response.js"></script>
```

Let's create a single trial of this type to start, and then we can generate more as needed. We'll start by assigning the `type` parameter. The only two acceptable responses are 'd' and 'k', so let's add those as `choices` (an array of strings).

``` javascript
const trial_1 = {
    type: jsPsychAudioKeyboardResponse,
    choices: ['d', 'k']
}
```

Of course, we also need some audio to act as a stimulus. Make sure you've downloaded the two sample audio files in this github directory, create a directory in your project called 'audio', and place the files in there. We tell jsPsych which audio to play by providing a relative path. In this case, if we wanted to play the file `Violin.wav`, that would be `audio/Violin.wav`. We can add this to the `stimulus` parameter (as a string):

``` javascript
const trial_1 = {
    type: jsPsychAudioKeyboardResponse,
    choices: ['d', 'k'],
    stimulus: 'audio/Violin.wav'
}
```

Now, if you push that trial to the timeline, you should be able to hear the stimulus, respond with 'd' or 'k' and then see the data. But you may notice that it's possible for participants to respond immediately, which ends the trial. That means that people can click through the whole experiment without listening to anything. To prevent this, there's a parameter for this plugin called `response_allowed_while_playing`, which takes a boolean argument. Let's set that to false:

``` javascript
const trial_1 = {
    type: jsPsychAudioKeyboardResponse,
    choices: ['d', 'k'],
    stimulus: 'audio/Violin.wav',
    response_allowed_while_playing: false
}
```

We might also want to set a maximum trial duration so that our RT data is more reliable. We can do that with the `trial_duration` parameter, whose argument is an integer representing the number of ms that must elapse before the experiment ends the trial even without a response. Let's set that to 4000 (4 s).

``` javascript
const trial_1 = {
    type: jsPsychAudioKeyboardResponse,
    choices: ['d', 'k'],
    stimulus: 'audio/Violin.wav',
    response_allowed_while_playing: false,
    trial_duration: 4000
}
```

Note that even with this parameter specified, the trial will end automatically when a response is made. That's because the plugin also has a parameter called `response_ends_trial` which is by default set to `true`. If a trial is behaving in a way you didn't expect, check the documentation and see if there are any parameters with default values. 

#### Adding custom CSS
We might also want participants to have a visual prompt so they don't forget which key is associated with which response. The parameter `prompt` takes an html string and displays that in the window. Here, we should remind participants that a response of 'd' means 'new' and 'k' means old. The simplest was to do this would be to just add raw text saying as much:

``` javascript
const trial_1 = {
    type: jsPsychAudioKeyboardResponse,
    choices: ['d', 'k'],
    stimulus: 'audio/Violin.wav',
    response_allowed_while_playing: false,
    trial_duration: 4000,
    prompt: "OLD: D; NEW: K"
}
```

But, that looks kind of sloppy. Remember that when there's an html input, we can do anything we could do in a regular html document, including adding our own styling. Let's add a more complex html prompt, including a few different classes that we can reference in a css file. 

``` javascript
const trial_1 = {
    type: jsPsychAudioKeyboardResponse,
    choices: ['d', 'k'],
    stimulus: 'audio/Violin.wav',
    response_allowed_while_playing: false,
    trial_duration: 4000,
    prompt: `<div class=\"option_container\"><div class=\"option\">NEW<br><br><b>D</b></div><div class=\"option\">OLD<br><br><b>K</b></div></div>`
}
```

It can be a bit of a headache to include html content like this because of the formatting, so sometimes it's a good idea to develop layouts in a separate file and then add them here. You can also write string arguments over multiple lines by enclosing everything with the ` character rather than " or '.

As with any html file, we can link to a stylesheet to change the appearance of elements. In the current directory, create a file called `experiment.css` and then link it in the head of your html file like this:

``` html
<link href="experiment.css" rel="stylesheet" type="text/css" />
```

We won't get into details of how to use CSS right now, but go ahead and add this block to your `experiment.css` file and save. 

``` css
.option_container {
    display: flex;
    flex-direction: row;
    justify-content: space-evenly;
    align-items: center;
    width: 500px;
    height: 400px;
    border: 2px solid black;
    border-radius: 10px;
}

.option {
    font-size: 24px;
}
```

Now, when you refresh the experiment, the visual prompt should be presented in a slightly more pleasant way! You can use your linked to stylesheet to alter the appearance of your experiment in nearly limitless ways. 

#### The `data` object 

Now, back to the .js file where we're building our first trial. Each jsPsych plugin automatically records certain bits of data. Some of these are shared across plugins and some are unique to particular plugins. Both lists can be found un plugins' documentation pages. Typically, it will also be important to record data that is not included by default, for example condition codes, correct responses for individual trials, etc. 

To record this additional information, every plugin has a parameter called `data`, where we can specify what else needs to get recorded. This parameter takes an object as its argument. For example, if you wanted to record the correct response for each trial so you can compare it with the participant's actual response, you could add it like this:

``` javascript
const trial_1 = {
    type: jsPsychAudioKeyboardResponse,
    choices: ['d', 'k'],
    stimulus: 'audio/Violin.wav',
    response_allowed_while_playing: false,
    trial_duration: 4000,
    prompt: `<div class=\"option_container\"><div class=\"option\">NEW<br><br><b>D</b></div><div class=\"option\">OLD<br><br><b>K</b></div></div>`,
    data: {
        correct: "NEW"
    }
}
```

Now if we click through the experiment and look at the data page at the very end, there will be a new column added called `correct`. Note that if you add a parameter like that outside of the `data` object, it won't break the experiment, but it also won't get recorded. 

#### Adding dynamic functionality

Sometimes we want to include custom behavior that's not supported out of the box. Most trials can take functions as arguments. A good way to demonstrate this is with the `on_finish` parameter, which specifies some kind of behavior to be executed when a trial finishes. See <a href="https://www.jspsych.org/7.3/overview/dynamic-parameters/">this page</a> for more info on how this works. 

Maybe we want to evaluate the participant's response as soon as the trial is over. Let's add a function that compares the participant's actual response to the expected response, and then assigns a label to be recorded. To do that, assign the `on_finish` parameter to a function with an argument of `data`. Then use a conditional to determine the correct label. That could look like:

``` javascript
const trial_1 = {
    type: jsPsychAudioKeyboardResponse,
    choices: ['d', 'k'],
    stimulus: 'audio/Violin.wav',
    response_allowed_while_playing: false,
    trial_duration: 4000,
    prompt: `<div class=\"option_container\"><div class=\"option\">NEW<br><br><b>D</b></div><div class=\"option\">OLD<br><br><b>K</b></div></div>`,
    data: {
        correct: "NEW"
    },
    on_finish: function(data) {
        if (data.response == 'd' & data.correct == 'NEW') {
            data.result = "correct_rejection"
        } else if (data.response == 'k' & data.correct == 'NEW') {
            data.result = "false_alarm"
        } else if (data.response == 'd' & data.correct == 'OLD') {
            data.result = "miss"
        } else  {
            data.result = "hit"
        }
    }
}
```

Now our output data will have a column called `result` that includes these possible cases. 

#### Linking custom JavaScript
Our trial is starting to look pretty good, but having all that JavaScript in the `on_finish` parameter make make the file difficult to read as we start adding more trials. One solution here would be to create a separate .js file where we define this behavior as a helper function. Create a file called `util.js` and then link it to your html file. Make sure to do this *before* your `experiment.js` file. At this point, your whole html file will look something like:

``` html
<!DOCTYPE html>
<html>
    <head>
        <!-- jsPsych plugins -->
        <script src="jspsych/dist/jspsych.js"></script>
        <script src="jspsych/dist/plugin-audio-keyboard-response.js"></script>  
        <script src="jspsych/dist/plugin-html-button-response.js"></script>
        <script src="jspsych/dist/plugin-html-keyboard-response.js"></script>
        
        <!-- CSS -->
        <link href="jspsych/dist/jspsych.css" rel="stylesheet" type="text/css" />
        <link href="experiment.css" rel="stylesheet" type="text/css" />

        <!-- Other .js files -->
        <script src="util.js"></script>
        <script src="experiment.js"></script>

        <!-- This is the name that will appear on the browser tab. -->
        <title>Language Experiment</title>
    </head>
</html>
```

Now we can go into the `util.js` file and define a function with our `on_finish` functionality. Let's call it `evaluate_response`.

``` javascript
function evaluate_response(data) {
        if (data.response == 'd' & data.correct == 'NEW') {
            data.result = "correct_rejection"
        } else if (data.response == 'k' & data.correct == 'NEW') {
            data.result = "false_alarm"
        } else if (data.response == 'd' & data.correct == 'OLD') {
            data.result = "miss"
        } else  {
            data.result = "hit"
        }
    }
```

Now, back to our trial, we can change the `on_finish` parameter to call this function. 

``` javascript
const trial_1 = {
    type: jsPsychAudioKeyboardResponse,
    choices: ['d', 'k'],
    stimulus: 'audio/Violin.wav',
    response_allowed_while_playing: false,
    trial_duration: 4000,
    prompt: `<div class=\"option_container\"><div class=\"option\">NEW<br><br><b>D</b></div><div class=\"option\">OLD<br><br><b>K</b></div></div>`,
    data: {
        correct: "NEW"
    },
    on_finish: function(data) {
        evaluate_response(data);
    }
}
```

Using helper functions in a separate .js file will help enormously to keep your code clean and readable. 

#### Repeating trials
At this point, our trial is looking pretty good, so let's add some more. The most straightforward way to do this is just to copy and paste your trial object and change the necessary parameters. Let's create an experiment with four trials, where each of our audio files is heard twice. To do that, we can just change the `stimulus` path and the `correct` response. Everything else can stay the same. 

``` javascript
const trial_1 = {
    type: jsPsychAudioKeyboardResponse,
    prompt: "<div class=\"option_container\"><div class=\"option\">NEW<br><br><b>D</b></div><div class=\"option\">OLD<br><br><b>K</b></div></div>",
    choices: ["d", 'k'],
    stimulus: "audio/Violin.wav",
    trial_duration: 4000,
    response_allowed_while_playing: false,
    data: {
        correct: "NEW"
    },
    on_finish: function(data) {
        evaluate_response(data)
    }
}

const trial_2 = {
    type: jsPsychAudioKeyboardResponse,
    prompt: "<div class=\"option_container\"><div class=\"option\">NEW<br><br><b>D</b></div><div class=\"option\">OLD<br><br><b>K</b></div></div>",
    choices: ["d", 'k'],
    stimulus: "audio/Bologna.wav",
    trial_duration: 4000,
    response_allowed_while_playing: false,
    data: {
        correct: "NEW"
    },
    on_finish: function(data) {
        evaluate_response(data)
    }
}

const trial_3 = {
    type: jsPsychAudioKeyboardResponse,
    prompt: "<div class=\"option_container\"><div class=\"option\">NEW<br><br><b>D</b></div><div class=\"option\">OLD<br><br><b>K</b></div></div>",
    choices: ["d", 'k'],
    stimulus: "audio/Violin.wav",
    trial_duration: 4000,
    response_allowed_while_playing: false,
    data: {
        correct: "OLD"
    },
    on_finish: function(data) {
        evaluate_response(data)
    }
}

const trial_4 = {
    type: jsPsychAudioKeyboardResponse,
    prompt: "<div class=\"option_container\"><div class=\"option\">NEW<br><br><b>D</b></div><div class=\"option\">OLD<br><br><b>K</b></div></div>",
    choices: ["d", 'k'],
    stimulus: "audio/Bologna.wav",
    trial_duration: 4000,
    response_allowed_while_playing: false,
    data: {
        correct: "OLD"
    },
    on_finish: function(data) {
        evaluate_response(data)
    }
}

timeline.push(trial_1, trial_2, trial_3, trial_4);
```

This will work, but it will also start to get a bit clunky. One way around this may be to define all these objects in a separate file (something like `trials.js`) and then just use the `experiment.js` file to push the objects you created externally. 

Another method would be to use <a href="https://www.jspsych.org/7.3/overview/timeline/#nested-timelines">nested timelines</a> and <a href="https://www.jspsych.org/7.3/overview/timeline/#timeline-variables">timeline variables</a>. These mechanisms allow you to specify the trials' consistent parameters once, and then specify only those things that change separately. Here, that would look like:

``` javascript 
const trials = {
    type: jsPsychAudioKeyboardResponse,
    choices: ['d', 'k'],
    response_allowed_while_playing: false,
    trial_duration: 4000,
    prompt: `<div class=\"option_container\"><div class=\"option\">NEW<br><br><b>D</b></div><div class=\"option\">OLD<br><br><b>K</b></div></div>`,
    data: {
        correct: "NEW"
    },
    on_finish: function(data) {
        evaluate_response(data);
    },
    timeline: [
        {stimulus: 'audio/Violin.wav', data: {correct: "NEW"}},
        {stimulus: 'audio/Bologna.wav', data: {correct: "NEW"}},
        {stimulus: 'audio/Violin.wav', data: {correct: "OLD"}},
        {stimulus: 'audio/Bologna.wav', data: {correct: "OLD"}}
    ]
}
timeline.push(trials)
```

Using nested variables has the added benefit of making trial order <a href="https://www.jspsych.org/7.3/overview/timeline/#random-orders-of-trials">randomization</a> a bit simpler. For the rest of this tutorial though, let's stick with the clunky version. 

#### Adding trials of a different type
The current four-trial experiment is working well, but maybe we want to add some kind of inter-trial interval to keep participants from feeling overburdened. To do this, we can define one single trial that does nothing, accepts no response, and ends after 1 second no matter what. For example:

``` javascript
const ITI = {
    type: jsPsychHtmlKeyboardResponse,
    choices: [""],
    stimulus: "",
    response_ends_trial: false,
    trial_duration: 1000
}
```

We want this trial to be presented after every single keyboard response trial, but we only need to define it once. To prevent ourselves from having to manually push it to the timeline every time, we can put our keyboard response trials in an array and iterate over it with a `for` loop, pushing a trial and an ITI on each pass. 

``` javascript
const trial_array = [trial_1, trial_2, trial_3, trial_4];
for (let i = 0; i < trial_array.length; i++) {
    timeline.push(trial_array[i], ITI);
}
```

(If you're working with nested timelines see <a href="https://www.jspsych.org/7.3/overview/timeline/#timeline-variables">timeline variables</a> for a way to do this with fewer lines of code.)

#### Preloading media
In default javascript behavior, the stimulus file associated with each trial is loaded to the user's local memory when it's called up. This introduces a delay that can negatively affect the flow of the experiment, the duration of the ITI, and RT data! A sloution to this problem is to preload all the necessary media *before* the procedure begins. This will be particularly important if you're working with large files like audio or images. 

In jsPsych, there's a separate plugin to handle preloading media, called <a href="https://www.jspsych.org/7.3/plugins/preload/">preload</a>. Add that plugin to the head of your html file:

```html
<script src="jspsych/dist/plugin-preload.js"></script>
```

The plugin uses different parameters to load each type of media, so we want to use the `audio` parameter, and give it an array including paths to all of our audio files. We want this "trial" to occur first in the timeline, but it usually gets defined at the end of development. Either go back to the beginning of the file and add it there, or use the `.unshift()` method to push it to the beginning of the `timeline` array. 

``` javascript 
const preload_array = ['audio/Bologna.wav', 'audio/Violin.wav'];
const preload_trial = {
    type: jsPsychPreload,
    audio: preload_array
};

timeline.unshift(preload_trial);
```


#### Surveys

That should just about do it for the main body of our experiment. It's generally a good idea though to collect a little bit of demographic information from participants. Let's add a trial to let them know that the experiment is done and ask them to fill out the questionnaire. 

``` javascript
const quest_intstructions = {
    type: jsPsychHtmlButtonResponse,
    choices: ['Continue'],
    stimulus: "That's the end of the experiment! Thank you for your responses. To help us analyze our results, it would be helpful to know know a little more about you. Please answer the following questions. <br><br>"
}
```

There are two main ways to add a survey. The traditional way is to use the <a href="https://www.jspsych.org/7.3/plugins/survey-html-form/">survey html form</a> plugin, where you basically just write an entire survey in html and supply that as the stimulus. 

There's also a new plugin called <a href="https://www.jspsych.org/7.3/plugins/survey/">survey</a>, which streamlines a lot of the development process. In the complete sample experiment on github, both implementations are included, but let's focus on the new Survey plugin here. 

This plugin uses its own independent stylesheet, so we need to load that along with the plugin itself. Frustratingly, this stylesheet is not included in the downloadable distribution, so we need to link to the CDN-hosted version. Add both links to the head of the html file

``` html 
<script src="jspsych/dist/plugin-survey.js"></script>
<link rel="stylesheet" href="https://unpkg.com/@jspsych/plugin-survey@0.2.1/css/survey.css">
```

Now we can build the survey itself. The main parameter is called `pages`. which takes an array of arrays as an argument. Each of the smaller arrays represents one page of the survey and contains one object per question. You can think of each of these objects as a mini-jsPsych trial. All of the options for these trials are laid out in the documentation. 

We might want to start with some kind of header with general instructions. For that we can use the `html` type. Let's make a `survey` trial with that as our first question.

``` javascript 
const questionnaire = {
    type: jsPsychSurvey,
    pages: [
        [
            {
                type: 'html',
                prompt: 'Please answer the following questions.' 
            }
        ]
    ]
};

timeline.push(questionnaire)
```

Now, to add more questions, we just add more objects to the inner array. Maybe we want a multiple choice question to see if participants felt like they understood the task. For this, we can use the `multi-choice` type, with an `options` array containing the possible answers. We should also give the question a `name`, which is how the question will be identified in the data output. 

``` javascript 
const questionnaire = {
    type: jsPsychSurvey,
    pages: [
        [
            {
                type: 'html',
                prompt: 'Please answer the following questions.' 
            },
            {
                type: 'multi-choice',
                prompt: 'Did you read the instructions and do you think you did the task correctly?', 
                name: 'correct', 
                options: ['Yes', 'No', 'I was confused']
            }
        ]
    ]
};

timeline.push(questionnaire)
```

We might then want to ask about the participant's gender using a drop-down menu. The syntax for this is similar:

``` javascript 
const questionnaire = {
    type: jsPsychSurvey,
    pages: [
        [
            {
                type: 'html',
                prompt: 'Please answer the following questions.' 
            },
            {
                type: 'multi-choice',
                prompt: 'Did you read the instructions and do you think you did the task correctly?', 
                name: 'correct', 
                options: ['Yes', 'No', 'I was confused']
            }
        ],
        [
            {
                type: 'drop-down',
                prompt: 'Gender:',
                name: 'gender',
                options: ['Female', 'Male', 'Non-binary/Non-conforming', 'Other']
            }
        ]
    ]
};

timeline.push(questionnaire)
```

Here's some code for a full survey: 

``` javascript 
const questionnaire = {
    type: jsPsychSurvey,
    pages: [
        [
            {
                type: 'html',
                prompt: "Please answer the following questions:"
            },
            {
                type: 'multi-choice',
                prompt: 'Did you read the instructions and do you think you did the task correctly?', 
                name: 'correct', 
                options: ['Yes', 'No', 'I was confused']
            },
            {
                type: 'drop-down',
                prompt: 'Gender:',
                name: 'gender',
                options: ['Female', 'Male', 'Non-binary/Non-conforming', 'Other']
            },
            {
                type: 'text',
                prompt: 'Age:',
                name: 'age',
                textbox_columns: 10
            },
            {
                type: 'drop-down',
                prompt: 'Level of education:',
                name: 'education',
                options: ['Some high school', 'Graduated high school', 'Some college', 'Graduated college', 'Hold a higher degree']
            },
            {
                type: 'text',
                prompt: "Native language? (What was the language spoken at home when you were growing up?)",
                name: 'language',
                textbox_columns: 20
            },
            {
                type: 'drop-down',
                prompt: 'Do you think the payment was fair?',
                name: 'payment',
                options: ['The payment was too low', 'The payment was fair']
            },
            {
                type: 'drop-down',
                prompt: 'Did you enjoy the experiment?',
                name: 'enjoy',
                options: ['Worse than the average experiment', 'An average experiment', 'Better than the average experiment']
            },
            {
                type: 'text',
                prompt: "Do you have any other comments about this experiment?",
                name: 'comments',
                textbox_columns: 30,
                textbox_rows: 4
            }
        ]
    ]
};
timeline.push(questionnaire)
```

#### Wrapping up

The very last thing to do is to thank the participant for their time! Just add one more trial.

``` javascript 
const thanks = {
    type: jsPsychHtmlButtonResponse,
    choices: ['Continue'],
    stimulus: "Thank you for your time! Please click 'Continue' and then wait a moment until you're directed back to Prolific.<br><br>"
}
```

Congratulations! You just made your first fully functional experiment with jsPsych! However, we still need to add some tools to integrate with Prolific and to make sure your data winds up in a database rather than disappearing into the ether. More on that next week.