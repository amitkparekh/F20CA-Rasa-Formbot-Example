# Formbot

The `formbot` example is designed to help you understand how the `FormAction` works and how to implement it in practice. Using the code and data files in this directory, you can build a simple restaurant search assistant capable of recommending restaurants based on user preferences.

> **Note**
>
> This example has been modified to make it easier to run things for the F20CA course at Heriot-Watt University.

## What's inside this example?

This example contains some training data and the main files needed to build an assistant on your local machine. The `formbot` consists of the following files:

- **data/nlu.yml** contains training examples for the NLU model
- **data/stories.yml** contains training stories for the Core model
- **actions/actions.py** contains the implementation of a custom `FormAction`
- **config.yml** contains the model configuration
- **domain.yml** contains the domain of the assistant
- **endpoints.yml** contains the webhook configuration for the custom actions

## How to use this example?

### Installing things to get ready

We have verified this works with the following tools, and this is also what you will need:

- Python 3.9 *or* Python 3.10 — which you use is up to you. If you have issues with one, try the other.
- [Docker](https://www.docker.com/)

> **Warning**
>
> We are presuming that you already know how to install Python, and know how to search for how to install Docker.

#### Create a virtual environment for your Python

If you are unsure how to create a virtual environment for your Python, or the below instructions don't work for your system, check out [these instructions from RealPython](https://realpython.com/python-virtual-environments-a-primer/#how-can-you-work-with-a-python-virtual-environment).

Run the following commands **with your python executable**.

<details>
<summary>Commands for Mac OS/Linux users</summary>

```bash
<YOUR PYTHON EXECUTABLE> -m venv .venv
source .venv/bin/activate
```
</details>

<details>
<summary>Commands for PowerShell</summary>

> **Note**
>
> These commands are untested.

```powershell
<YOUR PYTHON EXECUTABLE> -m venv .venv
.venv\Scripts\activate
```
</details>

#### Install dependencies

Run the following to install the dependencies:

```bash
pip install -U pip wheel
pip install rasa
```

### Running things

Using this example you can build an actual assistant which demonstrates the functionality of the `FormAction`. You can test the example using the following steps:

1. Train a Rasa model containing the Rasa NLU and Rasa Core models by running:
    ```
    rasa train
    ```
    The model will be stored in the `models/` directory as a zipped file.

2. Verify your Rasa model by evaluating it on the test stories by running:
    ```
    rasa test --stories tests/
    ```
    If your model does not perform well, you can change various components and hyperparameters by editing the `config.yaml` file. [Rasa has more details on what components are available and what can be adjusted](https://rasa.com/docs/rasa/model-configuration).

3. Run an instance of [duckling](https://rasa.com/docs/rasa/nlu/components/#ducklingentityextractor) on port `8000` by running the docker command:
   ```
   docker run -p 8000:8000 rasa/duckling
   ```
    If you can't use docker, or are having issues with running the duckling server, you can [install duckling on your machine](https://github.com/facebook/duckling#requirements).

1. Create a new terminal session and run the actions server with:
    ```
    rasa run actions
    ```

2. Create a new terminal session (should be your 3rd) and test the assistant by running:
    ```
    rasa shell -m models --endpoints endpoints.yml
    ```
    This will load the assistant in your command line for you to chat.

    > **Note**
    >
    > Depending on your operating system, you may or may not get a bunch of warnings. Don't worry about those, just keep waiting for Rasa to load the bot and ask you for an input.

You can stop the various servers and re-train and restart them at any time. For more information about the individual commands, please check out [Rasa's documentation](http://rasa.com/docs/rasa/command-line-interface).

## What do you do now?

Now everything is running, it's time for the main task.

### 0. Figure out what you are working with

Now that the bot is working, play with it and explore the code to figure out what it can do. Try to determine answers to the following questions:

- What tasks can it do? What slots does it support and what possible values do those slots take?
- Does it has mixed initiative?
- Does it handle uncertainty? If so, how?
- Does it track user goals?

### 1. Adding more slots to the bot

Time to make the bot do more!

#### 1.1. Add a new slot value to the bot

Let's start simple, add support for additional cuisines to the bot.

#### 1.2. Add new slot types

Slightly more difficult, add some new slot types to the bot that the user can provide a preference for.

For example, provide people the option to query for a child-friendly or pet-friendly location, and let people provide a desired minimum rating for the restaurant.

### 2. Add a global help function

If the user is not sure what the bot can do, they might struggle to start interacting with the bot. So next, include a new intent that will help the user if they say things like "What can I do?" or "Help".

### 3. Output an API call which represent the user's goals

Let's pretend that this bot is going to be used in production. This means that at the end of the interaction, the bot will need to make an API call to some other service to perform the restaurant booking.

For this task, create a custom action which converts the slots into a *pretend* API call and print it to the command line.

> **Note**
>
> The API call does not need to be to a real endpoint. Just pretend that you are sending the output of the form as JSON to some API.

### 4. Add clarification questions to get more precise answers out of the user

Let's say that the user isn't sure what type of restaurant they want, so they'd like you to surprise them by giving them some options from their chosen region of the world. For exmaple, if they want something from Europe, you can offer them cuisines from France, Italy, Greek, or any others.

This is just one example; the point of this task is to gather more information from the user to better serve their needs of booking a restaurant.

For this task, we recommend adding more training data and intents to the `data/`, and creating new stories within `tests/test_stories.yml` to automatically test the capabilities of your bot without needing to talk to it manually every time.
