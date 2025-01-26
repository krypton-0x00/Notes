

## Functions

### prompt_engineering(promptt)

Responsible for refining the input prompt.

//cohere api code
```python
import cohere

co = cohere.ClientV2("<<apiKey>>")
response = co.chat(
    model="command-r-plus", 
    messages=[{"role": "user", "content": "hello world!"}]
)

```

### hex_to_rgb(hex_color)

Converts a hex color code to an RGB tuple.

### get_policy_from_db(table_name, attribute_name, value)

Fetches a policy from the database based on the specified table name, attribute, and value.

## Classes
1. **ActionGreet**
    
    - **Description**: This class is responsible for greeting the user. It defines the logic that will be executed when the chatbot needs to initiate a conversation or respond to a greeting.
    - **Methods**:
        - `name(self)`: Returns the name of the action.
        - `run(self, dispatcher, tracker, domain)`: Executes the action logic to greet the user.
2. **ActionCompanyName**
    
    - **Description**: This class retrieves the company name from the context or database. It is used when the chatbot needs to provide information about the company.
    - **Methods**:
        - `name(self)`: Returns the name of the action.
        - `run(self, dispatcher, tracker, domain)`: Executes the action logic to retrieve the company name.
3. **ActionSetCompanyName**
    
    - **Description**: This class allows the chatbot to set or update the company name based on user input or other interactions. It helps in maintaining context during conversations.
    - **Methods**:
        - `name(self)`: Returns the name of the action.
        - `run(self, dispatcher, tracker, domain)`: Executes the action logic to set the company name.
4. **ActionHrPolicy**
    
    - **Description**: This class handles actions related to HR policies. It may retrieve, display, or manage HR policy information based on user queries.
    - **Methods**:
        - `name(self)`: Returns the name of the action.
        - `run(self, dispatcher, tracker, domain)`: Executes the action logic related to HR policies.
5. **ActionPolicyType**
    
    - **Description**: This class manages different types of policies that may be relevant to the user. It helps the chatbot understand and respond to queries regarding specific policy types.
    - **Methods**:
        - `name(self)`: Returns the name of the action.
        - `run(self, dispatcher, tracker, domain)`: Executes the action logic to handle different policy types.
6. **ActionSelectFlexibleWorkOption**
    
    - **Description**: This class allows users to select flexible work options. It may present choices to the user and process their selections accordingly.
    - **Methods**:
        - `name(self)`: Returns the name of the action.
        - `run(self, dispatcher, tracker, domain)`: Executes the action logic to select flexible work options.
7. **ValidateQuestionForm**
    
    - **Description**: This class validates user input in a question form. It ensures that the information provided by the user meets the necessary criteria before proceeding.
    - **Methods**:
        - `name(self)`: Returns the name of the action.
        - `run(self, dispatcher, tracker, domain)`: Executes the action logic to validate a question form.
        - `submit(self, dispatcher, tracker, domain)`: Handles the submission of the form.
8. **ActionSetQuestion**
    
    - **Description**: This class is responsible for setting a question based on user input or predefined criteria. It helps in managing the flow of conversation by asking relevant questions.
    - **Methods**:
        - `name(self)`: Returns the name of the action.
        - `run(self, dispatcher, tracker, domain)`: Executes the action logic to set a question.
9. **ActionSelectAppliedContexts**
    
    - **Description**: This class allows the chatbot to select and manage applied contexts based on user interactions. It helps in tailoring responses to fit the user's situation.
    - **Methods**:
        - `name(self)`: Returns the name of the action.
        - `run(self, dispatcher, tracker, domain)`: Executes the action logic to select applied contexts.
10. **ActionAppliedContext**
    
    - **Description**: This class handles actions related to applied contexts in conversations. It may provide context-specific information or manage user queries based on the context.
    - **Methods**:
        - `name(self)`: Returns the name of the action.
        - `run(self, dispatcher, tracker, domain)`: Executes the action logic related to applied contexts.
11. **ActionSelectEligibilityCriteria**
    
    - **Description**: This class allows users to select eligibility criteria for specific policies or programs. It helps in guiding users through the eligibility assessment process.
    - **Methods**:
        - `name(self)`: Returns the name of the action.
        - `run(self, dispatcher, tracker, domain)`: Executes the action logic to select eligibility criteria.
12. **ActionEligibilityCriteria**
    
    - **Description**: This class manages actions related to eligibility criteria. It may validate user input and provide information based on the criteria selected.
    - **Methods**:
        - `name(self)`: Returns the name of the action.
        - `run(self, dispatcher, tracker, domain)`: Executes the action logic related to eligibility criteria.
13. **ActionStoreResponse**
    
    - **  This class is responsible for storing user responses and may include functionality for saving these responses to a PDF or other formats.
    - **Methods**:
        - `name(self)`: Returns the name of the action.
        - `save_pdf(self, flexible_response, applied_context_response, eligibility_criteria_response)`: Saves the responses to a PDF.
        - `run(self, dispatcher, tracker, domain)`: Executes the action logic to store responses.
14. **ActionGenerateCustomPolicyClause**
    
    -   This class generates custom policy clauses based on specific user inputs or requirements. It helps in creating tailored policy documents.
    - **Methods**:
        - `name(self)`: Returns the name of the action.
        - `run(self, dispatcher, tracker, domain)`: Executes the action logic to generate a custom policy clause.
15. **ActionHRPolicyObservation**
    
    - ** This class observes and analyzes HR policies based on user interactions. It may identify gaps or areas for improvement in existing policies.
    - **Methods**:
        - `name(self)`: Returns the name of the action.
        - `run(self, dispatcher, tracker, domain)`: Executes the action logic to observe HR policies.
16. **ActionHandleMissingElement**
    
    -  manages situations where elements of a policy are missing. It may prompt the user for additional information or provide guidance on completing the policy.
    - **Methods**:
        - `name(self)`: Returns the name of the action.
        - `run(self, dispatcher, tracker, domain)`: Executes the action logic to handle missing elements in policies.
17. **ActionCreatePolicyDocument**
    
    - This class creates comprehensive policy documents based on user inputs and predefined templates. It consolidates various pieces of information into a structured format.
    - **Methods**:
        - `name(self)`: Returns the name of the action.
        - `createDocument(self, ...)`: Creates a policy document based on various parameters.
        - `run(self, dispatcher, tracker, domain)`: Executes the action logic to create a policy document.