# HR Policy Chatbot Documentation

## Overview
This is a Rasa-based chatbot that helps users create customized HR policies. The bot guides users through a series of questions and interactions to gather information about their company's HR policy needs, then generates appropriate documentation.

## Core Components

### Environment and Dependencies
- Uses OpenAI and Cohere APIs for natural language processing
- Document generation capabilities using FPDF, python-docx, and reportlab
- Loads environment variables for API keys
- Integration with external document templates and predefined questions

### Main Features
1. Policy Creation Flow
2. Question Management
3. Response Validation
4. Document Generation
5. Brand Integration

## Class Documentation

### ActionGreet
**Purpose**: Initial greeting and conversation starter
- Displays welcome message with emoji
- Presents initial options for HR Policy creation
- Returns action buttons for user interaction

### ActionCompanyName & ActionSetCompanyName
**Purpose**: Handles company information collection
- Prompts for company name
- Stores company name in slots
- Triggers HR policy selection flow

### ActionHrPolicy
**Purpose**: Main policy type selection handler
- Displays available policy types
- Manages policy template selection
- Presents options like Flexible Work, Remote Work, etc.
- Allows custom policy creation

### ActionPolicyType
**Purpose**: Handles specific policy type selection
- Loads predefined questions based on policy type
- Presents flexible work options
- Manages transition to question flow

### ActionSelectFlexibleWorkOption
**Purpose**: Flexible work policy configuration
- Uses AI to generate relevant questions
- Manages question flow for flexible work options
- Stores responses for policy generation

### ValidateQuestionForm
**Purpose**: Question validation system
- Validates user responses using AI
- Ensures answers are relevant to questions
- Manages question progression

### ActionSetQuestion
**Purpose**: Question presentation manager
- Presents questions sequentially
- Manages question index
- Handles transition between question sets

### ActionSelectAppliedContexts & ActionAppliedContext
**Purpose**: Policy context management
- Handles work situation selections
- Generates context-specific questions
- Manages applied context responses

### ActionSelectEligibilityCriteria & ActionEligibilityCriteria
**Purpose**: Eligibility criteria management
- Handles eligibility requirement selection
- Generates criteria-specific questions
- Manages eligibility responses

### ActionStoreResponse
**Purpose**: Response storage and PDF generation
- Collects all user responses
- Generates PDF summary
- Organizes responses by category

### ActionHRPolicyObservation
**Purpose**: Policy completeness checker
- Analyzes policy for missing elements
- Uses AI to identify gaps
- Triggers additional questions if needed

### ActionCreatePolicyDocument
**Purpose**: Final document generation
- Creates formatted Word document
- Incorporates company branding
- Summarizes policy sections
- Generates downloadable document

## Key Features

### Question Generation
- Uses AI models to generate contextual questions
- Implements question validation
- Manages question flow and progression

### Response Validation
```python
yes_no_prompt = """
Is the following answer relevant to the question {current_question}: '{user_answer}'? 
Answer with 'yes' or 'no', without any additional explanation.
"""
```
- Uses AI to validate response relevance
- Ensures quality of user inputs
- Manages response flow

### Document Generation
- Creates PDF summaries
- Generates formatted Word documents
- Incorporates company branding
- Organizes policy sections

### Error Handling
- Custom fallback actions
- Slot management
- Response validation

## Best Practices and Improvements

### Suggested Improvements
1. **Error Handling**
   - Add comprehensive try-catch blocks
   - Implement logging system
   - Add error recovery mechanisms

2. **Code Organization**
   - Separate constants into config file
   - Create utility classes for common functions
   - Implement proper dependency injection

3. **Documentation**
   - Add inline documentation
   - Create API documentation
   - Document configuration requirements

4. **Testing**
   - Add unit tests
   - Implement integration tests
   - Create test scenarios

5. **Performance**
   - Cache AI responses
   - Optimize document generation
   - Implement response batching

### Security Considerations
1. Input Validation
   - Implement strict input validation
   - Sanitize user inputs
   - Validate file operations

2. API Security
   - Secure API key management
   - Implement rate limiting
   - Add request validation

3. File Operations
   - Secure file handling
   - Implement file cleanup
   - Validate file paths

## Configuration

### Environment Variables
Required environment variables:
- OPENAI_API_KEY
- COHERE_API_KEY

### External Dependencies
- OpenAI API
- Cohere API
- Document templates
- Predefined questions JSON

## Usage Flow

1. User initiates conversation
2. Bot collects company information
3. User selects policy type
4. Bot generates relevant questions
5. User provides answers
6. Bot validates responses
7. Bot generates policy document
8. User receives downloadable document

## Error Codes and Handling

| Error Code | Description | Handling |
|------------|-------------|----------|
| API_ERROR | AI API failure | Retry with fallback |
| FILE_ERROR | Document generation error | Notify user |
| VALIDATION_ERROR | Response validation failure | Request new response |