# 🖥️ Local Dev Setup

Guidance for installing dependencies and running the data product environment locally.

## 🎯 Objectives

- Enable developers to quickly get a fully functional local environment.
- Ensure consistency across team members.
- Facilitate efficient debugging and feature development.

---

## ⚙️ Prerequisites

- Supported OS: Windows, macOS, Linux
- Installed tools:
  - Python (version X.X or higher)
  - Package manager (pip, conda)
  - Git
  - Docker (optional, for containerized services)
  - Database clients or CLI tools (if applicable)

---

## 🔧 Setup Steps

1. **Clone the repository**
   ```bash
   git clone <repository-url>
   cd <repository-folder>
   ```
2. Create and activate a virtual environment
   ```bash
   python -m venv env
   source env/bin/activate  # macOS/Linux
   .\env\Scripts\activate   # Windows
   ```
3. Install dependencies
   ```bash
   pip install -r requirements.txt
   ```
4. Configure environment variables
   - Copy `.env.example` to `.env`
   - Update `.env` with local credentials and settings
5. Start local services
   - Run any required local databases or mocks
   - Start local API servers or workers as needed
6. Run tests
   ```bash
   pytest tests/
   ```

## 🧰 Tools and Tips
- Use pre-commit hooks to enforce code style
- Utilize Docker Compose for complex local setups
- Keep dependencies updated regularly
- Document any local setup quirks in this guide

## 🚩 Troubleshooting
- Check environment variable correctness
- Confirm required services are running
- Review logs for errors
- Sync with team for environment changes

## 📌 Summary
A clear local dev setup ensures smooth onboarding, consistent environments, and faster development cycles.
