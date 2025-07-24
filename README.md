# Dynamic Laravel Project Creator

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Python Version](https://img.shields.io/badge/python-3.8+-blue.svg)](https://www.python.org/downloads/)
[![Laravel Version](https://img.shields.io/badge/laravel-10.x-orange.svg)](https://laravel.com/)

An intelligent tool that dramatically accelerates the setup of a Laravel API backend. Define your entire database structure in a simple JSON file, and this tool will automatically generate:
-   **Models** with relationships, UUIDs, and fillable attributes.
-   **Migrations** with correct column types, foreign keys, and pivot tables.
-   **API Controllers** with full CRUD (Create, Read, Update, Delete) functionality.
-   **Form Requests** for robust, automatic validation.
-   **API Routes** to wire everything up.

All through a user-friendly Streamlit web interface that includes schema validation and a helpful Q&A assistant.

---

###  Key Features

-   **Schema-First Development**: Define your database schema in a single, intuitive JSON file.
-   **Intelligent Code Generation**: Automatically generates Eloquent Models, Migrations, API Controllers, and Form Requests.
-   **User-Friendly UI**: A Streamlit-powered web interface for easy project creation.
-   **Built-in Schema Validator**: Catches errors in your schema *before* generation, ensuring clean output.
-   **Relationship Aware**: Understands and correctly implements `belongsTo`, `hasMany`, `hasOne`, and `belongsToMany` relationships, including pivot tables.
-   **Modern Practices**: Enforces best practices like UUID primary keys, snake_case naming, and pluralized table names.
-   **Passport Ready**: Optionally install and configure Laravel Passport for API authentication with a single click.
-   **Q&A Assistant**: A semantic search tool to quickly find answers about schema rules from the documentation.


###  The Workflow

1.  **Define**: Create a `schema.json` file detailing your application's tables, columns, and relationships.
2.  **Validate**: Upload your schema to the Streamlit UI to validate its structure and rules.
3.  **Configure**: Fill in your Laravel project name, path, and database details.
4.  **Generate**: Click "Create Project".
5.  **Develop**: `cd` into your fully-scaffolded Laravel project and start building your application's logic.

---

<h3> What Gets Generated?</h3>
<p>Given a simple JSON schema, the tool generates a complete, ready-to-use API backend.</p>

<table>
<thead>
  <tr>
    <th>From this JSON Schema...</th>
    <th>...to these PHP files:</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td valign="top">
<pre><code>{
  "posts": {
    "uuid_id": "uuid",
    "title": "string",
    "content": "text",
    "user_id": "uuid",
    "relations": {
      "users": "belongsTo"
    }
  }
}
</code></pre>
    </td>
    <td valign="top">
      <strong>Model</strong> (<code>app/Models/Post.php</code>): A Post model with <code>$fillable</code> attributes, a <code>user()</code> relationship method, and UUID configuration.<br><br>
      <strong>Migration</strong> (<code>database/migrations/...</code>): A migration to create the <code>posts</code> table with <code>uuid_id</code>, <code>title</code>, <code>content</code>, and a foreign key constraint for <code>user_id</code>.<br><br>
      <strong>Controller</strong> (<code>app/Http/Controllers/API/PostController.php</code>): A resource controller with <code>index</code>, <code>store</code>, <code>show</code>, <code>update</code>, and <code>destroy</code> methods.<br><br>
      <strong>Form Request</strong> (<code>app/Http/Requests/PostRequest.php</code>): A request class with validation rules for <code>title</code> and <code>content</code>.<br><br>
      <strong>API Route</strong> (<code>routes/api.php</code>): <code>Route::apiResource('posts', PostController::class);</code>
    </td>
  </tr>
</tbody>
</table>

---

###  Prerequisites

Before you begin, ensure you have the following installed on your system:
-   **PHP** (>= 8.1)
-   **Composer**
-   **Python** (>= 3.8)
-   **pip** (Python package installer)

###  Installation & Setup

1.  **Clone the repository:**
    ```bash
    git clone https://github.com/your-username/dynamic-laravel-project-creator.git
    cd dynamic-laravel-project-creator
    ```

2.  **Create and activate a Python virtual environment (recommended):**
    ```bash
    # For Windows
    python -m venv venv
    .\venv\Scripts\activate

    # For macOS/Linux
    python3 -m venv venv
    source venv/bin/activate
    ```

3.  **Install the required Python dependencies:**
    ```bash
    pip install -r requirements.txt
    ```

4.  **Build the backend executable:**
    This script uses PyInstaller to package the core Laravel generation logic into a standalone executable.
    ```bash
    python build_executable.py
    ```
    This will create a `laravel_project_creator.exe` (or equivalent for your OS) inside the `dist` directory.

    > **Note:** The current `app.py` script has a hardcoded path to the executable (`C:\\Valarr\\laravel_project_creator.exe`). You will need to update this path to match the location of the executable in your `dist` folder.

---

###  How to Use

1.  **Run the Streamlit application:**
    ```bash
    streamlit run app.py
    ```
    Your web browser should open with the application's UI.

2.  **Validate Your Schema:**
    -   On the **Schema Validator** tab, upload your JSON schema file.
    -   Click "Validate Schema". The application will report any errors or warnings.
    -   You can only proceed to project creation if the schema is valid and has no warnings.

3.  **Create the Laravel Project:**
    -   Once validated, the "Create Laravel Project" button will be enabled.
    -   Fill in the **Project Name**, **Project Path**, **Database Name**, and choose whether to include Laravel **Passport**.
    -   Click "Create Project". The terminal output of the generation process will be streamed to the UI in real-time.

4.  **Explore Schema Rules:**
    -   Navigate to the **Q&A** tab to ask questions about the schema rules (e.g., "how to define a many-to-many relationship?").

---

###  Schema Definition Guide

The JSON schema is the heart of this tool. It is a dictionary where keys are **plural, snake_case table names**.

#### Basic Structure
```json
{
  "table_one_name": {
    "column_name": "data_type:modifier",
    "..."
  },
  "table_two_name": {
    "column_name": "data_type",
    "relations": {
      "table_one_name": "relationship_type"
    }
  }
}
```

#### Key Rules
-   **Primary Key**: Every table **must** have a `"uuid_id": "uuid"` primary key.
-   **Data Types**: Supported types are `string`, `text`, `integer`, `decimal(p,s)`, `boolean`, `uuid`, `email`, `enum(a,b,c)`.
-   **Modifiers**: Supported modifiers are `:unique`, `:nullable`, `:default('value')`.
-   **Foreign Keys**: Foreign key columns must be of type `uuid` and follow the `singular_table_name_id` convention (e.g., `user_id`).
-   **Relationships**: The `relations` key holds an object defining relationships. Supported types are `belongsTo`, `hasMany`, `hasOne`, and `belongsToMany`.

#### Full Example

```json
{
  "users": {
    "uuid_id": "uuid",
    "name": "string",
    "email": "email:unique",
    "password": "string"
  },
  "posts": {
    "uuid_id": "uuid",
    "title": "string:nullable",
    "content": "text",
    "status": "enum(draft,published,archived):default('draft')",
    "user_id": "uuid",
    "relations": {
      "users": "belongsTo",
      "tags": "belongsToMany"
    }
  },
  "tags": {
    "uuid_id": "uuid",
    "name": "string:unique",
    "relations": {
      "posts": "belongsToMany"
    }
  }
}
```
---

###  Contributing

Contributions are welcome! If you have suggestions for improvements, please feel free to open an issue or submit a pull request.

1.  Fork the Project
2.  Create your Feature Branch (`git checkout -b feature/AmazingFeature`)
3.  Commit your Changes (`git commit -m 'Add some AmazingFeature'`)
4.  Push to the Branch (`git push origin feature/AmazingFeature`)
5.  Open a Pull Request

###  License

Distributed under the MIT License. See `LICENSE` for more information.
