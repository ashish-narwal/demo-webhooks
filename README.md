# demo-webhooks

Example showing how to connect your NLP models to [tagtog](https://www.tagtog.net). We will use Python, [Flask](https://flask.palletsprojects.com/), [spaCy](https://spacy.io), [tagtog webhooks](https://docs.tagtog.net/projects.html#webhooks) and [tagtog API](https://docs.tagtog.net/API_documents_v1.html). 

An introduction to webhooks and the full step-by-step guide is here: https://tagtog.medium.com/connect-your-nlp-models-to-tagtog-using-webhooks-13d422ae4dff

With this repo, you will learn how to setup a tagtog webhook and how to manage an event notification generated by the webhook:

1. Upon document upload to tagtog, to get the document's content from tagtog using the document ID.
2. To annotate the document text using the spaCy model [`en_core_web_sm`](https://spacy.io/models/en#en_core_web_sm) and to transform the spaCy annotations into the [ann.json](https://docs.tagtog.net/anndoc.html#ann-json) format.
3. To push the annotated document back to tagtog

## Setup and running
1. Create a virtual environment
```shell
# Create the virtual environment myenv
# Python 3.3+
$ python3 -m venv myenv

# Activate the virtualenv (OS X & Linux)
$ source myenv/bin/activate
```
2. Install the dependencies
```
$ pip install -r requirements.txt
```
3. Download the spaCy model
```
python3 -m spacy download en_core_web_sm
```
4. Setup the environment variable
```
export MY_TAGTOG_USERNAME='your_username'
export MY_TAGTOG_PASSWORD='your_password'
export MY_TAGTOG_PROJECT='project_name'
```
5. Run the app
```
$ python3 app.py
```
6. Make your app reachable from the outside using [ngrok](https://ngrok.com/)
```
$ ./ngrok http 5000
```
7. Create a project at [tagtog.net](https://www.tagtog.net)
8. Go to your project and create three entity types at Settings > Entity Types: `PERSON`, `ORG` and `MONEY`
9. Add a webhook to your project at Settings > Webhooks:
  * Endpoint: Use the endpoint given by ngrok (e.g. http://1cbc12c59c8d.ngrok.io)
  * Payload: Choose the payload `tagtogID`
  * Check the flag to `Trigger only if change originates in the GUI`
  * Authentication: we won't use any authentication mechanism for this example, therefore we choose none.
10. Upload a document to your tagtog project (e.g. "Paypal Holdings Inc (PYPL) President and CEO Daniel Schulman Sold $2.7 million of Shares"). 🪄 The document is automatically annotated by our spaCy model.
