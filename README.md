# AI-Powered Academic Notes Summarizer

A Flask web application that turns student-provided lecture notes and text-based PDF documents into structured study material using the Groq API. It creates a summary, concepts, definitions, important points, exam questions, quick revision notes, and key takeaways.

> **Accuracy note:** The application instructs the model to use only the submitted material and to identify information that is not present. AI output should still be reviewed against the original notes before an examination.

## Features

- Paste academic notes or upload a PDF (or provide both).
- Select Short Summary, Detailed Summary, or Exam Preparation Notes.
- Generate output in English or Urdu.
- Extract PDF text using PyMuPDF.
- Create seven organized study sections, including 5 questions in each of three question categories.
- Copy the complete study guide or download it as a `.txt` file.
- Responsive interface for desktop, tablet, and mobile.
- API key is kept on the server in `.env`; it is never sent to browser JavaScript.
- No database; uploaded PDF content is processed in memory.

## Technology

- Python 3.10 or newer
- Flask
- Groq Python SDK and a Groq-supported chat model
- PyMuPDF (`fitz`)
- python-dotenv
- HTML, CSS, JavaScript, Bootstrap 5

## Project structure

```text
academic-notes-summarizer/
├── app.py
├── requirements.txt
├── .env.example
├── .gitignore
├── README.md
├── templates/
│   └── index.html
├── static/
│   ├── style.css
│   └── script.js
└── uploads/
    └── .gitkeep
```

## Requirements

1. Install Python 3.10 or newer from https://www.python.org/downloads/.
2. During Windows installation, select **Add Python to PATH**.
3. Obtain a Groq API key from https://console.groq.com/keys.
4. Internet access is required when generating a summary.

## Windows PowerShell setup

Open PowerShell in the project folder.

### 1. Create and activate a virtual environment

```powershell
py -m venv venv
venv\Scripts\activate
```

If PowerShell blocks activation, run this in the same PowerShell window and activate again:

```powershell
Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass
venv\Scripts\activate
```

### 2. Install dependencies

```powershell
py -m pip install --upgrade pip
py -m pip install -r requirements.txt
```

### 3. Create your `.env` file

Copy the example file:

```powershell
Copy-Item .env.example .env
```

Open `.env` in Notepad:

```powershell
notepad .env
```

Replace the placeholder with your own key:

```env
GROQ_API_KEY=gsk_your_real_key_here
GROQ_MODEL=llama-3.3-70b-versatile
FLASK_DEBUG=false
```

Save and close Notepad. Do not share your key, paste it into frontend JavaScript, or commit `.env` to GitHub. The `.gitignore` file excludes it.

### 4. Run the application

```powershell
py app.py
```

Open this address in your browser:

http://127.0.0.1:5000

Keep the PowerShell window running while using the application. Press `Ctrl+C` to stop the server.

## How to use

1. Paste notes into the text box, select a PDF, or use both.
2. Choose a summary style and output language.
3. Click **Generate Summary**.
4. Wait for the result cards to appear.
5. Use **Copy Summary** or **Download .txt** to save your study guide.
6. **Generate Again** re-submits the current material and settings. **Clear All** resets the form and results.

## Limits and document notes

- Maximum PDF upload size: 12 MB.
- Maximum extracted/pasted source length: 100,000 characters.
- Only `.pdf` files are accepted.
- Text-based PDFs are supported. Image-only/scanned PDFs do not contain extractable text; run OCR on them before uploading.
- Encrypted/password-protected PDFs are rejected.
- Pasted text and extracted PDF text are combined when both are provided.
- The `uploads/` directory is included for the requested project structure, but uploaded files are not permanently saved there.

## Common errors

| Message / symptom | What to do |
|---|---|
| `GROQ_API_KEY is missing` | Create `.env` from `.env.example` and add your valid key. |
| Groq authentication failed | Check the key in `.env`; ensure it has no quotes or extra spaces. Restart `py app.py` after editing `.env`. |
| Rate limit reached | Wait and retry. Check your Groq account limits. |
| Could not connect to Groq | Check internet access, firewall, and Groq service availability. |
| No readable text found in PDF | The PDF may be scanned/image-only. OCR it first, then upload the searchable PDF. |
| Unsupported file type | Upload a PDF, not a Word document or image. |
| Upload too large | Use a PDF smaller than 12 MB. |
| `py` is not recognized | Reinstall Python and enable the Python launcher / PATH option. |
| `No module named ...` | Activate `venv` and run `py -m pip install -r requirements.txt`. |
| Port 5000 is already in use | Close the other app using port 5000, then run this app again. |

## API and privacy

The browser sends notes and selected options to this Flask server. The server sends the material to Groq to generate the requested output. The Groq API key remains server-side. This starter version does not use a database or persist uploaded PDFs. Do not submit confidential, personal, or restricted academic material unless you are permitted to share it with the configured AI provider.

## Validation and testing

Basic local checks:

```powershell
py -m py_compile app.py
py -m pip check
```

You can test empty-input validation without a Groq key:

```powershell
py -c "from app import app; c=app.test_client(); r=c.post('/api/summarize', data={}); print(r.status_code, r.json)"
```

The expected result is HTTP `400` with an empty-input message.

PDF extraction can be checked with a readable PDF using the web form. Full live Groq integration requires a valid API key, internet connectivity, and an available model on your Groq account. No mock/fake AI output is returned when the key is missing or the API fails.

## Model configuration

The default model is `llama-3.3-70b-versatile`. If it is unavailable to your account, set `GROQ_MODEL` in `.env` to a currently supported chat model listed in your Groq console/model documentation.

## License

For educational and personal project use. Review Groq's current terms and model availability before deployment.
