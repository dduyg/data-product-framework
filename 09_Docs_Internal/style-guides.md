# 🎨 Style Guides

Consistent coding, SQL, and naming conventions to maintain code quality and readability.

## 👩‍💻 Coding Conventions

- Follow [PEP 8](https://pep8.org/) for Python style:
  - Use snake_case for variables and functions.
  - Use PascalCase for classes.
  - Limit line length to 79 characters.
  - Add docstrings to all public functions and classes.
- Write clear and descriptive variable and function names.
- Use type hints for function signatures where applicable.
- Avoid deep nesting; refactor complex logic into smaller functions.
- Keep functions short and focused on a single task.

---

## 💾 SQL Conventions

- Use uppercase for SQL keywords (e.g., `SELECT`, `FROM`, `WHERE`).
- Use lowercase for table and column names, following naming conventions.
- Format queries with indentation for readability:
  ```sql
  SELECT
      column1,
      column2
  FROM
      table_name
  WHERE
      condition = value;
  ```
- Avoid `SELECT *`; explicitly list columns.
- Use CTEs (WITH clauses) for complex queries to improve readability.
- Add comments to explain complex SQL logic.

---

## 🏷️ Naming Conventions
- Use clear, descriptive names that reflect purpose.
- For tables:
  - Use singular nouns (e.g., user, order).
  - Prefix staging or temporary tables with `stg_` or `tmp_`.
- For columns:
  - Use snake_case (e.g., `created_at`, `user_id`).
  - Avoid abbreviations unless commonly understood.
- For variables and functions in code:
  - Use snake_case for variables and functions.
  - Use PascalCase for classes and exceptions.

---

## 🛠️ Tools and Automation
- Use linters and formatters (e.g., `flake8`, `black` for Python; `sqlfluff` for SQL).
- Integrate style checks into CI pipelines.
- Encourage code reviews focusing on style and readability.

---

## 📌 Summary
Adhering to consistent style guides improves maintainability, reduces bugs, and facilitates collaboration.
