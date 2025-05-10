# Exno.6-Prompt-Engg
# Date:
# Register no:212223223003
# Aim: 
Development of Python Code Compatible with Multiple AI Tools
# Algorithm: 
Write and implement Python code that integrates with multiple AI tools to automate the task of interacting with APIs, comparing outputs, and generating actionable insights.
## Output:
## Objective
The objective of this experiment is to develop Python code that integrates with multiple AI tools (via their APIs), automates the process of interacting with these APIs, compares the outputs from the AI tools, and generates actionable insights based on these comparisons.

## Software and Tools Used
Python 3.x
Requests library – for making HTTP API requests
TextBlob library – for performing sentiment analysis
Difflib module – for calculating similarity percentage
OpenAI API (GPT-3)
Cohere API

## Python Code with Explanation
Below is the complete Python code along with a step-by-step explanation for each section.
~~~
# Import required libraries
import requests          # For making API requests
from textblob import TextBlob  # For sentiment analysis
import difflib           # For calculating similarity between outputs

# =========================
# CONFIGURATION SECTION
# =========================

# API endpoint for OpenAI
OPENAI_API_URL = "https://api.openai.com/v1/completions"
# Replace with your actual OpenAI API key
OPENAI_API_KEY = "your_openai_api_key"

# API endpoint for Cohere
COHERE_API_URL = "https://api.cohere.ai/generate"
# Replace with your actual Cohere API key
COHERE_API_KEY = "your_cohere_api_key"

# =========================
# FUNCTION TO QUERY OPENAI
# =========================

def query_openai(prompt):
    """
    Sends a prompt to the OpenAI API and returns the output text.
    """
    headers = {"Authorization": f"Bearer {OPENAI_API_KEY}"}
    payload = {
        "model": "text-davinci-003",  # OpenAI GPT-3 model
        "prompt": prompt,
        "max_tokens": 150              # Limit the output length
    }
    response = requests.post(OPENAI_API_URL, headers=headers, json=payload)
    result = response.json()
    return result['choices'][0]['text'].strip()

# =========================
# FUNCTION TO QUERY COHERE
# =========================

def query_cohere(prompt):
    """
    Sends a prompt to the Cohere API and returns the output text.
    """
    headers = {
        "Authorization": f"Bearer {COHERE_API_KEY}",
        "Content-Type": "application/json"
    }
    payload = {
        "model": "command-xlarge-nightly",  # Cohere's language model
        "prompt": prompt,
        "max_tokens": 150
    }
    response = requests.post(COHERE_API_URL, headers=headers, json=payload)
    result = response.json()
    return result['generations'][0]['text'].strip()

# =========================
# FUNCTION TO COMPARE OUTPUTS
# =========================

def compare_outputs(output1, output2):
    """
    Compares outputs from OpenAI and Cohere based on:
    - Similarity percentage (using difflib)
    - Sentiment polarity (using TextBlob)
    Returns a dictionary with comparison results.
    """
    similarity = difflib.SequenceMatcher(None, output1, output2).ratio() * 100
    sentiment1 = TextBlob(output1).sentiment.polarity
    sentiment2 = TextBlob(output2).sentiment.polarity
    return {
        "similarity_percentage": similarity,
        "sentiment_openai": sentiment1,
        "sentiment_cohere": sentiment2
    }

# =========================
# FUNCTION TO GENERATE INSIGHTS
# =========================

def generate_insights(comparison_data):
    """
    Generates insights from the comparison data.
    For example:
    - Whether outputs are similar
    - Which output has more positive sentiment
    """
    insights = []
    if comparison_data['similarity_percentage'] > 80:
        insights.append("Outputs are very similar. Both AI tools are consistent.")
    else:
        insights.append("Outputs differ significantly. Models interpret differently.")
    
    if comparison_data['sentiment_openai'] > comparison_data['sentiment_cohere']:
        insights.append("OpenAI output has a more positive sentiment.")
    elif comparison_data['sentiment_openai'] < comparison_data['sentiment_cohere']:
        insights.append("Cohere output has a more positive sentiment.")
    else:
        insights.append("Both outputs have similar sentiment polarity.")
    
    return insights

# =========================
# MAIN AUTOMATION PIPELINE
# =========================

def automated_pipeline(prompt):
    """
    Automates the entire process:
    - Queries OpenAI and Cohere
    - Compares outputs
    - Generates and prints insights
    """
    print("=== AI Tools Integration Experiment ===")
    print(f"Input Prompt: {prompt}\n")

    # Query OpenAI
    print("Querying OpenAI...")
    openai_output = query_openai(prompt)
    print("OpenAI Output:\n", openai_output, "\n")

    # Query Cohere
    print("Querying Cohere...")
    cohere_output = query_cohere(prompt)
    print("Cohere Output:\n", cohere_output, "\n")

    # Compare Outputs
    comparison = compare_outputs(openai_output, cohere_output)
    print("=== Comparison Metrics ===")
    print(f"Similarity Percentage: {comparison['similarity_percentage']:.2f}%")
    print(f"Sentiment (OpenAI): {comparison['sentiment_openai']:.3f}")
    print(f"Sentiment (Cohere): {comparison['sentiment_cohere']:.3f}\n")

    # Generate Insights
    insights = generate_insights(comparison)
    print("=== Generated Insights ===")
    for insight in insights:
        print("-", insight)

# =========================
# RUN THE EXPERIMENT
# =========================

if __name__ == "__main__":
    test_prompt = "Explain the benefits of using renewable energy sources."
    automated_pipeline(test_prompt)

~~~
# Explanation of Code Sections:
 # Imports:
We import requests for API calls, textblob for sentiment analysis, and difflib to calculate similarity.

 # API Configuration:
We specify the API URLs and API keys for OpenAI and Cohere.

# Functions:

query_openai(prompt) → sends the prompt to OpenAI and returns the output.

query_cohere(prompt) → sends the prompt to Cohere and returns the output.

compare_outputs(output1, output2) → compares outputs by similarity and sentiment.

generate_insights(comparison_data) → generates user-readable insights.

automated_pipeline(prompt) → integrates all steps into an automated workflow.

# Main Block:
When the program runs, it uses a test prompt → queries both models → compares outputs → prints metrics and insights.

## Sample Input & Output
# Input Prompt:
~~~
Explain the benefits of using renewable energy sources.
~~~
# Example Output:
~~~
=== AI Tools Integration Experiment ===
Input Prompt: Explain the benefits of using renewable energy sources.

Querying OpenAI...
OpenAI Output:
 Renewable energy sources provide sustainable alternatives to fossil fuels...

Querying Cohere...
Cohere Output:
 Renewable energy reduces pollution, promotes sustainability...

=== Comparison Metrics ===
Similarity Percentage: 70.45%
Sentiment (OpenAI): 0.24
Sentiment (Cohere): 0.28

=== Generated Insights ===
- Outputs differ significantly. Models interpret differently.
- Cohere output has a more positive sentiment.
~~~
## Conclusion
In this experiment, we developed Python code that integrates with multiple AI tools via their APIs. The code automates sending prompts, retrieving outputs, comparing them, and generating insights. This approach can be extended to evaluate multiple AI models, benchmark performance, or create AI-based decision support systems.






# Result: 
The corresponding Prompt is executed successfully
