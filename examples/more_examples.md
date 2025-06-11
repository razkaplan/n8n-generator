TITLE: Structuring OpenAI Email Summary Prompt
DESCRIPTION: This text content serves as the prompt for the OpenAI node, guiding the AI model to process the aggregated email data. It instructs the model to identify key details, issues, and action items, and specifies the exact JSON structure required for the output, including an embedded n8n expression to inject the input data.
SOURCE: https://github.com/egouilliard/n8n_examples/blob/main/examples/Email Summary Agent.txt#_snippet_1

LANGUAGE: Prompt
CODE:
```
=Go through this email summary and identify all key details mentioned, any specific issues to look at, and action items.\nUse this format to output\n{\n \"summary_of_emails\": [\n \"Point 1\",\n \"Point 2\",\n \"Point 3\"\n ],\n \"actions\": [\n {\n \"name\": \"Name 1\",\n \"action\": \"Action 1\"\n },\n {\n \"name\": \"Name 1\",\n \"action\": \"Action 2\"\n },\n {\n \"name\": \"Name 2\",\n \"action\": \"Action 3\"\n }\n ]\n}\n\nInput Data:\n\n {{ $json.data.toJsonString() }}\n\n
```

----------------------------------------

TITLE: LLM Prompt for Data Extraction - n8n Template
DESCRIPTION: This template defines the input text sent to the LLM model. It includes the extracted file content from the previous node (`$json.text`) wrapped in `<file>` tags for clear separation. It also injects the description of the data to be extracted and the required output format, retrieved from the 'Event Ref' node. This provides the LLM with all necessary context to perform the extraction task based on the file content and the field description.
SOURCE: https://github.com/egouilliard/n8n_examples/blob/main/examples/AI Data Extraction with Dynamic Prompts and Airtable.txt#_snippet_2

LANGUAGE: n8n Template
CODE:
```
=<file>
{{ $json.text }}
</file>

Data to extract: {{ $('Event Ref').first().json.field.description }}
output format is: {{ $('Event Ref').first().json.field.type }}
```

----------------------------------------

TITLE: Configuring OpenAI Chat Completions API Request Body (JSON)
DESCRIPTION: This JSON payload defines the request sent to the OpenAI Chat Completions API. It specifies the model, provides system and user messages (injecting user input via n8n expression), and crucially defines a `response_format` using a `json_schema`. This schema strictly dictates the structure of the AI's output to represent UI components with type, label, children, and attributes suitable for Tailwind CSS classes.
SOURCE: https://github.com/egouilliard/n8n_examples/blob/main/examples/Dynamically generate a webpage from user request using OpenAI Structured Output (1).txt#_snippet_0

LANGUAGE: json
CODE:
```
={
 "model": "gpt-4o-2024-08-06",
 "messages": [
 {
 "role": "system",
 "content": "You are a user interface designer and copy writter. Your job is to help users visualize their website ideas. You design elegant and simple webs, with professional text. You use Tailwind framework"
 },
 {
 "role": "user",
 "content": "{{ $json.query.query }}"
 }
 ],
 "response_format":
{
 "type": "json_schema",
 "json_schema": {
 "name": "ui",
 "description": "Dynamically generated UI",
 "strict": true,
 "schema": {
 "type": "object",
 "properties": {
 "type": {
 "type": "string",
 "description": "The type of the UI component",
 "enum": [
 "div",
 "span",
 "a",
 "p",
 "h1",
 "h2",
 "h3",
 "h4",
 "h5",
 "h6",
 "ul",
 "ol",
 "li",
 "img",
 "button",
 "input",
 "textarea",
 "select",
 "option",
 "label",
 "form",
 "table",
 "thead",
 "tbody",
 "tr",
 "th",
 "td",
 "nav",
 "header",
 "footer",
 "section",
 "article",
 "aside",
 "main",
 "figure",
 "figcaption",
 "blockquote",
 "q",
 "hr",
 "code",
 "pre",
 "iframe",
 "video",
 "audio",
 "canvas",
 "svg",
 "path",
 "circle",
 "rect",
 "line",
 "polyline",
 "polygon",
 "g",
 "use",
 "symbol"
]
 },
 "label": {
 "type": "string",
 "description": "The label of the UI component, used for buttons or form fields"
 },
 "children": {
 "type": "array",
 "description": "Nested UI components",
 "items": {
 "$ref": "#"
 }
 },
 "attributes": {
 "type": "array",
 "description": "Arbitrary attributes for the UI component, suitable for any element using Tailwind framework",
 "items": {
 "type": "object",
 "properties": {
 "name": {
 "type": "string",
 "description": "The name of the attribute, for example onClick or className"
 },
 "value": {
 "type": "string",
 "description": "The value of the attribute using the Tailwind framework classes"
 }
 },
 "additionalProperties": false,
 "required": ["name", "value"]
 }
 }
 },
 "required": ["type", "label", "children", "attributes"],
 "additionalProperties": false
 }
 }
}
```

----------------------------------------

TITLE: Generating OpenAI Sentiment Analysis Prompt (n8n)
DESCRIPTION: Constructs the text prompt sent to the OpenAI API for sentiment analysis of customer feedback. It utilizes an n8n expression (`{{ $json[...] }}`) to dynamically insert the customer feedback text from the incoming data (received from the form trigger and merged) into a predefined prompt string. Dependencies include the upstream node providing the customer feedback in the `$json` data and a configured OpenAI credential. Expected input is a JSON object containing the 'Your feedback' field; output is the complete prompt string.
SOURCE: https://github.com/egouilliard/n8n_examples/blob/main/examples/AI Customer feedback sentiment analysis.txt#_snippet_1

LANGUAGE: n8n Expression
CODE:
```
=Classify the sentiment in the following customer feedback: {{ $json['Your feedback'] }}
```

----------------------------------------

TITLE: Prepare Data for LLM Prompting (JavaScript)
DESCRIPTION: This snippet prepares data for use with an LLM node, such as the Langchain chat node. It defines a helper function `replacePlaceholders` to substitute `{{...}}` patterns in text using values from the current item (`row`), workflow settings, or configuration data. It then uses this function to dynamically construct the prompt and determine the LLM model based on the 'Action' property of the current item and the loaded configuration, and also calculates if an action is needed.
SOURCE: https://github.com/egouilliard/n8n_examples/blob/main/examples/Author and Publish Blog Posts From Google Sheets.txt#_snippet_1

LANGUAGE: JavaScript
CODE:
```
function replacePlaceholders(text, row, config) {
 function checkProp(prop, lookup) {
 // console.log('checkProp:' + prop);
 if (!lookup.hasOwnProperty(prop)) return false;
 let value = lookup[prop];
 if (typeof(value) == 'string') {
 value = value.trim();
 if (value == '') return false;
 }
 // console.log('checkProp found:', value)
 return value;
 }
 function replaceMatch(fullMatch, prop) { 
 prop = prop.trim();
 // Return the corresponding value
 return checkProp(prop, row)
 || checkProp(prop, config)
 || checkProp(prop + checkProp('Context', row), config)
 || `[could not find "${ prop }]"`;
 }

 if (typeof(text) != 'string') return '';

 // Regex to capture {{ ... }}
 const pattern = /\{\{\s*([^}]+)\s*\}\}/g
 const result = text.replace(pattern, replaceMatch);
 return result.trim();
}

const row = $json;
const settings = $("Settings").first().json;
const config = $("Config").first().json;
const prompt_key = 'prompt_' + row.Action;
const prompt = replacePlaceholders(config[prompt_key], row, config);
const model_key = prompt_key + '_model';
const model = replacePlaceholders(config[model_key], row, config);
const outputFormat = config[prompt_key + '_outputFormat'];
const takeAction = row.Action != row.Status;
const action = row.Action

// console.log('prompt', prompt);

// console.log(prompt);
return { takeAction, action, model_key, model, prompt_key, prompt, outputFormat, row, config, settings }
```

----------------------------------------

TITLE: Check for Image Message Type Condition in n8n Expression
DESCRIPTION: This expression is used as a condition in a Switch node. It checks if the current item's JSON 'type' property is exactly 'image' AND if the 'image' field exists and is not null/undefined/false (using `Boolean($json.image)`), returning true only if both conditions are met to route image messages.
SOURCE: https://github.com/egouilliard/n8n_examples/blob/main/examples/Respond to WhatsApp Messages with AI Like a Pro!.txt#_snippet_11

LANGUAGE: n8n Expression
CODE:
```
={{ $json.type == 'image' && Boolean($json.image) }}
```

----------------------------------------

TITLE: Defining Output Schema for Structured Parser - JSON Schema
DESCRIPTION: This JSON schema defines the expected structure for the output of a Language Model, specifically for an 'article' object. It outlines required fields like category, title, metadata (timePosted, author, tag), content (mainText, sections with title, text, quote), and hashtags, ensuring the parsed data conforms to this format for subsequent processing.
SOURCE: https://github.com/egouilliard/n8n_examples/blob/main/examples/🔍 Perplexity Research to HTML_ AI-Powered Content Creation.txt#_snippet_0

LANGUAGE: JSON Schema
CODE:
```
{
 "type": "object",
 "properties": {
 "article": {
 "type": "object",
 "required": ["category", "title", "metadata", "content", "hashtags"],
 "properties": {
 "category": {
 "type": "string",
 "description": "Article category"
 },
 "title": {
 "type": "string",
 "description": "Article title"
 },
 "metadata": {
 "type": "object",
 "properties": {
 "timePosted": {
 "type": "string",
 "description": "Time since article was posted"
 },
 "author": {
 "type": "string",
 "description": "Article author name"
 },
 "tag": {
 "type": "string",
 "description": "Article primary tag"
 }
 },
 "required": ["timePosted", "author", "tag"]
 },
 "content": {
 "type": "object",
 "properties": {
 "mainText": {
 "type": "string",
 "description": "Main article content"
 },
 "sections": {
 "type": "array",
 "items": {
 "type": "object",
 "properties": {
 "title": {
 "type": "string",
 "description": "Section title"
 },
 "text": {
 "type": "string",
 "description": "Section content"
 },
 "quote": {
 "type": "string",
 "description": "Blockquote text"
 }
 },
 "required": ["title", "text", "quote"]
 }
 }
 },
 "required": ["mainText", "sections"]
 },
 "hashtags": {
 "type": "array",
 "items": {
 "type": "string"
 },
 "description": "Article hashtags"
 }
 }
 }
 }
}
```

----------------------------------------

TITLE: Getting Current Timestamp n8n Expression
DESCRIPTION: This n8n expression retrieves the current date and time and formats it as an ISO 8601 string. It is typically used in Set nodes to add a precise timestamp property to the workflow data being processed.
SOURCE: https://github.com/egouilliard/n8n_examples/blob/main/examples/Conversational Interviews with AI Agents and n8n Forms.txt#_snippet_0

LANGUAGE: n8n Expression
CODE:
```
={{ $now.toISO() }}
```

----------------------------------------

TITLE: Construct Gemini 2.0 API Request Body - n8n Expression
DESCRIPTION: This n8n expression dynamically generates the JSON payload for the Gemini 2.0 Flash API request. It includes a text prompt for object detection ("I want to see all bounding boxes of rabbits in this image."), embeds the image data in base64 format, and specifies the desired JSON response schema for the bounding boxes.
SOURCE: https://github.com/egouilliard/n8n_examples/blob/main/examples/Prompt-based Object Detection with Gemini 2.0.txt#_snippet_1

LANGUAGE: n8n Expression
CODE:
```
={{\n{\n \"contents\": [{\n \"parts\":[\n {\"text\": \"I want to see all bounding boxes of rabbits in this image.\"},\n {\n \"inline_data\": {\n \"mime_type\":\"image/jpeg\",\n \"data\": $input.item.binary.data.data\n }\n }\n ]\n }],\n \"generationConfig\": {\n \"response_mime_type\": \"application/json\",\n \"response_schema\": {\n \"type\": \"ARRAY\",\n \"items\": {\n \"type\": \"OBJECT\",\n \"properties\": {\n \"box_2d\": {\"type\":\"ARRAY\", \"items\": { \"type\": \"NUMBER\" } },\n \"label\": { \"type\": \"STRING\"}\n }\n }
 }
 }
 }
}\n}}
```

----------------------------------------

TITLE: Responding to Webhook with n8n Expression - n8n Expression
DESCRIPTION: This n8n expression retrieves the value of the 'text' property from the incoming JSON data. It is used within the 'Respond to Webhook' node to set the response body sent back to the original webhook caller.
SOURCE: https://github.com/egouilliard/n8n_examples/blob/main/examples/🔍 Perplexity Research to HTML_ AI-Powered Content Creation.txt#_snippet_1

LANGUAGE: n8n Expression
CODE:
```
={{ $json.text }}
```

----------------------------------------

TITLE: Accessing Current Item Data Property n8n Expression
DESCRIPTION: This n8n expression accesses the value of the 'data' property from the current workflow item's JSON data (`$json`). It is used here within an IF node's condition to check for the existence or value of this property.
SOURCE: https://github.com/egouilliard/n8n_examples/blob/main/examples/Conversational Interviews with AI Agents and n8n Forms.txt#_snippet_7

LANGUAGE: n8n Expression
CODE:
```
={{ $json.data }}
```

----------------------------------------

TITLE: Stringifying Node Output for AI Tool in n8n Expression
DESCRIPTION: This n8n expression is used within the AI Agent's system message. It calls the `JSON.stringify` function on the current node's output data (`$json.output`) to format it as a JSON string, preparing it as input for another AI tool (like 'personal_shopper').
SOURCE: https://github.com/egouilliard/n8n_examples/blob/main/examples/Personal Shopper Chatbot for WooCommerce with RAG using Google Drive and openAI.txt#_snippet_1

LANGUAGE: n8n Expression Language
CODE:
```
{{ JSON.stringify($json.output) }}
```

----------------------------------------

TITLE: Accessing Content Field (n8n Expression)
DESCRIPTION: Retrieves the value of the 'Content' field from the output of the preceding node. This expression is used as input for conditions in the 'Is Retweet or Old?' node and as input for sentiment analysis in the Google Cloud Natural Language nodes.
SOURCE: https://github.com/egouilliard/n8n_examples/blob/main/examples/Automate testimonials in Strapi with n8n.txt#_snippet_4

LANGUAGE: n8n Expression Language
CODE:
```
={{$json["Content"]}}
```

----------------------------------------

TITLE: Mapping Input Data to Airtable Name Column (n8n Expression)
DESCRIPTION: This n8n expression maps the 'Name' property from the incoming JSON data to the 'Name' column in the Airtable record. It assumes the input data contains a top-level 'Name' key. Required: Input data with a 'Name' field.
SOURCE: https://github.com/egouilliard/n8n_examples/blob/main/examples/HR Job Posting and Evaluation with AI.txt#_snippet_0

LANGUAGE: n8n Expression
CODE:
```
={{ $json.Name }}
```

----------------------------------------

TITLE: Iterating over Email Attachments with n8n Code Node (JavaScript)
DESCRIPTION: This JavaScript code snippet, used within an n8n Code node, processes input items to extract each email attachment into its own separate output item. It loops through the binary data keys provided by the email trigger node and creates a new item for each binary property, facilitating individual processing of attachments by subsequent nodes.
SOURCE: https://github.com/egouilliard/n8n_examples/blob/main/examples/Send specific PDF attachments from Gmail to Google Drive using OpenAI.txt#_snippet_0

LANGUAGE: JavaScript
CODE:
```
// https://community.n8n.io/t/iterating-over-email-attachments/13588/3
let results = [];

for (const item of $input.all()) {
 for (key of Object.keys(item.binary)) {
  results.push({
   json: {},
   binary: {
    data: item.binary[key],
   }
  });
 }
}

return results;
```

----------------------------------------

TITLE: Get Message Text from Input Item in n8n Expression
DESCRIPTION: This expression directly accesses the 'text' property from the current item's JSON data (`$json`). It is used to extract the message content if the incoming message is of type 'text' and the text content is available in the item's payload.
SOURCE: https://github.com/egouilliard/n8n_examples/blob/main/examples/Respond to WhatsApp Messages with AI Like a Pro!.txt#_snippet_6

LANGUAGE: n8n Expression
CODE:
```
={{ $json.text }}
```

----------------------------------------

TITLE: Defining AI Behavior for Meeting Processing - N8N Prompt/Template
DESCRIPTION: This multi-line string defines the system prompt for the AI model. It instructs the AI to act as a meeting assistant, process Zoom data (title, participants, transcript), identify tasks and follow-up meeting details, and format the output for specific tools (ClickUp for tasks, Outlook for meetings). It includes placeholders for dynamic data inputs from previous nodes using N8N expressions.
SOURCE: https://github.com/egouilliard/n8n_examples/blob/main/examples/Zoom AI Meeting Assistant creates mail summary, ClickUp tasks and follow-up call.txt#_snippet_0

LANGUAGE: N8N Prompt/Template
CODE:
```
=<system_prompt>\n\nTODAY IS: {{ $now }}\n\nYOU ARE A MEETING ASSISTANT FOR AUTOMATION IN N8N. YOUR TASK IS TO EFFICIENTLY AND PRECISELY PROCESS INFORMATION FROM ZOOM MEETINGS TO GENERATE TO-DOS AND SCHEDULE FOLLOW-UP MEETINGS. YOU HAVE ACCESS TO THE FOLLOWING DATA:\n\n### INPUTS ###\n- **MEETING TITLE**: {{ $('Zoom: Get data of last meeting').item.json.topic }}\n- **PARTICIPANTS**: {{ $('Zoom: Get participants data').item.json.participants[0].name }}\n- **TRANSCRIPT**: {{ $('Format transcript text').item.json.transcript }}\n\n### YOUR TASKS ###\n1. **CREATE TO-DOS**:\n - IDENTIFY TASKS AND TO-DOS IN THE TRANSCRIPT.\n - FORMULATE CLEAR, CONCRETE TASKS.\n - PASS THESE TASKS TO THE TOOL "Create tasks" TO SAVE THEM IN CLICKUP. \n - DATA STRUCTURE:\n - **TASK DESCRIPTION**: Brief description of the task.\n - **ASSIGNED PERSON**: First name from the participant list.\n - **DUE DATE**: Use any date mentioned in the transcript; otherwise, set to "Not specified."\n\n2. **CREATE MEETING**:\n - ANALYZE THE TRANSCRIPT TO IDENTIFY INFORMATION ABOUT THE NEXT MEETING (DATE, TIME, AND TOPIC).\n - PASS THIS INFORMATION TO THE TOOL "Create follow-up call."\n - DATA STRUCTURE:\n - **MEETING TITLE**: "Follow-up: [Meeting Title]"\n - **DATE AND TIME**: Determined from the transcript or set to "Next Tuesday at 10:00 AM" if no information is provided.\n - **PARTICIPANTS**: Add all participants from the list.\n\n### CHAIN OF THOUGHTS ###\n1. **UNDERSTAND**: Read and analyze the provided inputs (title, participants, transcript).\n2. **IDENTIFY**: Extract relevant information for the to-dos and the next meeting.\n3. **DIVIDE**: Split the task into two separate processes: creating to-dos and creating the meeting.\n4. **STRUCTURE**: Format the results in the required structure for the respective tools.\n5. **TRANSMIT**: Pass the data to the designated tools in n8n.\n6. **VERIFY**: Ensure the data is correct and complete.\n\n### WHAT YOU SHOULD NOT DO ###\n- **NEVER**: Create unclear or vague to-dos.\n- **NEVER**: Ignore missing data – use default values where uncertain.\n- **NEVER**: Overlook information from the inputs or make incorrect connections.\n- **NEVER**: Transmit tasks or meetings without proper formatting.\n\n### OUTPUT EXAMPLES ###\n1. **TO-DO**:\n - **TASK DESCRIPTION**: "Prepare presentation for the next meeting."\n - **ASSIGNED PERSON**: "John Doe."\n - **DUE DATE**: "2025-01-25."

2. **MEETING**:
 - **MEETING TITLE**: "Follow-up: Project Discussion."
 - **DATE AND TIME**: "2025-01-28 at 10:00 AM."
 - **PARTICIPANTS**: "John Doe, Jane Example."

### NOTES ###
- EXECUTE YOUR TASKS WITH THE HIGHEST PRECISION AND CONTEXT SENSITIVITY.
- RELY ON THE PROVIDED DATA AND DEFAULT VALUES WHERE NECESSARY.
</system_prompt>
```

----------------------------------------

TITLE: Generating Email Content from Input Data in HTML Template
DESCRIPTION: This snippet uses n8n's expression syntax (JavaScript-based) within an HTML template to iterate over input items, extract specific data fields (Title, Agency, URL, Goal, Success Criteria, Dates), format them into an HTML structure (div, h3, strong, a, p, ul, li), split and process arrays (Success Criteria), format dates, and join the results with horizontal rules. It dynamically creates the content for an email or notification.
SOURCE: https://github.com/egouilliard/n8n_examples/blob/main/examples/Deduplicate Scraping AI Grants for Eligibility using AI.txt#_snippet_5

LANGUAGE: JavaScript
CODE:
```
$input.all().map((input,idx) => {\nreturn `\n <div>\n <div style=\"padding-top:14px;padding-bottom:24px\">\n <h3 style=\"margin-top:0;margin-bottom:7px;font-size:16px\">\n ${idx+1}. ${input.json.Title}\n </h3>\n <div style=\"margin-bottom:14px;font-size:12px;\">\n <strong>${input.json.Agency}</strong>\n &middot;\n <a href=\"${input.json.URL}\">See details</a>\n </div>\n <p style=\"margin-bottom:14px;font-size:14px\">\n <strong>Synopsis:</strong> ${input.json.Goal}\n </p>\n <ul style=\"font-size:14px;\">\n ${input.json['Success Criteria']\n .split('\\n')\n .map(text => `<li>${text}</li>`)\n .join('')\n }\n </ul>\n <div style=\"font-size:12px;\">\n <strong>Posted By</strong> ${input.json['Posted Date']\n .toDateTime()\n .format('EEE, dd MMM yyyy t')}\n <br/>\n <strong>Respond By</strong> ${input.json['Response Date']\n .toDateTime()\n .format('EEE, dd MMM yyyy t')}\n \n </div>\n</div> \n`\n}).join('<hr/>')
```

----------------------------------------

TITLE: Splitting Out Sections Field (n8n Expression Language)
DESCRIPTION: Specifies the 'section' array field to be split out into separate items.
SOURCE: https://github.com/egouilliard/n8n_examples/blob/main/examples/Build a Tax Code Assistant with Qdrant, Mistral.ai and OpenAI.txt#_snippet_10

LANGUAGE: n8n Expression Language
CODE:
```
=section
```

----------------------------------------

TITLE: Formatting Reddit Post Creation Date (Localized) (n8n Expression)
DESCRIPTION: Converts the Reddit post creation timestamp (`$json.created`, which is in seconds since epoch) into a human-readable, localized date/time string. It uses the `DateTime` object and `toLocaleString()` method provided by n8n's expression environment. Requires a preceding Reddit node.
SOURCE: https://github.com/egouilliard/n8n_examples/blob/main/examples/Reddit AI digest.txt#_snippet_4

LANGUAGE: n8n Expression
CODE:
```
DateTime.fromSeconds($json.created).toLocaleString()
```

----------------------------------------

TITLE: Getting Telegram Chat ID from Trigger Node in n8n
DESCRIPTION: Retrieves the chat ID from the original Telegram trigger node ('Get the Image'). This is necessary to send the workflow's response back to the correct chat conversation where the message originated. It uses the '$' function to reference a specific node by name.
SOURCE: https://github.com/egouilliard/n8n_examples/blob/main/examples/Automated AI image analysis and response via Telegram.txt#_snippet_1

LANGUAGE: n8n Expression
CODE:
```
={{ $('Get the Image').item.json.message.chat.id }}
```

----------------------------------------

TITLE: Set Final Email Recipient and Sender in n8n
DESCRIPTION: These snippets show how the 'Send Email' node dynamically sets the 'To' and 'From' email addresses for the final reply. It uses expressions to pull the sender (`from`) and recipient (`to`) addresses from the original incoming email captured by the 'Email Trigger (IMAP)' node.
SOURCE: https://github.com/egouilliard/n8n_examples/blob/main/examples/AI-powered email processing autoresponder and response approval (Yes_No).txt#_snippet_10

LANGUAGE: n8n Expression
CODE:
```
={{ $('Email Trigger (IMAP)').item.json.from }}
```

LANGUAGE: n8n Expression
CODE:
```
={{ $('Email Trigger (IMAP)').item.json.to }}
```

----------------------------------------

TITLE: Extracting GitHub Posts from Hacker News HTML Python
DESCRIPTION: This Python code snippet parses the HTML content of a Hacker News page to extract metadata for posts that link to GitHub. It uses BeautifulSoup to find post details like title, URL, score, author, and comment count, specifically filtering for URLs containing 'github.com'. It requires the 'beautifulsoup4' and 'simplejson' libraries to be installed (handled asynchronously within the script) and outputs a list of dictionaries, each representing a GitHub post's metadata.
SOURCE: https://github.com/egouilliard/n8n_examples/blob/main/examples/AI-Powered Social Media Amplifier.txt#_snippet_0

LANGUAGE: python
CODE:
```
# Import necessary modules
import asyncio
import micropip

# Define an asynchronous function to install packages
async def install_packages():
 await micropip.install("beautifulsoup4")
 await micropip.install("simplejson")

# Run the asynchronous package installation
asyncio.get_event_loop().run_until_complete(install_packages())

# Now, import the installed packages
import simplejson as json
from bs4 import BeautifulSoup

# Retrieve the HTML content from the first item in the input
# Assuming n8n passes data as a list of items, each with a 'json' key
html_content = items[0].get('json', {}).get('data', '')

# Initialize BeautifulSoup with the HTML content
soup = BeautifulSoup(html_content, 'html.parser')

# Initialize a list to store metadata of GitHub posts
github_posts = []

# Find all 'tr' elements with class 'athing submission'
posts = soup.find_all('tr', class_='athing submission')

for post in posts:
 post_id = post.get('id')
 title_line = post.find('span', class_='titleline')
 if not title_line:
 continue # Skip if titleline is not found

 # Extract the title and URL
 title_tag = title_line.find('a')
 if not title_tag:
 continue # Skip if title tag is not found

 title = title_tag.get_text(strip=True)
 url = title_tag.get('href', '')

 # Check if the URL is a GitHub link
 if 'github.com' not in url.lower():
 continue # Skip if not a GitHub link

 # Extract the site domain (e.g., github.com/username/repo)
 site_bit = title_line.find('span', class_='sitebit comhead')
 site = site_bit.find('span', class_='sitestr').get_text(strip=True) if site_bit else ''

 # The subtext is in the next 'tr' element
 subtext_tr = post.find_next_sibling('tr')
 if not subtext_tr:
 continue # Skip if subtext row is not found

 subtext_td = subtext_tr.find('td', class_='subtext')
 if not subtext_td:
 continue # Skip if subtext td is not found

 # Extract score
 score_span = subtext_td.find('span', class_='score')
 score = score_span.get_text(strip=True) if score_span else '0 points'

 # Extract author
 author_a = subtext_td.find('a', class_='hnuser')
 author = author_a.get_text(strip=True) if author_a else 'unknown'

 # Extract age
 age_span = subtext_td.find('span', class_='age')
 age_a = age_span.find('a') if age_span else None
 age = age_a.get_text(strip=True) if age_a else 'unknown'

 # Extract comments
 comments_a = subtext_td.find_all('a')[-1] if subtext_td.find_all('a') else None
 comments_text = comments_a.get_text(strip=True) if comments_a else '0 comments'

 # Construct the Hacker News URL
 hn_url = f"https://news.ycombinator.com/item?id={post_id}"

 # Compile the metadata
 post_metadata = {
 'Post': post_id,
 'title': title,
 'url': url,
 'site': site,
 'score': score,
 'author': author,
 'age': age,
 'comments': comments_text,
 'hn_url': hn_url
 }

 # Append to the list of GitHub posts
 github_posts.append(post_metadata)

# Prepare the output for n8n
output = [{'json': post} for post in github_posts]

# Return the output
return output
```

----------------------------------------

TITLE: Define AI Recruitment Agent - LangChain Prompt
DESCRIPTION: Provides the detailed system prompt for the LangChain agent node. It instructs the AI to analyze and compare resume text with a job description, assess the fit level and score, and provide a justification. The prompt dynamically includes the job title, job description, and extracted resume text using n8n expressions.
SOURCE: https://github.com/egouilliard/n8n_examples/blob/main/examples/AI-Powered Candidate Shortlisting Automation for ERPNext.txt#_snippet_10

LANGUAGE: Prompt
CODE:
```
System Prompt : 
You are a highly skilled AI agent trained to compare and analyze text from resumes against job descriptions. Your primary goal is to assess whether the candidate is a good fit for the role based on the given inputs. You will receive two inputs:

1. **Job Description**: A detailed description of the responsibilities, qualifications, and skills required for a specific job role.
2. **Resume Text**: A summary of a candidate's qualifications, skills, experience, and education.

Your task is to:
1. **Analyze Match**: Compare the candidate's resume text against the job description and assess the alignment of:
 - Required skills
 - Relevant experience
 - Educational background
 - Certifications
 - Keywords mentioned in both texts (e.g., specific tools, methodologies, or terminologies).

2. **Assess Fit**: Determine if the candidate is a strong, moderate, or weak fit for the role. Assign a score from 0 to 100 based on relevance:
 - **Strong Fit**: 80–100 (Candidate meets or exceeds the majority of the job requirements).
 - **Moderate Fit**: 50–79 (Candidate meets some key requirements but lacks in others).
 - **Weak Fit**: Below 50 (Candidate does not align with the role requirements).

3. **Provide Justification**: Include a brief explanation of why the candidate is or isn’t a good fit, highlighting strengths, gaps, or missing criteria.

Output Format:
- **Fit Level**: [Strong Fit / Moderate Fit / Weak Fit]
- **Score**: [0–100]
- **Rating**: [0–5]
- **Justification**: A concise summary of the reasoning behind the fit level.

Remember to maintain a neutral and objective tone in your analysis and ensure that your assessment is solely based on the information provided in the inputs."


Provide me the output in the following format:

FitLevel
<fitLevel>

Score:
<score>

Rating:
<rating>

Justification:
<justification>

Below are the inputs 

Job Title : {{ $json.job_title }}
Job Desription : {{ $json.description }}


Here here Job Applican't text from Resume : 
{{ $('PDF to Text').item.json.text }}

```

----------------------------------------

TITLE: Combining Data from Previous and Current Items in n8n Expression
DESCRIPTION: This n8n expression combines the 'text' property from the 'Get invoice' node's item data with the 'text' property of the current item's JSON data. It inserts a newline character ('\n') between the two values, useful for concatenating text fields while preserving formatting.
SOURCE: https://github.com/egouilliard/n8n_examples/blob/main/examples/Extract spending history from gmail to google sheet.txt#_snippet_4

LANGUAGE: n8n Expression
CODE:
```
={{ $('Get invoice').item.json.text }} \n {{ $json.text }}
```

----------------------------------------

TITLE: Generating Slack Block Kit for Auto-Issue Confirmation (n8n)
DESCRIPTION: This snippet defines the Slack Block Kit JSON structure for a confirmation message sent after a Certificate Signing Request (CSR) has been successfully auto-issued via Venafi TLS Protect Cloud. It pulls details like team, requestor email, issue date, common name, organization, and validity period using n8n expressions and includes action buttons to view CSR details in Venafi or revoke the certificate.
SOURCE: https://github.com/egouilliard/n8n_examples/blob/main/examples/Venafi Cloud Slack Cert Bot.txt#_snippet_20

LANGUAGE: n8n Workflow JSON
CODE:
```
={
	"blocks": [
		{
			"type": "section",
			"text": {
				"type": "mrkdwn",
				"text": "*:lock: CSR Auto-Issued Successfully!*"
			}
		},
		{
			"type": "divider"
		},
		{
			"type": "section",
			"text": {
				"type": "mrkdwn",
				"text": "*Team:* {{ $('Parse Webhook').item.json.response.message.blocks[2].text.text.match(/\*Team:\*\s*([^\n]*)/)[1] }}\n*Requested by:* \n*Email:* {{ $('Parse Webhook').item.json.response.message.blocks[2].text.text.match(/\*Requestor\sEmail:\*\s*<mailto:([^|]+)\|/)[1] }}\n*Date Issued:* {{ $json.creationDate }}"
			},
			"accessory": {
				"type": "image",
				"image_url": "{{ $('Parse Webhook').item.json.response.message.blocks[2].accessory.image_url }}",
				"alt_text": "Team Avatar"
			}
		},
		{
			"type": "context",
			"elements": [
				{
					"type": "mrkdwn",
					"text": "*CSR Details:*"
				}
			]
		},
		{
			"type": "section",
			"fields": [
				{
					"type": "mrkdwn",
					"text": "*Common Name:* {{ $('Parse Webhook').item.json.response.message.blocks[2].text.text.match(/\*Domain:\*\s*<http[^|]+\|([^\n]+)>/)[1] }}"
				},
				{
					"type": "mrkdwn",
					"text": "*Organization:* n8n.io"
				},
				{
					"type": "mrkdwn",
					"text": "*Issued By:* Venafi CA"
				},
				{
					"type": "mrkdwn",
					"text": "*Validity Period:* {{ DateTime.fromISO($json.creationDate).toFormat('MMMM dd, yyyy') }} to {{ DateTime.fromISO($json.creationDate).plus({ years: 1 }).toFormat('MMMM dd, yyyy') }}"
				}
			]
		},
		{
			"type": "divider"
		},
		{
			"type": "actions",
			"elements": [
				{
					"type": "button",
					"text": {
						"type": "plain_text",
						"text": "View CSR Details"
					},
					"url": "https://eval-32690260.venafi.cloud/issuance/certificate-requests?id={{ $json.id }}",
					"style": "primary"
				},
				{
					"type": "button",
					"text": {
						"type": "plain_text",
						"text": "Revoke CSR"
					},
					"style": "danger",
					"value": "revoke_csr"
				}
			]
		}
	]
}
```

----------------------------------------

TITLE: Identify Missing Fields - n8n JavaScript
DESCRIPTION: This JavaScript snippet identifies fields that need to be updated. It retrieves the current row data from the 'Row Ref' node and the list of potential fields from the 'Get Prompt Fields' node. It filters the fields list to find items that have a 'description' property but lack a corresponding value in the row data. The output is an array of these 'missing' field objects. It depends on the output of the 'Row Ref' and 'Get Prompt Fields' nodes.
SOURCE: https://github.com/egouilliard/n8n_examples/blob/main/examples/AI Data Extraction with Dynamic Prompts and Airtable.txt#_snippet_1

LANGUAGE: JavaScript
CODE:
```
const row = $('Row Ref').first().json;
const fields = $('Get Prompt Fields').first().json.fields;
const missingFields = fields
 .filter(field => field.description && !row[field.name]);

return missingFields;
```

----------------------------------------

TITLE: Configuring OpenAI Chat Completions Body (JSON/n8n)
DESCRIPTION: Defines the JSON request payload for calling the OpenAI chat completions API. It specifies the model ('gpt-4o-mini'), includes a system message (prompt) and a user message (encoded CV text), and requests the response in a JSON format structured according to a provided JSON schema. The prompt and schema are retrieved from the 'Set Variables' node's output, and the user message uses the 'text' property from the current input data, URL-encoded and stringified.
SOURCE: https://github.com/egouilliard/n8n_examples/blob/main/examples/CV Screening with OpenAI.txt#_snippet_1

LANGUAGE: JSON
CODE:
```
={
 "model": "gpt-4o-mini",
 "messages": [
 {
 "role": "system",
 "content": "{{ $('Set Variables').item.json.prompt }}"
 },
 {
 "role": "user",
 "content": {{ JSON.stringify(encodeURIComponent($json.text))}}
 }
 ],
 "response_format":{ "type": "json_schema", "json_schema": {{ $('Set Variables').item.json.json_schema }}

 }
 }
```

----------------------------------------

TITLE: Process Google Sheet Config Data (JavaScript)
DESCRIPTION: This snippet processes data received from the 'fetchConfig' node, which is expected to be a list of key-value pairs read from a Google Sheet. It iterates through each item and transforms the list into a single JavaScript object where the 'Key' property of each item becomes the object key and the 'Value' property becomes the object value, making configuration parameters easily accessible for subsequent nodes.
SOURCE: https://github.com/egouilliard/n8n_examples/blob/main/examples/Author and Publish Blog Posts From Google Sheets.txt#_snippet_0

LANGUAGE: JavaScript
CODE:
```
let a = $("fetchConfig").all();
let params = {};
a.forEach(p => params[p.json.Key] = p.json.Value);

return params;
```

----------------------------------------

TITLE: Generate Slack Blocks for Negative Sentiment Issues - n8n Expression
DESCRIPTION: This complex n8n expression generates the `blocks` payload for a Slack message. It defines an array of Slack block objects, including a static header and divider. It dynamically generates multiple section blocks by mapping over items from the 'Deduplicate Notifications' node, formatting Linear issue details (ID, Title, Summary) into Markdown links and text for each item.
SOURCE: https://github.com/egouilliard/n8n_examples/blob/main/examples/Sentiment Analysis Tracking on Support Issues with Linear and Slack (1).txt#_snippet_9

LANGUAGE: n8n Expression
CODE:
```
={{
{
 "blocks": [
 {
 "type": "section",
 "text": {
 "type": "mrkdwn",
 "text": ":rotating_light: The following Issues transitioned to Negative Sentiment"
 }
 },
 {
 "type": "divider"
 },
 ...($('Deduplicate Notifications').all().map(item => (
 {
 "type": "section",
 "text": {
 "type": "mrkdwn",
 "text": `*<https://linear.app/myOrg/issue/${$json.fields['Issue ID']}|${$json.fields['Issue ID']} ${$json.fields['Issue Title']}>*\n${$json.fields.Summary}`
 }
 }
 )))
 ]
}
}}
```

----------------------------------------

TITLE: Accessing Form Submission Data - Email (N8N Expression)
DESCRIPTION: This expression retrieves the 'Email' field from the data item output by the node named 'On form submission'. It's used to map form data to other nodes or services within the workflow.
SOURCE: https://github.com/egouilliard/n8n_examples/blob/main/examples/HR Job Posting and Evaluation with AI.txt#_snippet_8

LANGUAGE: N8N Expression
CODE:
```
={{ $('On form submission').item.json.Email }}
```

----------------------------------------

TITLE: Formatting LLM Prompt for JSON Output (n8n Expression)
DESCRIPTION: Defines the prompt instruction for the Ollama LLM, guiding it to format its output as a JSON object containing the user's prompt and the LLM's response. It explicitly requests no preamble. The user input is injected using `{{ $json.chatInput }}`.
SOURCE: https://github.com/egouilliard/n8n_examples/blob/main/examples/🔐🦙🤖 Private & Local Ollama Self-Hosted AI Assistant.txt#_snippet_0

LANGUAGE: n8n Expression
CODE:
```
=Provide the users prompt and response as a JSON object with two fields:\n- Prompt\n- Response\n\nAvoid any preample or further explanation.\n\nThis is the question: {{ $json.chatInput }}
```

----------------------------------------

TITLE: Set Authorization Header for Adobe API (Process Query) - n8n Expression
DESCRIPTION: Sets the `Authorization` header for the HTTP request node that initiates the Adobe PDF Services operation. It dynamically includes a Bearer token retrieved from the authentication node's output (`Authenticartion (get token)`), required for the process request.
SOURCE: https://github.com/egouilliard/n8n_examples/blob/main/examples/Manipulate PDF with Adobe developer API.txt#_snippet_4

LANGUAGE: n8n Expression
CODE:
```
=Bearer {{ $('Authenticartion (get token)').first().json["access_token"] }}
```