# 📄 Cover_letter_AI_Agent

### 🔧 What It Does

This n8n workflow automates the creation of a customized cover letter by using an **AI agent**. It accepts:

* 📝 A resume (as plain text)
* 📄 A job description (as plain text)

...and returns:

* ✅ A professionally written, tailored cover letter in response.

---

### 📦 Components Used

* **Webhook Node**: Accepts POST requests with JSON containing `resume` and `job_description`.
* **AI Agent Node**: Sends the input to an AI agent (ChatGPT or similar) with a prompt to generate a cover letter.
* **Set Node**: Formats and saves the AI response.
* **(Optional) Code Node**: Cleans up Markdown formatting from the AI's output using Python.

---

### 📥 Input Format

You send a POST request like this:

```bash
curl -X POST "http://localhost:5678/webhook/cover-letter" \
  -H "Content-Type: application/json" \
  -d '{
        "resume": "Skilled Python developer with a passion for NLP and cloud technologies. Strong experience in AWS and building AI pipelines.",
        "job_description": "We are seeking a Cloud Engineer with expertise in NLP, automation, and Python."
      }'
```

---

### 📤 Output Format

You get a response like:

```json
[
  {
    "cover_letter": "Dear Hiring Manager, I am writing to express my interest in the Cloud Engineer position..."
  }
]
```

If you added a cleanup step using a **Code Node**, the final output is a clean, readable paragraph.

---

### 🧠 How the AI Agent Works

The AI Agent receives a prompt like:

> "You are a professional cover letter assistant. Given a resume and a job description, your task is to generate a customized and impressive cover letter.\n\nResume: {{ \$json.resume }}\nJob Description: {{ \$json.job\_description }}\n\nPlease write the full cover letter now:"

This guides the AI to create a personalized, job-targeted letter based on the two inputs.

---

### 🔍 Cleaning the Output (Optional)

A Code Node can be used to clean Markdown formatting from the AI response:

```python
import re

def clean_cover_letter(text):
    if not text:
        return ""

    cleaned = re.sub(r'[*#`]', '', text)                 # Remove *, #, `
    cleaned = re.sub(r'\[.*?\]', '', cleaned)            # Remove anything inside []
    cleaned = re.sub(r'\\n|\\r|\\t|\n', ' ', cleaned)     # Replace newlines and tabs with space
    cleaned = re.sub(r'\s+', ' ', cleaned)               # Collapse multiple spaces
    return cleaned.strip()

clean_cover_letter($json.cover_letter)
```

---

### 💻 Local Testing

Start the n8n instance and test the webhook with a tool like Postman or cURL. Make sure the workflow is active and the webhook URL matches what you call.

---

### 📝 Notes

* The AI Agent must be correctly authenticated (e.g., OpenAI API key if using that).
* Input should be clean, well-structured text for best results.
* This setup doesn't support PDF input directly — it's designed for structured text only.

---

Would you like this as a downloadable `.md` file or uploaded to GitHub later?
