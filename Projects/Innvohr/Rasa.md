![[Pasted image 20241221015837.png]]## **Detailed Explanation of Rasa**


### 1. **Rasa Overview**

Rasa is designed to make chatbots capable of understanding natural language and maintaining conversational context. It achieves this with two main components:

- **Rasa NLU (Natural Language Understanding)**: Identifies **intents** (user's purpose) and extracts **entities** (specific pieces of information).
- **Rasa Core**: Manages the flow of conversations, deciding how the bot should respond based on the context.

---

### 2. **Setting Up Rasa**

#### Step 1: **Installation**

- Create a virtual environment for Rasa to avoid dependency conflicts:
    
    ```bash
    python3 -m venv rasa_env
    source rasa_env/bin/activate  # Linux/MacOS
    rasa_env\Scripts\activate     # Windows
    ```
    
- Install Rasa:
    
    ```bash
    pip install rasa
    ```
    

---

### 3. **Creating a New Rasa Project**

Run:

```bash
rasa init
```

This creates a directory structure like:

```
my_project/
├── actions/               # For custom Python actions
├── data/                  # Contains training data
│   ├── nlu.yml            # Examples for intents and entities
│   ├── stories.yml        # Defines conversation examples
├── domain.yml             # Configures intents, entities, slots, actions, etc.
├── config.yml             # Machine learning pipeline configuration
├── endpoints.yml          # Configuration for external services (e.g., custom actions)
├── credentials.yml        # Connectors for messaging platforms (e.g., Telegram, Slack)
```

---

### 4. **Understanding Key Rasa Files**

#### 1. **`nlu.yml`**: Intent and Entity Training Data

This file contains example sentences for each **intent**. An **intent** represents the user's goal or purpose in sending a message.

Example:

```yaml
version: "3.0"
nlu:
  - intent: greet
    examples: |
      - hello
      - hi
      - hey
      - good morning

  - intent: ask_weather
    examples: |
      - What's the weather like today?
      - Tell me the weather in [London](location)
      - Is it raining in [Paris](location)?
```

**Key Concepts**:

- **Intent**: The goal of the user's message (e.g., `greet`, `ask_weather`).
- **Entities**: Specific details in the user's message, such as `location` (`London`, `Paris`).

---

#### 2. **`stories.yml`**: Define Conversation Paths

This file describes how conversations should flow.

Example:

```yaml
version: "3.0"
stories:
  - story: greet and ask weather
    steps:
      - intent: greet
      - action: utter_greet
      - intent: ask_weather
      - action: action_weather
```

**Key Concepts**:

- **Steps**: Define a sequence of intents and actions.
- **Actions**: Include predefined responses (`utter_...`) or custom Python actions.

---

#### 3. **`domain.yml`**: Define the Bot's Knowledge

This file combines intents, entities, slots, responses, and actions.

Example:

```yaml
version: "3.0"
intents:
  - greet
  - ask_weather

entities:
  - location

responses:
  utter_greet:
    - text: "Hello! How can I assist you?"

  utter_goodbye:
    - text: "Goodbye! Have a great day!"

actions:
  - action_weather
```

**Key Concepts**:

- **Intents**: User goals.
- **Entities**: Extracted details.
- **Responses**: Predefined messages.
- **Actions**: Can include dynamic behavior implemented in Python.

---

#### 4. **`actions.py`**: Define Custom Actions

For more complex responses, you can write Python code in `actions.py`. This is used for dynamic replies like querying databases or APIs.

Example:

```python
from rasa_sdk import Action, Tracker
from rasa_sdk.executor import CollectingDispatcher

class ActionWeather(Action):
    def name(self) -> str:
        return "action_weather"

    def run(self, dispatcher: CollectingDispatcher,
            tracker: Tracker,
            domain: Dict) -> List[Dict]:
        location = tracker.get_slot("location")
        weather = "sunny"  # Replace with API call
        dispatcher.utter_message(text=f"The weather in {location} is {weather}.")
        return []
```

**Explanation**:

1. **`dispatcher`**: Sends messages to the user.
2. **`tracker`**: Tracks conversation context, including user intents and slots.
3. **`return`**: Can update slots or trigger follow-up actions.

---

#### 5. **`config.yml`**: Machine Learning Pipeline

Defines how intents and entities are detected.

Example:

```yaml
pipeline:
  - name: "WhitespaceTokenizer"
  - name: "CountVectorsFeaturizer"
  - name: "DIETClassifier"

policies:
  - name: "RulePolicy"
  - name: "TEDPolicy"
```

---
# # Entity Extraction 

![[Pasted image 20241221141551.png]]
![[Pasted image 20241221142356.png]]

# Synonyms
![[Pasted image 20241221142816.png]]
OR 
![[Pasted image 20241221142831.png]]
#Note: synonym mapping happens after entities are extracted.

# Lookup tables
![[Pasted image 20241221143048.png]]

# Entity Role
![[Pasted image 20241221143222.png]]

# Slots
![[Pasted image 20241221143429.png]]
![[Pasted image 20241221143450.png]]
![[Pasted image 20241221143555.png]]
![[Pasted image 20241221143806.png]]
![[Pasted image 20241221143939.png]]
![[Pasted image 20241221144001.png]]
# Slot Mappings
![[Pasted image 20241221144438.png]]
![[Pasted image 20241221144508.png]]
![[Pasted image 20241221144750.png]]

# Slot Type
![[Pasted image 20241221145017.png]]
![[Pasted image 20241221145033.png]]
![[Pasted image 20241221145111.png]]
![[Pasted image 20241221145229.png]]
![[Pasted image 20241221145252.png]]


# Responses
![[Pasted image 20241221155515.png]]
 Using Variables 
 ![[Pasted image 20241221160006.png]]
##### Adding Buttons
![[Pasted image 20241221161848.png]]

CUSTOM PAYLOAD
![[Pasted image 20241221162555.png]]




### 5. **Training the Model**

Train your model using:

```bash
rasa train
```

This creates a `models/` directory containing the trained model.

---

### 6. **Running the Chatbot**

Run your bot interactively:

```bash
rasa shell
```

You can also start the action server for custom actions:

```bash
rasa run actions
```

---

### 7. **Key Features of Rasa**

#### **1. Slots**

Slots store data for the current conversation (e.g., a user's name or location).

Example (`domain.yml`):

```yaml
slots:
  location:
    type: text
```

#### **2. Forms**

Forms help collect multiple pieces of information in a structured way.

Example:

```yaml
forms:
  weather_form:
    required_slots:
      location:
        - type: from_entity
          entity: location
```

#### **3. Interactive Learning**

Improve your bot by correcting its behavior interactively:

```bash
rasa interactive
```

---

### 8. **Deployment**

Deploy Rasa on a cloud server or connect it to platforms like Slack, Telegram, or a custom frontend.

#### Example: Adding a Connector

Configure `credentials.yml` to connect to a messaging platform:

```yaml
telegram:
  access_token: "YOUR_ACCESS_TOKEN"
  verify: "YOUR_BOT_USERNAME"
  webhook_url: "YOUR_WEBHOOK_URL"
```

Start the bot with:

```bash
rasa run --enable-api
```

---

### **9. Advanced Features of Rasa**

#### **1. Slots in Detail**

Slots store specific pieces of information about the user or conversation. You can use slots to tailor responses or guide the conversation flow.

**Example: Slot Declaration** In `domain.yml`:

```yaml
slots:
  location:
    type: text
    influence_conversation: true
```

- **Types of Slots**:
    
    - `text`: Stores text input.
    - `categorical`: Stores predefined categories.
    - `bool`: Stores `True` or `False`.
    - `list`: Stores lists of items.
    - `float`: Stores numeric values.
- **Auto-Filling Slots**: Slots can automatically extract values from entities using the `from_entity` mapping.
    

Example (`forms`):

```yaml
forms:
  restaurant_form:
    required_slots:
      cuisine:
        - type: from_entity
          entity: cuisine
```

---

#### **2. Forms**

Forms are used to guide users through a series of questions to collect multiple pieces of information.

**Example: Collecting Information for Booking** Define the form in `domain.yml`:

```yaml
forms:
  booking_form:
    required_slots:
      date:
        - type: from_text
      time:
        - type: from_text
      party_size:
        - type: from_text
```

Add the form to a story in `stories.yml`:

```yaml
stories:
  - story: book a table
    steps:
      - intent: book_table
      - action: booking_form
      - active_loop: booking_form
      - action: utter_submit
```

Implement custom validation for slots in `actions.py`:

```python
from rasa_sdk import Action, Tracker
from rasa_sdk.events import SlotSet
from rasa_sdk.forms import FormValidationAction

class ValidateBookingForm(FormValidationAction):
    def name(self) -> str:
        return "validate_booking_form"

    def validate_date(self, slot_value, dispatcher, tracker, domain):
        if valid_date(slot_value):
            return {"date": slot_value}
        else:
            dispatcher.utter_message(text="Invalid date format. Please try again.")
            return {"date": None}
```

---

#### **3. Custom Components**

Rasa allows you to create custom NLU components to extend its functionality. This is useful for tasks like sentiment analysis or custom entity extraction.

**Example: Adding a Sentiment Analyzer**

1. Create a file `sentiment_analyzer.py`:
    
    ```python
    from rasa.nlu.components import Component
    from typing import Any, Optional, Text, Dict, List
    
    class SentimentAnalyzer(Component):
        name = "sentiment_analyzer"
        provides = ["sentiment"]
        requires = ["text"]
    
        def process(self, message, **kwargs):
            sentiment = analyze_sentiment(message.text)  # Implement this
            message.set("sentiment", sentiment, add_to_output=True)
    ```
    
2. Add the component to `config.yml`:
    
    ```yaml
    pipeline:
      - name: "WhitespaceTokenizer"
      - name: "SentimentAnalyzer"
    ```
    

---

#### **4. Policies**

Policies decide how the bot should behave during a conversation. Rasa includes multiple policies:

- **RulePolicy**: Follows strict rules defined in stories.
- **TEDPolicy**: Predicts actions based on the entire conversation context.
- **FallbackPolicy**: Handles unexpected user inputs.

**Example: Setting a Fallback Policy** Add this to `config.yml`:

```yaml
policies:
  - name: "RulePolicy"
    core_fallback_threshold: 0.3
    core_fallback_action_name: "action_default_fallback"
```

Define the fallback action in `domain.yml`:

```yaml
actions:
  - action_default_fallback

responses:
  utter_default:
    - text: "I'm sorry, I didn't understand that. Can you please rephrase?"
```

---

### **10. Connecting Rasa to External APIs**

#### **Example: Fetching Weather Information**

In `actions.py`:

```python
import requests
from rasa_sdk import Action

class ActionFetchWeather(Action):
    def name(self) -> str:
        return "action_fetch_weather"

    def run(self, dispatcher, tracker, domain):
        location = tracker.get_slot("location")
        if location:
            response = requests.get(f"http://api.weatherapi.com/v1/current.json?key=YOUR_API_KEY&q={location}")
            data = response.json()
            weather = data["current"]["condition"]["text"]
            temperature = data["current"]["temp_c"]
            dispatcher.utter_message(text=f"The weather in {location} is {weather} with a temperature of {temperature}°C.")
        else:
            dispatcher.utter_message(text="I need to know your location to fetch the weather.")
        return []
```

---

### **11. Deployment and Scaling**

1. **Local Deployment**:
    
    - Run Rasa using:
        
        ```bash
        rasa run --enable-api
        ```
        
2. **Docker Deployment**:
    
    - Use the official Rasa Docker images to containerize your chatbot.
    
    Example `docker-compose.yml`:
    
    ```yaml
    version: "3.0"
    services:
      rasa:
        image: rasa/rasa:latest
        ports:
          - "5005:5005"
        volumes:
          - "./models:/app/models"
          - "./actions:/app/actions"
        command:
          - run
      action_server:
        image: rasa/rasa-sdk:latest
        ports:
          - "5055:5055"
        volumes:
          - "./actions:/app/actions"
    ```
    
3. **Cloud Deployment**:
    
    - Use platforms like AWS, Google Cloud, or Heroku to host your bot.
    - Set up a webhook to connect with messaging platforms (e.g., Telegram, Slack).

---

### **12. Debugging and Improving the Bot**

1. **Interactive Learning**: Refine your bot's responses by running:
    
    ```bash
    rasa interactive
    ```
    
2. **Logging**: Add debug logs to monitor the bot’s behavior:
    
    ```bash
    rasa shell --debug
    ```
    
3. **Review Conversations**: Use Rasa X to collect and annotate real conversations.
    

---

# Folder structure :

#### **1. Directory Structure**

Organize your actions into a dedicated directory structure. For example:

```
actions/
├── __init__.py
├── action_greet.py
├── action_fetch_weather.py
├── validations/
│   ├── __init__.py
│   └── booking_validation.py
```

- Each `.py` file will contain related actions or validation logic.
- The `__init__.py` files are necessary to make these directories Python modules.

---

#### **2. Move Each Action to a Separate File**

Create separate files for related actions.

For example, move the `ActionGreet` action to `action_greet.py`:

```python
# actions/action_greet.py
import random
from typing import Any, Text, Dict, List
from rasa_sdk import Action, Tracker
from rasa_sdk.executor import CollectingDispatcher

class ActionGreet(Action):
    def name(self) -> Text:
        return "action_greet"

    def run(self, dispatcher: CollectingDispatcher,
            tracker: Tracker,
            domain: Dict[Text, Any]) -> List[Dict[Text, Any]]:

        messages = [
            "Hi there. 👋😃 It's such a pleasure to have you here. How may I help you?",
            "Hello, 🤗 how can we assist you?"
        ]
        buttons = [
            {"payload": '/select_option{"option": "selected_hr_policy"}', "title": "HR Policy"}
        ]
        reply = random.choice(messages)
        dispatcher.utter_message(text=reply, buttons=buttons)
        return []
```

Move the `ActionFetchWeather` action to `action_fetch_weather.py`:

```python
# actions/action_fetch_weather.py
import requests
from rasa_sdk import Action

class ActionFetchWeather(Action):
    def name(self) -> Text:
        return "action_fetch_weather"

    def run(self, dispatcher, tracker, domain):
        location = tracker.get_slot("location")
        if location:
            response = requests.get(f"http://api.weatherapi.com/v1/current.json?key=YOUR_API_KEY&q={location}")
            data = response.json()
            weather = data["current"]["condition"]["text"]
            temperature = data["current"]["temp_c"]
            dispatcher.utter_message(text=f"The weather in {location} is {weather} with a temperature of {temperature}°C.")
        else:
            dispatcher.utter_message(text="I need to know your location to fetch the weather.")
        return []
```

---

#### **3. Update the `__init__.py` File**

The `__init__.py` file allows you to aggregate and expose all actions in one place for Rasa to discover them.

In `actions/__init__.py`:

```python
from actions.action_greet import ActionGreet
from actions.action_fetch_weather import ActionFetchWeather

# Add other imports as needed
```

---

#### **4. Configure the `endpoints.yml` File**

In your `endpoints.yml`, ensure the action server is pointing to your module correctly:

```yaml
action_endpoint:
  url: "http://localhost:5055/webhook"
```

The action server automatically discovers all actions in the project directory (including submodules) if `__init__.py` is set up correctly.

---

#### **5. Run Your Rasa Bot**

Start the Rasa action server:

```bash
rasa run actions
```

If everything is set up correctly, your modularized actions should work seamlessly.

---

### **Benefits of Splitting Actions**

1. **Modularity**: Each file handles related actions, making it easier to locate and edit code.
2. **Reusability**: Common functionalities can be abstracted into helper modules.
3. **Scalability**: Adding new actions doesn’t clutter existing files.

---

### **Advanced: Using Helper Utilities**

For common logic shared by multiple actions, create a helper utility file.

For example, a `utils.py`:

```python
# actions/utils.py
def format_weather_response(data):
    weather = data["current"]["condition"]["text"]
    temperature = data["current"]["temp_c"]
    return f"The weather is {weather} with a temperature of {temperature}°C."
```

You can then use it in any action:

```python
from actions.utils import format_weather_response

class ActionFetchWeather(Action):
    def run(self, dispatcher, tracker, domain):
        # Use the helper function
        ...
        reply = format_weather_response(data)
        dispatcher.utter_message(text=reply)
```

---
