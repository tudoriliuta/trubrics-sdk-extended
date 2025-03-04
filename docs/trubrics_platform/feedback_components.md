# ✏️ Feedback components

The main purpose of the trubrics-sdk is to provide AI teams with UI widgets and an API to save user feedback from within their apps to [Trubrics](https://trubrics.streamlit.app/). All feedback is organised into [feedback components](#create-a-feedback-component), that are created from within the platform. Each component collects a unique type of user feedback.

## Create a feedback component
In the \`✏️ Feedback components\` page, the tab `Create a component` allows you to create a feedback component with a specific [type](#types-of-feedback). To help you determine what type of feedback to use, there is a generated code snippet of the component to include into your AI application, and a visual preview of the UI component.

!!!tip
    Trubrics currently supports user feedback collection from python environments. [Contact us](https://trubrics.com/contact-us/) to discuss all other options.

### Types of feedback
Each feedback response in a component must be of a particular type, as seen in the `response` field of the Feedback data object.

!!!note "Feedback object"
    :::trubrics.feedback.dataclass.Feedback
There are three out-of-the-box types of feedback:

- `thumbs` feedback (👍, 👎), with an optional open text box
- `faces` feedback (😞, 🙁, 😐, 🙂, 😀), with an optional open text box
- `textbox` feedback, an open text box for purely qualitative feedback

To save custom feedback with multiple fields, such as collecting survey responses, users can make use of the Feedback `metadata` field.

## Saving feedback to Trubrics
Upon creating a feedback component in [Trubrics](https://trubrics.streamlit.app/), a code snippet is generated for users to incorporate quickly into their apps. There are several ways to save feedback:

### 1. With the Python SDK

Set [Trubrics](https://trubrics.streamlit.app/) `email` and `password` as environment variables:

```bash
export TRUBRICS_EMAIL="trubrics_email"
export TRUBRICS_PASSWORD="trubrics_password"
```

and push some feedback to the `default` feedback component:

```python
import os
import trubrics

config = trubrics.init(
    email=os.environ["TRUBRICS_EMAIL"],  # read your Trubrics secrets from environment variables
    password=os.environ["TRUBRICS_PASSWORD"]
)

feedback = trubrics.collect(
    component_name="default",
    model="default_model",
    response={
        "type": "thumbs",
        "score": "👎",
        "text": "A comment / textual feedback from the user."
    },
)

trubrics.save(config, feedback)
```

### 2. With Streamlit
Trubrics has an out-of-the-box [integration with Streamlit](../integrations/streamlit.md):

```console
pip install "trubrics[streamlit]"
```

```python
import streamlit as st
from trubrics.integrations.streamlit import FeedbackCollector

collector = FeedbackCollector(
    component_name="default",
    email=st.secrets["TRUBRICS_EMAIL"], # Store your Trubrics credentials in st.secrets:
    password=st.secrets["TRUBRICS_PASSWORD"], # https://blog.streamlit.io/secrets-in-sharing-apps/
)

collector.st_feedback(
    feedback_type="thumbs",
    model="your_model_name",
    open_feedback_label="[Optional] Provide additional feedback",
)
```

Take a look at our [demo LLM app](https://trubrics-llm-example.streamlit.app/) for an example.


### 3. With Flask

[Here](../integrations/flask_example.md) is an example of how the python SDK can be used with a Flask app.
