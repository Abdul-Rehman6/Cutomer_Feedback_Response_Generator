# Customer Review Sentiment Analysis & Response System

An intelligent customer review analysis system built with LangGraph and LangChain that automatically analyzes customer feedback, determines sentiment, diagnoses issues, and generates appropriate responses.

## Overview

This system processes customer reviews through an automated workflow that:
- Analyzes sentiment (positive/negative)
- Diagnoses negative feedback by categorizing issues and urgency
- Generates contextually appropriate responses based on the analysis

## Features

- **Automated Sentiment Analysis**: Identifies whether reviews are positive or negative
- **Issue Diagnosis**: For negative reviews, categorizes problems by type, tone, and urgency
- **Intelligent Response Generation**: Creates personalized, empathetic responses tailored to the review content
- **Conditional Workflow**: Uses LangGraph for dynamic routing based on sentiment
- **Structured Outputs**: Leverages Pydantic schemas for consistent, typed results

## Architecture

The system uses a state graph with conditional edges:

```
START → Find Sentiment → Check Sentiment
                              ↓
                    Positive ↙   ↘ Negative
                            ↓       ↓
              Positive Response   Run Diagnosis
                            ↓       ↓
                          END   Negative Response
                                      ↓
                                    END
```

### State Schema

The workflow maintains the following state:
- `review`: Original customer review text
- `sentiment`: Detected sentiment (positive/negative)
- `diagnosis`: Detailed analysis for negative reviews (issue type, tone, urgency)
- `response`: Generated response message

### Issue Categories

**Issue Types**:
- UX (User Experience)
- Performance
- Bug
- Support
- Other

**Tone Detection**:
- Angry
- Frustrated
- Disappointed
- Calm

**Urgency Levels**:
- Low
- Medium
- High

## Technology Stack

- **LangGraph**: Workflow orchestration and state management
- **LangChain**: LLM integration and tooling
- **OpenAI GPT-4o-mini**: Language model for analysis and generation
- **Pydantic**: Schema validation and structured outputs
- **Python**: Core programming language

## Setup

### Prerequisites

- Python 3.8+
- OpenAI API key

### Installation

1. Clone the repository:
```bash
git clone <repository-url>
cd <project-directory>
```

2. Install dependencies:
```bash
pip install langgraph langchain langchain-openai langchain-community pydantic python-dotenv
```

3. Configure environment variables:
Create a `.env` file in the project root:
```env
OPENAI_API_KEY=your_openai_api_key_here
```

4. Run the notebook:
```bash
jupyter notebook Customer_Review_Conditional_LG.ipynb
```

## Usage

### Basic Example

```python
from dotenv import load_dotenv
load_dotenv()

# Initialize the workflow
workflow = graph.compile()

# Process a review
initial_state = {
    'review': "I have been trying to log in for over an hour now, and the app keeps freezing on the authentication screen."
}

# Run the workflow
final_state = workflow.invoke(initial_state)

# Access results
print(f"Sentiment: {final_state['sentiment']}")
print(f"Diagnosis: {final_state['diagnosis']}")
print(f"Response: {final_state['response']}")
```

### Example Output

**Input Review**:
> "I have been trying to log in for over an hour now, and the app keeps freezing on the authentication screen. I even tried reinstalling it, but no luck. This kind of bug is unacceptable, especially when it affects basic functionality."

**Analysis**:
- Sentiment: `negative`
- Diagnosis: `{'issue_type': 'Bug', 'tone': 'frustrated', 'urgency': 'high'}`

**Generated Response**:
> Subject: We're Here to Help You with Your Bug Issue
>
> Hi [User's Name],
>
> Thank you for reaching out and sharing your concerns. I completely understand how frustrating it can be to encounter bugs, especially when they disrupt your workflow. I'm here to help resolve this issue for you as quickly as possible...

## Workflow Components

### 1. Sentiment Detection
Uses structured output to classify reviews as positive or negative.

### 2. Conditional Routing
Routes to appropriate handler based on sentiment:
- Positive reviews → Thank you message
- Negative reviews → Diagnostic analysis

### 3. Issue Diagnosis
For negative reviews, extracts:
- Issue type
- Emotional tone
- Urgency level

### 4. Response Generation
Creates empathetic, context-aware responses that:
- Acknowledge the customer's concern
- Match the urgency level
- Provide helpful next steps

## Customization

### Modify Issue Categories
Edit the `DiagnosisSchema` in the notebook:
```python
class DiagnosisSchema(BaseModel):
    issue_type: Literal["YourCategories"] = Field(...)
    tone: Literal["YourTones"] = Field(...)
    urgency: Literal["YourLevels"] = Field(...)
```

### Adjust Response Templates
Modify the prompts in `positive_response()` and `negative_response()` functions.

### Change Language Model
Update the model initialization:
```python
llm = ChatOpenAI(model_name="gpt-4")  # or other models
```

## Project Structure

```
.
├── Customer_Review_Conditional_LG.ipynb  # Main notebook
├── .env                                  # Environment configuration
├── .gitignore                           # Git ignore rules
└── README.md                            # This file
```

## Best Practices

1. **Monitor API Usage**: Track OpenAI API calls to manage costs
2. **Validate Inputs**: Sanitize user reviews before processing
3. **Human Review**: Consider human oversight for high-urgency cases
4. **Feedback Loop**: Collect data on response quality for continuous improvement
5. **Privacy**: Ensure customer data handling complies with privacy regulations

## Limitations

- Requires internet connection for OpenAI API calls
- Processing time depends on API latency
- Quality depends on the underlying language model
- May require fine-tuning for domain-specific terminology

## Future Enhancements

- [ ] Add multi-language support
- [ ] Implement response caching for similar reviews
- [ ] Create a web interface for real-time processing
- [ ] Add analytics dashboard for review trends
- [ ] Integrate with ticketing systems
- [ ] Support batch processing for multiple reviews
- [ ] Add sentiment confidence scores

## Contributing

Contributions are welcome! Please feel free to submit pull requests or open issues for bugs and feature requests.

## License

This project is open source and available under the MIT License.

## Support

For questions or support, please open an issue in the repository.

---

Built with LangGraph and LangChain
