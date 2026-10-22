Cleaning_app - README.md
A Django-powered web application that automates messy CSV data cleaning using the OpenAI Agents SDK and Batch API, featuring dynamic cleaning profiles, human-in-the-loop QA reviews, and gold-set accuracy validation.
Table of Contents
•	About the Project
•	Key Features
•	Tech Stack
•	Project Architecture & Data Flow
•	Getting Started
•	Usage Guide
•	Cost & Safety Notes
•	Testing & CI
•	Roadmap
•	License
About the Project
CleanSheet solves the common data-ingestion pain point where customer exports and CRM uploads contain unstandardized, messy entries (such as mixed-case names, malformed phone numbers, typo-ridden emails, erratic date formats, and hidden data values).
Instead of hard-coding rigid scripts for a single dataset, CleanSheet uses a Profiler Agent to dynamically analyze uploaded CSV columns, suggest appropriate cleaning operations from a flexible operation library, and process rows efficiently and cost-effectively through the OpenAI Batch API.
Key Features
•	Dynamic Profiler Agent: Analyzes samples and column statistics using the OpenAI Agents SDK and Pydantic structured outputs to propose a custom cleaning plan.
•	Configurable Cleaning Rules: Users can review, toggle, edit, or add custom plain-English rules per upload without altering core code paths.
•	Asynchronous Batch Processing: Leverages the OpenAI Batch API with custom row IDs (custom_id) to process up to thousands of rows asynchronously over a 24-hour window at reduced costs.
•	Human-in-the-Loop QA Review: Side-by-side review interface allowing operators to inspect model flags, override decisions, and verify rows before final export.
•	Gold-Set Accuracy Reporting: Evaluates processed output against a hidden gold standard dataset to generate detailed per-column accuracy and error metrics.
Tech Stack
•	Backend: Python 3.12+, Django, Django REST / HTML Views
•	AI & Integration: OpenAI Agents SDK, OpenAI Batch API, Pydantic (v2)
•	Database: SQLite (Development) / PostgreSQL (Production)
•	Testing & Quality: pytest-django, ruff, mypy, GitHub Actions CI
•	Package Management: uv / pip
Project Architecture & Data Flow
[Upload CSV] -> [Profiler Agent (Agents SDK)] -> [User Customization & Profile Generation] -> [JSONL Batch Builder] -> [OpenAI Batch API (Async)] -> [Result Ingestion & Polling] -> [QA Review & Human Overrides] -> [Cleaned CSV & Accuracy Report]
Getting Started
Prerequisites
•	Python 3.12+
•	Git
•	Docker (optional, for containerized deployments)
•	An OpenAI project-scoped API key with a designated spend limit
Installation & Setup
1. Clone the repository:
   git clone https://github.com/your-username/cleansheet.git
   cd cleansheet
2. Set up the virtual environment and install dependencies:
   python -m venv .venv
   source .venv/bin/activate  # On Windows use: .venv\Scripts\activate
   pip install -r requirements.txt
3. Run database migrations:
   python manage.py migrate
4. Create a superuser to access the Django admin:
   python manage.py createsuperuser
Configuration (Environment Variables)
Create a .env file in the project root directory (never commit this file to version control):
DEBUG=True
SECRET_KEY=your-django-secret-key
OPENAI_API_KEY=your-project-scoped-openai-api-key
Usage Guide
1. Start the development server: python manage.py runserver
2. Navigate to http://127.0.0.1:8000/ in your browser.
3. Upload a messy CSV dataset through the interface.
4. Review the Profiler Agent's suggested cleaning plan, adjust options or add custom rules if needed, and approve the profile.
5. Submit the batch, monitor status via the polling command (python manage.py poll_batches), and inspect results in the QA review queue once complete.
6. Export the final cleaned CSV file along with its accuracy report.
Cost & Safety Notes
API Cost Efficiency: CleanSheet utilizes the OpenAI Batch API to process rows at a significantly reduced cost compared to synchronous real-time calls. Token usage and cost per 1,000 rows are logged per batch job.
Data Privacy: Only synthetic or sanitized datasets are sent to OpenAI. API keys are strictly managed via environment variables and GitHub Actions secrets, ensuring they are never exposed in source code, logs, or UI screenshots.
Testing & CI
CleanSheet includes a robust test suite utilizing mocked API clients to ensure tests run completely offline without hitting live endpoints.
Run tests: pytest
Type checking & linting: mypy . && ruff check .
Roadmap
•	Version 0.9.0: Core batch ingestion, QA review overrides, and unseen CSV generalization.
•	Version 1.0.0: Full polished release with Docker support and comprehensive metric reporting.
•	Automated webhook notifications upon batch completion.
License
Distributed under the MIT License. See LICENSE for more information.
