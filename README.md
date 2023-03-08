# Formbot

The `formbot` example is designed to help you understand how the `FormAction` works and how
to implement it in practice. Using the code and data files in this directory, you
can build a simple restaurant search assistant capable of recommending
restaurants based on user preferences.

> **Note**
>
> This example has been modified to make it easier to run things for the F20CA course at Heriot-Watt University.

## What's inside this example?

This example contains some training data and the main files needed to build an
assistant on your local machine. The `formbot` consists of the following files:

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
> We are presuming that you already know how to install Python 3.10, and know how to search for how to install Docker.

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

Using this example you can build an actual assistant which demonstrates the
functionality of the `FormAction`. You can test the example using the following
steps:

1. Train a Rasa model containing the Rasa NLU and Rasa Core models by running:
    ```
    rasa train
    ```
    The model will be stored in the `/models` directory as a zipped file.

2. Run an instance of [duckling](https://rasa.com/docs/rasa/nlu/components/#ducklingentityextractor)
   on port 8000 by either running the docker command
   ```
   docker run -p 8000:8000 rasa/duckling
   ```
   or [installing duckling](https://github.com/facebook/duckling#requirements) directly on your machine and starting the server.

3. Test the assistant by running:
    ```
    rasa run actions&
    rasa shell -m models --endpoints endpoints.yml
    ```
    This will load the assistant in your command line for you to chat.

For more information about the individual commands, please check out our
[documentation](http://rasa.com/docs/rasa/command-line-interface).

## Encountered any issues?
Let us know about it by posting on [Rasa Community Forum](https://forum.rasa.com)!
