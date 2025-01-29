

## Prerequisites
- Anaconda or Miniconda installed on your system
- Python 3.10 or higher
- Git (optional, for cloning the repository)

## Installation
Follow these steps to set up and run the application on your local machine.

1. Clone the Repository
   ```bash
   git clone https://github.com/PromtEngineer/localGPT-Vision.git
   cd localGPT-Vision
   ```

2. Create a Conda Environment
   ```bash
   conda create -n localgpt-vision python=3.10
   conda activate localgpt-vision
   ```

3a. Install Dependencies
   ```bash
   pip install -r requirements.txt
   ```

3b. Install Transformers from HuggingFace - Dev version
   ```bash
    pip uninstall transformers
    pip install git+https://github.com/huggingface/transformers
   ```

4. Set Environment Variables
   Set your API keys for Google Gemini and OpenAI GPT-4:

   ```bash
   export GENAI_API_KEY='your_genai_api_key'
   export OPENAI_API_KEY='your_openai_api_key'
   export GROQ_API_KEY='your_groq_api_key'
   ```

   On Windows Command Prompt:
   ```cmd
   set GENAI_API_KEY=your_genai_api_key
   set OPENAI_API_KEY=your_openai_api_key
   set GROQ_API_KEY='your_groq_api_key'
   ```

5. Run the Application
   ```bash
   python app.py
   ```

6. Access the Application
   Open your web browser and navigate to:
   ```
   http://localhost:5050/
   ```

## Usage
### Upload and Index Documents
1. Click on "New Chat" to start a new session.
2. Under "Upload and Index Documents", click "Choose Files" and select your PDF or image files.
3. Click "Upload and Index". The documents will be indexed using ColPali and ready for querying.

### Ask Questions
1. In the "Enter your question here" textbox, type your query related to the uploaded documents.
2. Click "Send". The system will retrieve relevant document pages and generate a response using the selected Vision Language Model.

### Manage Sessions
- Rename Session: Click "Edit Name", enter a new name, and click "Save Name".
- Switch Sessions: Click on a session name in the sidebar to switch to that session.
- Delete Session: Click "Delete" next to a session to remove it permanently.

### Settings
1. Click on "Settings" in the navigation bar.
2. Select the desired language model and image dimensions.
3. Click "Save Settings".

## Project Structure
```
localGPT-Vision/
├── app.py
├── logger.py
├── models/
│   ├── indexer.py
│   ├── retriever.py
│   ├── responder.py
│   ├── model_loader.py
│   └── converters.py
├── sessions/
├── templates/
│   ├── base.html
│   ├── chat.html
│   ├── settings.html
│   └── index.html
├── static/
│   ├── css/
│   │   └── style.css
│   ├── js/
│   │   └── script.js
│   └── images/
├── uploaded_documents/
├── byaldi_indices/
├── requirements.txt
├── .gitignore
└── README.md
```

- `app.py`: Main Flask application.
- `logger.py`: Configures application logging.
- `models/`: Contains modules for indexing, retrieving, and responding.
- `templates/`: HTML templates for rendering views.
- `static/`: Static files like CSS and JavaScript.
- `sessions/`: Stores session data.
- `uploaded_documents/`: Stores uploaded documents.
- `.byaldi/`: Stores the indexes created by Byaldi.
- `requirements.txt`: Python dependencies.
- `.gitignore`: Files and directories to be ignored by Git.
- `README.md`: Project documentation.

## System Workflow
1. User Interaction: The user interacts with the web interface to upload documents and ask questions.
2. Document Indexing with ColPali:
   - Uploaded documents are converted to PDFs if necessary.
   - Documents are indexed using ColPali, which creates embeddings based on the visual content of the document pages.
   - The indexes are stored in the byaldi_indices/ directory.
3. Session Management:
   - Each chat session has a unique ID and stores its own index and chat history.
   - Sessions are saved on disk and loaded upon application restart.
4. Query Processing:
   - User queries are sent to the backend.
   - The query is embedded and matched against the visual embeddings of document pages to retrieve relevant pages.
5. Response Generation with Vision Language Models:
   - The retrieved document images and the user query are passed to the selected Vision Language Model (Qwen, Gemini, or GPT-4).
   - The VLM generates a response by understanding both the visual and textual content of the documents.
6. Display Results:
   - The response and relevant document snippets are displayed in the chat interface.

```mermaid
graph TD
    A[User] -->|Uploads Documents| B(Flask App)
    B -->|Saves Files| C[uploaded_documents/]
    B -->|Converts and Indexes with ColPali| D[Indexing Module]
    D -->|Creates Visual Embeddings| E[byaldi_indices/]
    A -->|Asks Question| B
    B -->|Embeds Query and Retrieves Pages| F[Retrieval Module]
    F -->|Retrieves Relevant Pages| E
    F -->|Passes Pages to| G[Vision Language Model]
    G -->|Generates Response| B
    B -->|Displays Response| A
    B -->|Saves Session Data| H[sessions/]
    subgraph Backend
        B
        D
        F
        G
    end
    subgraph Storage
        C
        E
        H
    end
```


