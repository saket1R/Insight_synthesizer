# Insight_synthesizer
Creating a Python-based module that takes in raw user feedback (survey responses) and uses an LLM to extract structured insights.
## Technological stack used  :- Groq (mistral-saba-24b) model , Tried Hugging face Model (Mistralai-7b) , Python , API calling
A little overview :- 
Here I used the Groq Cloud model "mistral-saba-24b" as it was fast and did not cost any api charge as compared to other OpenAI models
I was also trying on the hugging face model Which was mistralai-7b but it was taking too much RAM usage on Collab and as well as was not able to generate proper responses and was giving session runtime error It was a 15 GB model and the code for the response generation was running for too long and Not producing result .
So I shifted to Groq Model .

Response Generation code:
prompt = """
Generate 58 unique, short user feedback responses (1–2 sentences each) for an AI Email assistant.
These should reflect different opinions, suggestions, or concerns. Mix positive, negative, and neutral tones.
"""

payload = {
    "model": "mistral-saba-24b",   
    "messages": [
        {"role": "user", "content": prompt}
    ],
    "temperature": 0.7,
    "max_tokens": 2048
}

response = requests.post(url, headers=headers, data=json.dumps(payload))
result = response.json()
print(result) 

content = result['choices'][0]['message']['content']

sentences = content.split(". ")
for sentence in sentences:
    print(sentence.strip())

  Stored the responses into a single file called , feedback list.

  Final output generation code :-

  prompt = f"""
You are a Product Analyst AI.

Analyze the following 58 user feedback responses for an AI Email Assistant and provide the output in the following JSON format:

[
  {{
    "theme": "string, summarizing a key theme",
    "quotes": [
      "direct quote from feedback",
      "another quote..."
    ],
    "sentiment": "positive | neutral | negative"
  }},
  ...
]

Here are the feedback responses:

{joined_feedbacks}

Make sure the output is valid JSON and well-structured.
"""
payload = {
    "model": "mistral-saba-24b",  
    "messages": [
        {
            "role": "user",
            "content": prompt
        }
    ],
    "temperature": 0.7,
    "max_tokens": 2048
}

response = requests.post(url, headers=headers, data=json.dumps(payload))
result = response.json()

print(result["choices"][0]["message"]["content"])


