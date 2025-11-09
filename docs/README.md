# Flask PWA - API Extension Task

This task is to build a safe [RESTful API](https://blog.hubspot.com/website/what-is-rest-api) that extends the [Flask PWA - Programming for the Web Task](https://github.com/TempeHS/Flask_PWA_Programming_For_The_Web_Task_Template). From the parent task, students will abstract the database and management to an REST API with key authentication. The PWA will then be retooled to GET request the data from the REST API and POST request data to the REST API. The PWA UI for the API will be rapidly prototyped using the [Bootstrap](https://getbootstrap.com/) frontend framework.

The API instructions focus on modelling how to build and test an API incrementally. The PWA instructions focus on using the [Bootstrap](https://getbootstrap.com/) frontend framework to prototype an enhanced UI/UX frontend rapidly using [Bootstrap](https://getbootstrap.com/) components and classes.

> [!note]
> The template for this project has been pre-populated with assets from the Flask PWA task, including the logo, icons and database. Students can migrate their own assets if they wish.

## Dependencies

## Requirements

1. [VSCode](https://code.visualstudio.com/download) or [GitHub Codespaces](https://github.com/features/codespaces)
2. [Python 3.x](https://www.python.org/downloads/)
3. [GIT 2.x.x +](https://git-scm.com/downloads)
4. [SQLite3 Editor](https://marketplace.visualstudio.com/items?itemName=yy0931.vscode-sqlite3-editor)
5. [Start git-bash](https://marketplace.visualstudio.com/items?itemName=McCarter.start-git-bash) 6.[Thunder Client](https://marketplace.visualstudio.com/items?itemName=rangav.vscode-thunder-client)
6. pip/pip3 installs

```bash
    pip install Flask
    pip install flask_cors
    pip install flask_limiter
    pip install flask_wtf
    pip install flask_csp
    pip install jsonschema
    pip install requests
```

> [!Important]
> MacOS and Linux users may have a `pip3` soft link instead of `pip`, run the below commands to see what path your system is configured with and use that command through the project. If neither command returns a version, then likely [Python 3.x](https://www.python.org/downloads/) needs to be installed.
>
> ```bash
> pip show pip
> pip3 show pip
> ```

## Using Git for Version Control

> [!Note]
> Your repository is already cloned and Git is configured. This section focuses on the essential Git workflow for tracking your development progress.

### Check your repository status

1. Verify you're in the correct directory:

```bash
pwd
```

2. Check what files have changed:

```bash
git status
```

### Making commits as you work

As you complete each step in the instructions, commit your changes:

1. Stage files you've modified:

```bash
git add api.py
```

2. Or stage multiple files:

```bash
git add api.py database_manager.py
```

3. Commit with a descriptive message:

```bash
git commit -m "Add basic API route for extensions"
```

### Working with branches (Optional)

If you want to experiment without affecting your main code:

1. Create and switch to a new branch:

```bash
git checkout -b feature/my-experiment
```

2. Work on your changes, then commit:

```bash
git add .
git commit -m "Test new feature"
```

3. Switch back to main branch:

```bash
git checkout main
```

4. If your experiment worked, merge it:

```bash
git merge feature/my-experiment
```

### Pushing your work to GitHub

1. Push your commits to GitHub:

```bash
git push
```

2. If pushing a new branch:

```bash
git push -u origin feature/my-experiment
```

### Commit message best practices

Use clear, descriptive commit messages:

```bash
git commit -m "feat: add POST endpoint for new extensions"
git commit -m "fix: resolve JSON validation error"
git commit -m "docs: update README with API examples"
```

For detailed commits, add a description:

```bash
git commit -m "feat: implement API authentication

- Add API key validation
- Configure rate limiting
- Add security logging"
```

### Viewing your history

See what you've accomplished:

```bash
git log --oneline
```

Or view the last 5 commits:

```bash
git log --oneline -5
```

> [!Tip]
> Commit regularly! A good rule is to commit whenever you complete a working feature or step in the instructions. This makes it easy to track your progress and undo changes if needed.

---

## Instructions for building the API

### Understanding the Architecture Shift

In your previous Flask PWA, everything ran in one application:

- `main.py` handled both database queries AND served web pages
- This works fine for small projects, but has limitations:
  - Hard to reuse data in other applications
  - Can't control who accesses your data
  - Difficult to update frontend without touching backend

**The API Architecture:**

- **API Server** (`api.py` on port 3000): Manages data & database
- **PWA Server** (`main.py` on port 5000): Serves web pages & handles user interface
- They communicate using HTTP requests (like talking over the internet)

**Real-World Analogy:**
Think of a restaurant:

- Kitchen (API) = Prepares food (data)
- Dining room (PWA) = Where customers interact
- Waiters (HTTP requests) = Carry orders and food between them

This means multiple "dining rooms" (mobile app, website, desktop app) can all use the same "kitchen" (API).

---

## Instructions for building the API

### Step 1: Learn the basics of implementing an API in Flask

Watch: [Build a Flask API in 12 Minutes](https://www.youtube.com/watch?v=zsYIw6RXjfM)

> [!Note]
> The video uses [Postman](https://www.postman.com/), this tutorial uses [Thunder Client](https://www.thunderclient.com/) a VS Code extension that has similar functionality, it does not work in a Codespace, you will need to use it in desktop version of VS Code.

### Step 2: Create the Directory Structure

Students can create files as they are needed. This structure defines the correct directory structure for all files. As students `touch` each file, they should refer to this structure to ensure the file path is correct.

```text
├── database
│   └─── data_source.db
├── static
│   ├── css
│   │   ├──bootstrap.min.css
│   │   └──style.css
│   ├── icons
│   │   ├──desktop_screenshot.png
│   │   ├──icon-128x128.png
│   │   ├──icon-192x192.png
│   │   ├──icon-384x384.png
│   │   ├──icon-512x512.png
│   │   └──mobile_screenshot.png
│   ├── images
│   │   ├──favicon.png
│   │   └──logo.png
│   ├─── js
│   │   ├──app.js
│   │   ├──bootstrap.bundle.min.js
│   │   └──serviceWorker.js
│   └── manifest.json
├── templates
│   ├── partials
│   │   ├──footer.html
│   │   └──menu.html
│   ├──index.html
│   ├──layout.html
│   └──privacy.html
├── api.py
├── database_manager.py
├── LICENSE
└── main.py
```

### Step 3: Setup a basic API in api.py

This Python implementation in 'api.py':

1. Imports all the required dependencies for the whole project.
2. Configure the 'Cross Origin Request' policy.
3. Configure the rate limiter.
4. Configure a route for the root `/` with a GET method to return stub data and a 200 response.
5. Configure a route to /add_extension with a POST method to return stub data and a 201 response.

**Understanding CORS - Cross-Origin Resource Sharing:**

By default, browsers block requests between different "origins" (different ports/domains) for security.

- Your PWA runs on `http://localhost:5000`
- Your API runs on `http://localhost:3000`
- These are different origins!

**Why do we need CORS?**
Without CORS, when your PWA tries to request data from the API, the browser will block it saying: "These are different servers, this might be dangerous!"

**The CORS Configuration:**

- `CORS(api)` = "Allow requests from other origins"
- `CORS_HEADERS` = "Allow Content-Type header" (needed for JSON)

**In Production:** You'd limit CORS to only allow YOUR specific PWA domain, not all origins.

**Understanding Rate Limiting:**

Rate limiting protects your API from abuse by limiting how many requests each user can make.

**Without Rate Limiting:**

- Someone could write a script to call your API millions of times
- This could crash your server or fill your database with junk
- Called a "Denial of Service" (DoS) attack

**How This Configuration Works:**

- `default_limits`: Everyone gets max 200 requests/day, 50/hour
- `@limiter.limit("3/second")`: This specific endpoint allows only 3 requests per second
- `get_remote_address`: Tracks limits per IP address
- `storage_uri="memory://"`: Stores counters in RAM (resets when server restarts)

**What Happens When Limit Exceeded?**
User receives `429 Too Many Requests` error.

```python
from flask import Flask
from flask import request
from flask_cors import CORS
from flask_limiter import Limiter
from flask_limiter.util import get_remote_address
import logging

import database_manager as dbHandler


api = Flask(__name__)
cors = CORS(api)
api.config["CORS_HEADERS"] = "Content-Type"
limiter = Limiter(
    get_remote_address,
    app=api,
    default_limits=["200 per day", "50 per hour"],
    storage_uri="memory://",
)


@api.route("/", methods=["GET"])
@limiter.limit("3/second", override_defaults=False)
def get():
    return ("API Works"), 200


@api.route("/add_extension", methods=["POST"])
@limiter.limit("1/second", override_defaults=False)
def post():
    data = request.get_json()
    return data, 201


if __name__ == "__main__":
    api.run(debug=True, host="0.0.0.0", port=3000)
```

### Step 3: Test your basic API with Thunder Client

![Screen recording testing an API with Thunder Client](/docs/README_resources/test_basic_API.gif "Follow these steps to test your basic API")

#### ✅ Checkpoint: Basic API Setup Complete

```bash
# Verify your API is working:
python api.py
# Should show: * Running on all addresses (0.0.0.0)
# Should show: * Running on http://127.0.0.1:3000

# Test in Thunder Client:
# 1. GET request to http://localhost:3000 → Should return "API Works" with status 200
# 2. POST request to http://localhost:3000/add_extension with JSON body → Should echo back your JSON with status 201
```

> **API not starting?** Check that Flask, flask_cors, and flask_limiter are installed: `pip list | grep -i flask`
>
> **Having issues?** See [🔧 Troubleshooting - API Server Issues](#-troubleshooting-common-issues)

---

### Understanding HTTP Status Codes

Every API response includes a status code that tells the client what happened.

**Common Status Codes You'll Use:**

| Code | Name                  | Meaning                        | When to Use                        |
| ---- | --------------------- | ------------------------------ | ---------------------------------- |
| 200  | OK                    | Request succeeded              | GET requests that return data      |
| 201  | Created               | New resource created           | POST requests that add to database |
| 400  | Bad Request           | Client sent invalid data       | JSON validation fails              |
| 401  | Unauthorised          | Missing/invalid authentication | Wrong API key                      |
| 404  | Not Found             | Resource doesn't exist         | URL endpoint not found             |
| 429  | Too Many Requests     | Rate limit exceeded            | User hits rate limit               |
| 500  | Internal Server Error | Server crashed                 | Your code has a bug                |

**Why They Matter:**

- Allows client (PWA) to handle different situations appropriately
- Standard way for all web services to communicate success/failure
- Makes debugging easier (specific error codes)

**Example:**

```python
return ("API Works"), 200  # Tell client: "Success! Here's your data"
return {"error": "Invalid JSON"}, 400  # Tell client: "You sent bad data"
return {"error": "Unauthorised"}, 401  # Tell client: "You're not allowed"
```

---

### Step 4: Build a basic GET response

**Understanding `with` Context Managers for Databases:**

In programming, we often need to manage resources, like files or database connections. A common pattern is "open, do something, close."

**The Old Way (Manual `con.close()`):**

```python
con = sql.connect("database/data_source.db")
# ... do database work ...
con.close()
```

This works, but what if an error happens before `con.close()` is called? The connection might be left open, which can cause problems like locking the database file.

**The Better Way (Using `with`):**
Python's `with` statement simplifies resource management. It automatically handles the setup and teardown, even if errors occur.

```python
with sql.connect("database/data_source.db") as con:
    # ... do database work ...
# The connection is automatically closed here!
```

**Why is `with` better?**

- **Safer:** It guarantees the database connection is closed, preventing resource leaks.
- **Cleaner:** The code is more readable and you don't have to remember to call `con.close()`.
- **Modern:** It's the standard, recommended way to work with resources in Python.

Think of it like borrowing a book from a library. The `with` statement is like a librarian who automatically checks the book back in for you when you leave, no matter what.

**Understanding `jsonify()` in APIs:**

In your previous PWA, you returned Python data directly to Jinja2 templates. In an API, you must return JSON text for HTTP responses.

**Without `jsonify`:**

```python
return {"name": "test"}
# Response: Python object (only works within Python)
```

**With `jsonify`:**

```python
return jsonify({"name": "test"})
# Response: '{"name": "test"}' (JSON text string)
# + Sets Content-Type: application/json header
# + Properly escapes special characters
```

**What's the Difference?**

| Aspect    | Python Dict     | JSON String (via `jsonify`)      |
| --------- | --------------- | -------------------------------- |
| Data Type | `dict` object   | Text string                      |
| Use       | Within Python   | Over HTTP/network                |
| Format    | Python-specific | Universal standard               |
| Headers   | None            | `Content-Type: application/json` |

Extend the `get():` method in `api.py` to get data from the database via the `dbHandler` and return it to the request with a status `200`.

```python
def get():
    content = dbHandler.extension_get("%")
    return (content), 200
```

This Python implementation in 'database_manager.py'

1. Imports all the required dependencies for the project
2. Connects to the SQLite3 database using a `with` context manager.
3. Executes a query.
4. Converts the query data to a JSON structure.
5. Returns the JSON data.

**Why Build Dictionaries from Rows?**

Instead of returning tuples like `(1, 'name', 'link')`, we build dictionaries for JSON:

```python
# Without dictionaries (tuple):
data = cur.fetchall()  # Returns: [(1, 'name', 'link'), (2, 'name2', 'link2')]

# With dictionaries:
migrate_data = [
    dict(extID=row[0], name=row[1], hyperlink=row[2], ...)
    for row in cur.fetchall()
]
# Returns: [{"extID": 1, "name": "name", "hyperlink": "link"}, ...]
```

**Why?**

- **Self-documenting**: JSON keys show what each value means
- **Order-independent**: Can access `data["name"]` instead of `data[1]`
- **Flexible**: Can add/remove fields without breaking client code

```python
from flask import jsonify
import sqlite3 as sql
from jsonschema import validate
from flask import current_app


def extension_get(lang):
    with sql.connect("database/data_source.db") as con:
        cur = con.cursor()
        cur.execute("SELECT * FROM extension WHERE language LIKE ?;", [lang])
        migrate_data = [
            dict(
                extID=row[0],
                name=row[1],
                hyperlink=row[2],
                about=row[3],
                image=row[4],
                language=row[5],
            )
            for row in cur.fetchall()
        ]
    return jsonify(migrate_data)
```

### Step 5: Test your basic GET Response

![Screen recording testing a API GET with Thunder Client](/docs/README_resources/test_basic_GET_API.gif "Follow these steps to test your basic GET API")

#### ✅ Checkpoint: Basic GET Response Working

```bash
# Verify your GET endpoint returns database data:
# 1. Ensure api.py is running: python api.py
# 2. Test in Thunder Client:
#    GET request to http://localhost:3000
#    Should return JSON array with all extensions from database
# 3. Check the response format is valid JSON
# 4. Verify all database fields appear in the response (extID, name, hyperlink, about, image, language)
```

> **No data returning?** Check that database_manager.py is in the same directory and data_source.db exists in the database folder.
>
> **Having issues?** See [🔧 Troubleshooting - Database Connection Issues](#-troubleshooting-common-issues)

---

### Step 6 Add a GET request argument to filter extensions by language

**Understanding Query Parameters:**

Query parameters are data sent in the URL after a `?` symbol. They're perfect for filtering, searching, and sorting.

**Example:**

```
http://127.0.0.1:3000?lang=python
                      └─ Query parameter: lang=python
```

**Use Query Parameters for:**

- Filtering data (search, sort, filter)
- Data visible in URL
- GET requests
- Bookmarkable URLs

**Security Note:** We validate that `lang` contains only alphabetic characters using `.isalpha()` to prevent SQL injection attacks!

Extend the `get():` method in `api.py` to either get all data or data that matches a language parameter from the database by

1. Validating the argument is "lang" and that the "lang" is only alpha characters for security.
2. Passing the language request to the dbHandler.
3. If no language is specified, the wildcard `%` will be passed.
4. Return the data from dbHandler to the request.
5. Return a status `200`.

```python
def get():
    # For security data is validated on entry
    if request.args.get("lang") and request.args.get("lang").isalpha():
        lang = request.args.get("lang")
        lang = lang.upper()
        content = dbHandler.extension_get(lang)
    else:
        content = dbHandler.extension_get("%")
    return (content), 200
```

Extend the database query in the `extension_get():` method in the `database_manager.py` to filter the SQL query based on the argument parameter and return it as JSON data where:

1. If no valid parameter is passed, the function will return the entire database in a JSON format because of the `%` wildcard.
2. If a valid parameter is passed, the database will be queried with a `WHERE language LIKE' SQL query, and all matching languages (if any) will be returned in JSON format.

```python
def extension_get(lang):
    with sql.connect("database/data_source.db") as con:
        cur = con.cursor()
        cur.execute("SELECT * FROM extension WHERE language LIKE ?;", [lang])
        migrate_data = [
            dict(
                extID=row[0],
                name=row[1],
                hyperlink=row[2],
                about=row[3],
                image=row[4],
                language=row[5],
            )
            for row in cur.fetchall()
        ]
    return jsonify(migrate_data)
```

### Step 7: Test your GET Response

![Screen recording testing a API GET with Thunder Client](/docs/README_resources/test_GET_API.gif "Follow these steps to test your GET API")

#### ✅ Checkpoint: GET Request with Query Parameters Working

```bash
# Verify your filtered GET endpoint works:
# 1. Test in Thunder Client:
#    GET http://localhost:3000?lang=python
#    Should return only Python extensions
# 2. Test with different languages:
#    GET http://localhost:3000?lang=bash
#    GET http://localhost:3000?lang=html
# 3. Test without parameter:
#    GET http://localhost:3000
#    Should return ALL extensions
# 4. Verify case insensitivity (lowercase 'python' returns results)
```

> **Filtering not working?** Check that your language values in the database match the ENUM values (uppercase: PYTHON, CPP, BASH, etc.)
>
> **Having issues?** See [🔧 Troubleshooting - Query Parameter Issues](#-troubleshooting-common-issues)

---

### Step 8: Setup your basic POST response

Extend the `/add_extension` route in api.py to pass the POST data to the 'dbHandler' and set up a driver to return the response with a 201 status code.

```python
def post():
    data = request.get_json()
    response = dbHandler.extension_add(data)
    return response
```

Extend the `extension_add():` method in the `database_manager.py` to be a driver that returns the received data to the POST request.

```python
def extension_add(response):
    data = response
    return data, 200
```

### Step 9: Test your basic POST response

![Screen recording testing a API basic POST with Thunder Client](/docs/README_resources/test_basic_POST_API.gif "Follow these steps to test your basic POST API")

#### ✅ Checkpoint: Basic POST Response Working

```bash
# Verify your POST endpoint echoes data back:
# 1. Test in Thunder Client:
#    POST to http://localhost:3000/add_extension
#    Body (JSON): {"name": "test", "hyperlink": "https://test.com", "about": "test", "image": "https://test.jpg", "language": "PYTHON"}
#    Should return the same JSON with status 200
# 2. Verify the response matches what you sent
# 3. Note: Data is NOT saved to database yet (coming in next steps)
```

> **POST not working?** Ensure you set Content-Type to "application/json" in Thunder Client headers.
>
> **Having issues?** See [🔧 Troubleshooting - POST Request Issues](#-troubleshooting-common-issues)

---

### Step 10: Extend the dbHandler to validate the JSON

**Understanding JSON Schema Validation:**

In your previous PWA, you trusted user input and inserted it directly into the database. **This is dangerous!** Users could:

- Send malicious code (XSS attacks)
- Send wrong data types that crash your app
- Send extra fields you don't expect

**JSON Schema Solution:**
Define strict rules for what valid JSON looks like, then reject anything that doesn't match.

Update the `extension_add():` method in `database_manager.py` to validate the JSON and return a message and response code. The schema provided validates the JSON with the following rules:

1. All 5 properties are required.
2. No extra properties are allowed.
3. The data type for all 5 properties is string.
4. The hyperlink pattern enforces the URL to start with `https://marketplace.visualstudio.com/items?itemName=`, and the characters `<` and `>` are not allowed to prevent XXS attacks.
5. The image pattern requires https:// but `<` and `>` are not allowed to prevent XXS attacks.
6. Languages must be enumerated with the list of languages.

**Breaking Down the JSON Schema:**

```python
schema = {
    "type": "object",                    # Must be a JSON object {...}
    "required": ["name", "hyperlink", ...], # These 5 fields MUST exist
    "additionalProperties": False,       # Extra fields = REJECTED
    "properties": {
        "name": {"type": "string"},      # Must be text
        "hyperlink": {
            "type": "string",
            "pattern": r"^https:\/\/marketplace\.visualstudio\.com..."
        },
    }
}
```

**Understanding the Hyperlink Pattern:**

Let's break down the regex pattern piece by piece:

```
^https:\/\/marketplace\.visualstudio\.com\/items\?itemName=
└─ MUST start with this exact URL

(?!.*[<>])
└─ FORBIDS < and > characters (prevents XSS attacks)

[a-zA-Z0-9\-._~:\/?#\[\]@!$&'()*+,;=]*$
└─ ALLOWS only safe URL characters
```

**Why Block < and >?**
These characters can inject HTML/JavaScript:

```html
<script>
  alert("hacked!");
</script>
```

**Testing Your Patterns:**
Use https://regex101.com/ to test and understand regex patterns.

> [!Important]
> You can use [https://regex101.com/](https://regex101.com/) to design and test patterns for your database design. Regular expressions in Python require a raw string (with the r prefix) due to the way characters need to be escaped.

```python
    if validate_json(data):
        return {"message": "Extension added successfully"}, 201
    else:
        return {"error": "Invalid JSON"}, 400


schema = {
    "type": "object",
    "validationLevel": "strict",
    "required": [
        "name",
        "hyperlink",
        "about",
        "image",
        "language",
    ],
    "properties": {
        "name": {"type": "string"},
        "hyperlink": {
                "type": "string",
                "pattern": r"^https:\/\/marketplace\.visualstudio\.com\/items\?itemName=(?!.*[<>])[a-zA-Z0-9\-._~:\/?#\[\]@!$&'()*+,;=]*$",
        },
        "about": {"type": "string"},
        "image": {
            "type": "string",
            "pattern": r"^https:\/\/(?!.*[<>])[a-zA-Z0-9\-._~:\/?#\[\]@!$&'()*+,;=]*$",
        },
        "language": {
            "type": "string",
            "enum": ["PYTHON", "CPP", "BASH", "SQL", "HTML", "CSS", "JAVASCRIPT"],
        },
    },
    "additionalProperties": False,
}

def validate_json(json_data):
    try:
        validate(instance=json_data, schema=schema)
        return True
    except:
        return False
```

Sample JSON data for you to test the API:

```text
{"name": "test", "hyperlink": "https://marketplace.visualstudio.com/items?itemName=123.html", "about": "This is a test", "image": "https://test.jpg", "language": "BASH"}
```

### Step 10: Test your validation POST response

![Screen recording testing a API basic POST with Thunder Client](/docs/README_resources/test_POST_API_Auth.gif "Follow these steps to test your basic POST API")

#### ✅ Checkpoint: JSON Validation Working

```bash
# Verify your JSON validation works:
# 1. Test VALID JSON in Thunder Client:
#    POST to http://localhost:3000/add_extension
#    Body: {"name": "test", "hyperlink": "https://marketplace.visualstudio.com/items?itemName=123.html", "about": "This is a test", "image": "https://test.jpg", "language": "BASH"}
#    Should return: {"message": "Extension added successfully"} with status 201
#
# 2. Test INVALID JSON (missing field):
#    Body: {"name": "test", "hyperlink": "https://test.com"}
#    Should return: {"error": "Invalid JSON"} with status 400
#
# 3. Test INVALID URL pattern:
#    Body with hyperlink: "https://wrong-domain.com"
#    Should return: {"error": "Invalid JSON"} with status 400
#
# 4. Test XSS attack prevention:
#    Body with image: "https://test.com/<script>alert('xss')</script>"
#    Should return: {"error": "Invalid JSON"} with status 400
```

> **Validation always passing?** Check that jsonschema is installed: `pip install jsonschema`
>
> **Having issues?** See [🔧 Troubleshooting - JSON Validation Issues](#-troubleshooting-common-issues)

---

### Step 11: Insert the POST data into the database

Update the `extension_add():` method in database_manager.py to INSERT the JSON data into the database. The `extID` is not required as it has been configured to auto increment in the database table.

```python
def extension_add(data):
    if validate_json(data):
        with sql.connect("database/data_source.db") as con:
            cur = con.cursor()
            cur.execute(
                "INSERT INTO extension (name, hyperlink, about, image, language) VALUES (?, ?, ?, ?, ?);",
                [
                    data["name"],
                    data["hyperlink"],
                    data["about"],
                    data["image"],
                    data["language"],
                ],
            )
            con.commit()
        return jsonify({"message": "Extension added successfully"}), 201
    else:
        return jsonify({"error": "Invalid JSON"}), 400
        return {"error": "Invalid JSON"}, 400
```

#### ✅ Checkpoint: Database Insertion Working

```bash
# Verify data is being saved to database:
# 1. Test POST with valid JSON in Thunder Client
# 2. Check response: {"message": "Extension added successfully"} with status 201
# 3. Verify data in database:
#    - Open database/data_source.db in SQLite3 Editor
#    - Run query: SELECT * FROM extension ORDER BY extID DESC LIMIT 1;
#    - Should see your newly added extension
# 4. Test GET endpoint to confirm new data appears:
#    GET http://localhost:3000
```

> **Data not saving?** Check database file permissions and that the connection path is correct: `database/data_source.db`
>
> **Having issues?** See [🔧 Troubleshooting - Database Insert Issues](#-troubleshooting-common-issues)

---

### Step 12: Implement POST Authorisation

**Understanding API Authentication:**

Currently, ANYONE can POST data to your API and add extensions to your database!

**Authentication Types:**

| Type                       | Description                 | Use Case                 |
| -------------------------- | --------------------------- | ------------------------ |
| **API Key** (We use this)  | Shared secret string        | App-to-app communication |
| **User Auth** (OAuth, JWT) | Individual user credentials | User-specific data       |
| **No Auth**                | Public access               | Read-only public data    |

**Why API Keys for This Project?**

- We're not authenticating individual users
- We're authenticating the PWA application itself
- Only authorized applications can add extensions

**How It Works:**

1. **Server side (api.py):**

```python
auth_key = "4L50v92nOgcDCYUM"  # Secret key (like a password)

if request.headers.get("Authorisation") == auth_key:
    # Key matches = allow request
else:
    return {"error": "Unauthorised"}, 401  # Wrong key = reject
```

2. **Client side (main.py) - coming later:**

```python
app_header = {"Authorisation": "4L50v92nOgcDCYUM"}  # Include key in request

response = requests.post(
    "http://127.0.0.1:3000/add_extension",
    json=data,
    headers=app_header  # Send key with request
)
```

**Security Considerations:**

- ⚠️ **NEVER commit API keys to GitHub** (use environment variables in production)
- ⚠️ **NEVER expose API keys in client-side JavaScript** (they're in main.py server-side only)
- ⚠️ **Use HTTPS in production** (or keys can be intercepted)
- ✅ **Generate unique keys** using https://acte.ltd/utils/randomkeygen

**Analogy:**
API key = Building access card

- Only people with the card can enter
- If card is stolen, change the lock code

[API Key Authorisation](https://cloud.google.com/endpoints/docs/openapi/when-why-api-key) is a common method for authorising an application, site, or project. In this scenario, the API is not authorising a specific user. This is a very simple implementation of API Key Authorisation.

Extend the `api.py` to store the key as a variable. Students will need to generate a unique basic 16 secret key with [https://acte.ltd/utils/randomkeygen](https://acte.ltd/utils/randomkeygen).

```python
auth_key = "4L50v92nOgcDCYUM"
```

Extend the `def post():` method in `api.py` to request the `authorisation` attribute from the post head, compare it to the `auth_key,` and process the appropriate response.

```python
def post():
    if request.headers.get("Authorisation") == auth_key:
        data = request.get_json()
        response = dbHandler.extension_add(data)
        return response
    else:
        return {"error": "Unauthorised"}, 401
```

### Step 13: Test your authorisation for a POST response

![Screen recording testing a API basic POST with Thunder Client](/docs/README_resources/test_POST_API_Auth.gif "Follow these steps to test your POST API Authorisation")

#### ✅ Checkpoint: API Authentication Working

```bash
# Verify API key authentication works:
# 1. Test POST WITHOUT auth header in Thunder Client:
#    POST to http://localhost:3000/add_extension
#    Body: (valid JSON)
#    Headers: (none)
#    Should return: {"error": "Unauthorised"} with status 401
#
# 2. Test POST WITH WRONG auth key:
#    Headers: Authorisation: wrongkey123
#    Should return: {"error": "Unauthorised"} with status 401
#
# 3. Test POST WITH CORRECT auth key:
#    Headers: Authorisation: 4L50v92nOgcDCYUM (or your generated key)
#    Should return: {"message": "Extension added successfully"} with status 201
#
# 4. Verify only authenticated requests can add data
```

> **Authentication not blocking requests?** Check the header name is exactly "Authorisation" (not "Authorization") to match the code.
>
> **Having issues?** See [🔧 Troubleshooting - Authentication Issues](#-troubleshooting-common-issues)

---

### Step 14: Configure the logger to log to api_security_log.log

Extend the `api.py` with the implementation below, which should be inserted directly below the `imports`. This will configure the logger to log to a file for security analysis.

```python
api_log = logging.getLogger(__name__)
logging.basicConfig(
    filename="api_security_log.log",
    encoding="utf-8",
    level=logging.DEBUG,
    format="%(asctime)s %(message)s",
)
```

#### ✅ Checkpoint: API Development Complete

```bash
# Verify your complete API is working:
# 1. Test GET all extensions: http://localhost:3000
# 2. Test GET filtered: http://localhost:3000?lang=python
# 3. Test POST with auth: Should add to database
# 4. Test POST without auth: Should return 401
# 5. Test POST with invalid JSON: Should return 400
# 6. Check api_security_log.log file was created
# 7. Restart API and verify everything still works
```

> **Ready to move on?** Your API is complete! Next, you'll build the PWA interface to interact with this API.

---

## Instructions for building the PWA user interface to the API.

> [!Note]
> This implementation uses the [Bootstrap](https://getbootstrap.com/) frontend CSS & JS design framework. Version 5.3.3 has been included in the static files.

### Step 1: Setup the Jinga2 template engine file structure

```text
├── templates
│   ├── partials
│   │   ├──footer.html
│   │   └──menu.html
│   ├──index.html
│   ├──layout.html
│   └──privacy.html
```

### Step 2: Setup the Jinga2 template

This Jinga2/HTML implementation in layout.html:

1. Security features are defined in the head.
2. The menu and footer are defined in a partial for easy maintenance.
3. The body will be defined by the block content when the `layout.html` is inherited.
4. Bootstrap components (CSS & JavaScript) are linked.
5. JS Components, including the PWA service worker, are linked.

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta
      http-equiv="Content-Security-Policy"
      content="base-uri 'self'; default-src 'self'; style-src 'self'; script-src 'self'; img-src 'self' *; media-src 'self'; font-src 'self'; connect-src 'self'; object-src 'self'; worker-src 'self'; frame-src 'none'; form-action 'self'; manifest-src 'self'"
    />
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <link rel="stylesheet" type="text/css" href="static/css/style.css" />
    <title>VS Code Extensions for Software Engineering</title>
    <link rel="manifest" href="static/manifest.json" />
    <link rel="icon" type="image/x-icon" href="static/images/favicon.png" />
    <meta name="theme-color" content="" />
    <link href="static/css/bootstrap.min.css" rel="stylesheet" />
  </head>
  <body>
    {% include "partials/menu.html" %}
    <main>{% block content %}{% endblock %}</main>
    {% include "partials/footer.html" %}
    <script src="static/js/bootstrap.bundle.min.js"></script>
    <script src="static/js/serviceWorker.js"></script>
    <script src="static/js/app.js"></script>
  </body>
</html>
```

### Step 3: Setup the footer.html

This HTML implementation provides a full-width horizontal rule and a Bootstrap column containing a link to the privacy page.

```html
<div class="container-fluid">
  <hr />
</div>
<div class="container">
  <div class="row">
    <div class="col-12">
      <a href="privacy.html">Privacy Policy</a>
    </div>
  </div>
</div>
```

### Step 4: Setup the menu.html and add some UX/accessibility advanced features using JS.

This HTML implementation is an adaption of the basic [Bootstrap Navbar](https://getbootstrap.com/docs/5.3/components/navbar/).

```html
<nav class="navbar navbar-expand-lg bg-body-tertiary">
  <div class="container-fluid">
    <a class="navbar-brand" href="/">
      <img src="static/images/logo.png" alt="logo" height="80" />
    </a>
    <button
      class="navbar-toggler"
      type="button"
      data-bs-toggle="collapse"
      data-bs-target="#navbarSupportedContent"
      aria-controls="navbarSupportedContent"
      aria-expanded="false"
      aria-label="Toggle navigation"
    >
      <span class="navbar-toggler-icon"></span>
    </button>
    <div class="collapse navbar-collapse" id="navbarSupportedContent">
      <ul class="navbar-nav me-auto mb-2 mb-lg-0">
        <li class="nav-item">
          <a class="nav-link active" href="/" aria-current="page">Home</a>
        </li>
        <li class="nav-item">
          <a class="nav-link" href="/add.html">Add Extension</a>
        </li>
        <li class="nav-item">
          <a class="nav-link" href="/privacy.html">Privacy</a>
        </li>
      </ul>
      <form class="d-flex" role="search" id="search-form">
        <input
          class="form-control me-2"
          type="search"
          placeholder="Search"
          aria-label="Search"
          id="search-input"
        />
        <button class="btn btn-outline-success" type="submit">Search</button>
      </form>
    </div>
  </div>
</nav>
```

Extend the `app.js` with this script that toggles the active class and the `aria-current="page"` attribute for the current page menu item. The `active` class improves UX by styling the current page in the menu differently and adding the [`aria-current` attribute](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/Attributes/aria-current) to the current page which improves the context understanding of screen readers for enhanced accessibility.

```js
document.addEventListener("DOMContentLoaded", function () {
  const navLinks = document.querySelectorAll(".nav-link");
  const currentUrl = window.location.pathname;

  navLinks.forEach((link) => {
    const linkUrl = link.getAttribute("href");
    if (linkUrl === currentUrl) {
      link.classList.add("active");
      link.setAttribute("aria-current", "page");
    } else {
      link.classList.remove("active");
      link.removeAttribute("aria-current");
    }
  });
});
```

Extend the `app.js` with this script that adds basic search functionality to the search button in the menu by searching the current page and highlighting matching words.

```js
document.addEventListener("DOMContentLoaded", function () {
  const form = document.getElementById("search-form");
  const input = document.getElementById("search-input");

  form.addEventListener("submit", function (event) {
    event.preventDefault();
    const searchTerm = input.value.trim().toLowerCase();
    if (searchTerm) {
      highlightText(searchTerm);
    }
  });

  function highlightText(searchTerm) {
    const mainContent = document.querySelector("main");
    removeHighlights(mainContent);
    highlightTextNodes(mainContent, searchTerm);
  }

  function removeHighlights(element) {
    const highlightedElements = element.querySelectorAll("span.highlight");
    highlightedElements.forEach((el) => {
      el.replaceWith(el.textContent);
    });
  }

  function highlightTextNodes(element, searchTerm) {
    const regex = new RegExp(`(${searchTerm})`, "gi");
    const walker = document.createTreeWalker(
      element,
      NodeFilter.SHOW_TEXT,
      null,
      false
    );
    let node;
    while ((node = walker.nextNode())) {
      const parent = node.parentNode;
      if (
        parent &&
        parent.nodeName !== "SCRIPT" &&
        parent.nodeName !== "STYLE"
      ) {
        const text = node.nodeValue;
        const highlightedText = text.replace(
          regex,
          '<span class="highlight">$1</span>'
        );
        if (highlightedText !== text) {
          const tempDiv = document.createElement("div");
          tempDiv.innerHTML = highlightedText;
          while (tempDiv.firstChild) {
            parent.insertBefore(tempDiv.firstChild, node);
          }
          parent.removeChild(node);
        }
      }
    }
  }
});
```

Extend style.css to add the class required by the search script.

```css
.highlight {
  background-color: yellow;
  border-radius: 20px;
  border: 1px yellow solid;
}
```

### Step 5: Inherit the layout to the /index.html and set up the app route.

Insert the basic HTML into index.html.

```html
{% extends 'layout.html' %} {% block content %}
<div class="container">
  <div class="row"></div>
  <div class="row"></div>
</div>
{% endblock %}
```

This Python Flask implementation in `main.py`

1. Imports all dependencies required for the whole project.
2. Set up CSRFProtect to provide asynchronous keys that protect the app from a CSRF attack. Students will need to generate a unique basic 16 secret key with [https://acte.ltd/utils/randomkeygen](https://acte.ltd/utils/randomkeygen).
3. Defines the head attribute for authorising a POST request to the API.
4. Define a secure Content Secure Policy (CSP) head.
5. Configures the Flask app.
6. Redirect /index.html to the domain root for a consistent user experience.
7. Renders the index.html for a GET app route.
8. Provide an endpoint to log CSP violations for security analysis.

```python
from flask import Flask
from flask import redirect
from flask import render_template
from flask import request
import requests
from flask_wtf import CSRFProtect
from flask_csp.csp import csp_header
import logging


# Generate a unique basic 16 key: https://acte.ltd/utils/randomkeygen
app = Flask(__name__)
csrf = CSRFProtect(app)
app.secret_key = b"6HlQfWhu03PttohW;apl"

app_header = {"Authorisation": "4L50v92nOgcDCYUM"}


@app.route("/index.html", methods=["GET"])
def root():
    return redirect("/", 302)


@app.route("/", methods=["GET"])
@csp_header(
        {
        "base-uri": "self",
        "default-src": "'self'",
        "style-src": "'self'",
        "script-src": "'self'",
        "img-src": "*",
        "media-src": "'self'",
        "font-src": "self",
        "object-src": "'self'",
        "child-src": "'self'",
        "connect-src": "'self'",
        "worker-src": "'self'",
        "report-uri": "/csp_report",
        "frame-ancestors": "'none'",
        "form-action": "'self'",
        "frame-src": "'none'",
        }
)
def index():
    return render_template("/index.html")


@app.route("/csp_report", methods=["POST"])
@csrf.exempt
def csp_report():
    app.logger.critical(request.data.decode())
    return "done"


if __name__ == "__main__":
    app.run(debug=True, host="0.0.0.0", port=5000)

```

### Step 7: Test the basic index.html

```bash
python main.py
```

![A render of the basic index.html](README_resources\basic_index.png "The basic index.html should load like this")

#### ✅ Checkpoint: Basic PWA Application Running

```bash
# Verify your PWA application works:
# 1. Start the PWA server: python main.py
# 2. Open browser to http://localhost:5000
# 3. Should see:
#    - Logo and menu at top
#    - "VS Code Extensions" heading
#    - Empty container (no data yet)
#    - Footer with privacy policy link
# 4. Check browser console (F12) for any errors
# 5. Verify menu links work (Home, Add Extension, Privacy)
```

> **PWA not loading?** Check that Flask, flask_wtf, flask_csp, and requests are installed: `pip list | grep -i flask`
>
> **Having issues?** See [🔧 Troubleshooting - Flask Application Issues](#-troubleshooting-common-issues)

---

### Step 8: Setup the Privacy Policy

```html
{% extends 'layout.html' %} {% block content %}
<div class="container">
  <div class="row">
    <h1 class="display-1">Privacy Policy</h1>
    <p>Policy here...</p>
  </div>
  <div class="row"></div>
</div>
{% endblock %}
```

Extend `main.py` to include an app route to privacy.html

```python
@app.route("/privacy.html", methods=["GET"])
def privacy():
    return render_template("/privacy.html")
```

### Step 9: Test privacy.html and search functionality.

![A render of the privacy.html](README_resources\privacy_search.png "The privacy index.html should load like this")

Ensure your page renders correctly with the test cases:

1. The page renders correctly
2. The privacy menu item is darker than the other menu items
3. A search for "priv" highlights the correct letters in the main body.

#### ✅ Checkpoint: Menu Navigation and Search Working

```bash
# Verify navigation and search features:
# 1. Test menu navigation:
#    - Click "Home" → Should go to /
#    - Click "Add Extension" → Should go to /add.html (will show template error - that's OK for now)
#    - Click "Privacy" → Should go to /privacy.html
# 2. Test active menu styling:
#    - On each page, verify the current page menu item is highlighted/darker
# 3. Test search functionality:
#    - Go to privacy page
#    - Type "priv" in search box and click Search
#    - Should see "priv" highlighted in yellow on the page
# 4. Check browser console for JavaScript errors
```

> **Search not highlighting?** Check that app.js is properly linked in layout.html and the highlight CSS class exists in style.css.
>
> **Having issues?** See [🔧 Troubleshooting - JavaScript Issues](#-troubleshooting-common-issues)

---

### Step 10: Setup the cards and request the data from the API

**From Direct Database Access to HTTP Requests:**

In your previous PWA, `main.py` directly called the database:

```python
# Previous approach:
def index():
   data = dbHandler.listExtension()  # Direct database query
   return render_template('/index.html', content=data)
```

In this API architecture, `main.py` requests data from the API via HTTP:

```python
# API approach:
def index():
    url = "http://127.0.0.1:3000"
    response = requests.get(url)  # HTTP GET request
    data = response.json()  # Parse JSON response
    return render_template("index.html", data=data)
```

**What Changed?**

| Previous Approach        | API Approach                 |
| ------------------------ | ---------------------------- |
| Direct function call     | HTTP request over network    |
| Instant response         | Network delay (milliseconds) |
| Python data structures   | JSON text format             |
| No authentication needed | API key required             |
| Tight coupling           | Loose coupling               |

**Why Use requests Library?**

- `requests.get()` = Send HTTP GET request (retrieve data)
- `requests.post()` = Send HTTP POST request (send data)
- `.json()` = Parse JSON text into Python dictionary
- `.raise_for_status()` = Raise exception if error status code

**Error Handling is Now Critical:**

```python
try:
    response = requests.get(url)
    response.raise_for_status()
    data = response.json()
except requests.exceptions.RequestException as e:
    # API might be down, network error, etc.
    data = {"error": "Failed to retrieve data from the API"}
```

**What Could Go Wrong?**

- API server not running
- Network connection lost
- API returns error status
- Invalid JSON response

Extend the `index():` method in `main.py` so it requests data from the API and handles the exception that the API did not respond with an error message.

```python
def index():
    url = "http://127.0.0.1:3000"
    try:
        response = requests.get(url)
        response.raise_for_status()  # Raise an exception for HTTP errors
        data = response.json()
    except requests.exceptions.RequestException as e:
        data = {"error": "Failed to retrieve data from the API"}
    return render_template("index.html", data=data)
```

Replace the test html in 'index.html` template that:

1. Implements a [Bootstrap jumbotron heading](https://getbootstrap.com/docs/5.3/examples/jumbotron/).
2. Implements a [Bootstrap button group](https://getbootstrap.com/docs/5.3/components/button-group/) that will later allow users to filter the extensions by language.
3. Implements the database items as [Bootstrap cards](https://getbootstrap.com/docs/5.3/components/card/) in a responsive[Bootstrap Column Layout](https://getbootstrap.com/docs/5.3/layout/columns/).
4. Provides API error feedback to the user that is styled by the [Bootstrap color utility](https://getbootstrap.com/docs/5.3/utilities/colors/).
5. Apply [Bootstrap sizing](https://getbootstrap.com/docs/5.3/utilities/sizing/) and [Bootstrap spacing](https://getbootstrap.com/docs/5.3/utilities/spacing/) utilities to layout the cards.

```html
{% extends 'layout.html' %} {% block content %}
<div class="container py-4"">
  <div class="p-4 bg-body-tertiary rounded-3">
    <div class="container-fluid py-2">
      <h1 class="display-4">VS Code Extensions for Software Engineering</h1>
      <p class="lead">
        This is a collection of Visual Studio Code extensions that are useful
        for software engineering.
      </p>
    </div>
  </div>
</div>
<div class="container">
  <div class="row">
    <div class="btn-group" role="group" aria-label="Filter by language">
      <button type="button" class="btn btn-primary" id="all">All</button>
      <button type="button" class="btn btn-primary" id="python">Python</button>
      <button type="button" class="btn btn-primary" id="c++">C++</button>
      <button type="button" class="btn btn-primary" id="bash">BASH</button>
      <button type="button" class="btn btn-primary" id="sql">SQL</button>
      <button type="button" class="btn btn-primary" id="html">HTML</button>
      <button type="button" class="btn btn-primary" id="css">CSS</button>
      <button type="button" class="btn btn-primary" id="js">JAVASCRIPT</button>
    </div>
  </div>
</div>
<div class="container pt-4">
  <div class="row">
    <div class="error"><h2 class="text-danger">{{ data.error }}</h2></div>
    {% if data.error is not defined %}
      {% for row in data %}
      <div class="col-sm-12 col-lg-4 mb-4">
        <div class="card h-100" style="width: 18rem">
          <img
            src="{{ row.image }}"
            class="card-img-top"
            alt="Product image for the {{ row.name }} VSCode extension."
          />
          <div class="card-body">
            <h5 class="card-title">{{ row.name }}</h5>
            <p class="card-text">{{ row.about }}</p>
            <a href="{{ row.hyperlink }}" class="btn btn-primary">Read More</a>
          </div>
        </div>
      </div>
      {% endfor %}
    {% endif %}
  </div>
</div>
{% endblock %}
```

### Step 11: Test the index.html and API integration

![Screen recording testing a the index and the API integration](/docs/README_resources/test_index_and_API.gif "Follow these steps to test your index.html and API")

#### ✅ Checkpoint: PWA Connected to API

```bash
# Verify PWA is retrieving data from API:
# 1. Ensure BOTH servers are running:
#    Terminal 1: python api.py (port 3000)
#    Terminal 2: python main.py (port 5000)
#
# 2. Open browser to http://localhost:5000
#    - Should see Bootstrap jumbotron header
#    - Should see filter buttons (All, Python, C++, etc.)
#    - Should see Bootstrap cards displaying extensions from database
#
# 3. Test API connection:
#    - Stop the API server (Terminal 1)
#    - Refresh browser → Should see error message "Failed to retrieve data from the API"
#    - Restart API server
#    - Refresh browser → Data should load again
#
# 4. Test filter buttons:
#    - Click "Python" button → URL should change to ?lang=python
#    - Should see only Python extensions
#    - Click "All" → Should see all extensions again
```

> **No data showing?** Check both servers are running and the API URL in main.py is correct: `http://127.0.0.1:3000`
>
> **Having issues?** See [🔧 Troubleshooting - API Integration Issues](#-troubleshooting-common-issues)

---

### Running Two Servers Simultaneously

**IMPORTANT: This project requires TWO servers running at the same time!**

**Terminal 1 - API Server:**

```bash
python api.py
# Running on http://127.0.0.1:3000
```

**Terminal 2 - PWA Server:**

```bash
python main.py
# Running on http://127.0.0.1:5000
```

**Why Two Servers?**

- **API (port 3000)**: Manages data & database
- **PWA (port 5000)**: Serves web pages to users
- PWA makes HTTP requests to API to get/send data

**Communication Flow:**

```
User Browser (localhost:5000)
    ↓ Views web page
PWA Server (main.py port 5000)
    ↓ HTTP GET request
API Server (api.py port 3000)
    ↓ Query database
Database (data_source.db)
    ↑ Return data
API Server
    ↑ JSON response
PWA Server
    ↑ Render HTML with data
User Browser
```

**Testing Checklist:**

1. ✅ Start API server (Terminal 1): `python api.py`
2. ✅ Test API directly: Visit `http://localhost:3000` → Should see JSON
3. ✅ Start PWA server (Terminal 2): `python main.py`
4. ✅ Test PWA: Visit `http://localhost:5000` → Should see web page with data

**Troubleshooting:**

- If PWA shows "Failed to retrieve data from API" → API server not running
- If you see "Address already in use" → Server already running, kill it first

---

### Step 12: Implement a form with attribute controls to POST a new extension to API.

**Understanding Request Data: Form Data vs JSON:**

**Form Data (HTML forms):**

```python
# Client-side (HTML form):
<form method="POST">
    <input name="email" value="user@example.com">
</form>

# Server-side (main.py):
email = request.form["email"]  # Gets "user@example.com" from form
```

**Use for:**

- Submitting new data from HTML forms
- Sensitive data (passwords)
- POST requests
- Large amounts of data

**JSON Data (API communication):**

```python
# Client-side (PWA):
data = {"name": "test", "language": "PYTHON"}
requests.post(url, json=data)

# Server-side (API):
data = request.get_json()  # Gets entire JSON object
name = data["name"]  # Access by key
```

**Use for:**

- API communication
- Complex structured data
- App-to-app communication

The HTML Implementation in `add.html`

1. Provides error and message feedback to the user that is styled by the [Bootstrap color utility](https://getbootstrap.com/docs/5.3/utilities/colors/).
2. Uses [Bootstrap Forms](https://getbootstrap.com/docs/5.3/forms/form-control/) to layout a data entry form.
3. Uses Form attributes type, place holder & pattern to improve user experience in entering the correct data.

```html
{% extends 'layout.html' %} {% block content %}
<div class="container">
  <div class="row">
    <h1>Add an Extension</h1>
    <div class="error">
      <h2>
        <span class="text-danger">{{ data.error }}</span
        ><span class="text-success">{{ data.message }}</span>
      </h2>
    </div>
  </div>
</div>
<div class="container">
  <div class="row">
    <form action="/add.html" method="POST" class="box">
      <div class="col-auto">
        <label for="name" class="form-label">Extension name</label>
        <textarea
          id="name"
          name="name"
          class="form-control"
          rows="1"
          autocomplete="off"
        ></textarea>
      </div>
      <div class="col-auto">
        <label for="hyperlink" name="hyperlink" class="form-label"
          >Hyperlink to extension</label
        >
        <input
          id="hyperlink"
          name="hyperlink"
          type="url"
          class="form-control"
          placeholder="https://marketplace.visualstudio.com/items?itemName="
          pattern="^https:\/\/marketplace\.visualstudio\.com\/items\?itemName=(?!.*[<>])[a-zA-Z0-9\-._~:\/?#\[\]@!$&'()*+,;=]*$"
        />
      </div>
      <div class="col-auto">
        <label for="about" class="form-label">About</label>
        <textarea
          id="about"
          name="about"
          class="form-control"
          rows="3"
          placeholder="A brief description of the extension"
        ></textarea>
      </div>
      <div class="col-auto">
        <label for="name" name="image" class="form-label">URL to Icon</label>
        <input
          id="image"
          name="image"
          type="url"
          class="form-control"
          pattern="^https:\/\/(?!.*[<>])[a-zA-Z0-9\-._~:\/?#\[\]@!$&'()*+,;=]*$"
          placeholder="https://"
        />
      </div>
      <div class="col-auto">
        <label for="language" name="language" class="form-label"
          >Programming language</label
        >
        <select
          id="language"
          name="language"
          class="form-select"
          aria-label="Default select language"
        >
          <option selected>Select a language from this menu</option>
          <option value="PYTHON">PYTHON</option>
          <option value="CPP">CPP</option>
          <option value="BASH">BASH</option>
          <option value="SQL">SQL</option>
          <option value="HTML">HTML</option>
          <option value="CSS">CSS</option>
          <option value="JAVASCRIPT">JAVASCRIPT</option>
        </select>
      </div>
      <br />
      <div class="col-auto">
        <button type="submit" class="btn btn-primary mb-3">Submit</button>
      </div>
      <input type="hidden" name="csrf_token" value="{{ csrf_token() }}" />
    </form>
  </div>
</div>
{% endblock %}
```

Extend `main.py' to provide a route with POST and GET methods for `add.html` that

1. Renders `add.html` on GET requests.
2. On a POST method, read the form in add.html and construct a JSON.
3. Then, POST the JSON with the header that includes the Authentication key to the API.
4. Render `add.html` with any errors or messages from the API.

```python
@app.route("/add.html", methods=["POST", "GET"])
def form():
    if request.method == "POST":
        name = request.form["name"]
        hyperlink = request.form["hyperlink"]
        about = request.form["about"]
        image = request.form["image"]
        language = request.form["language"]
        data = {
            "name": name,
            "hyperlink": hyperlink,
            "about": about,
            "image": image,
            "language": language,
        }
        app.logger.critical(data)
        try:
            response = requests.post(
                "http://127.0.0.1:3000/add_extension",
                json=data,
                headers=app_header,
            )
            data = response.json()
        except requests.exceptions.RequestException as e:
            data = {"error": "Failed to retrieve data from the API"}
        return render_template("/add.html", data=data)
    else:
        return render_template("/add.html", data={})
```

### Step 13: Add event listeners to the `/` page for the buttons to filter the extensions by language.

Extend `app.js` with a script to provide functionality to the home page buttons.

```js
document.addEventListener("DOMContentLoaded", function () {
  if (window.location.pathname === "/") {
    const buttons = [
      { id: "all", url: "/" },
      { id: "python", url: "?lang=python" },
      { id: "cpp", url: "?lang=cpp" },
      { id: "bash", url: "?lang=bash" },
      { id: "sql", url: "?lang=sql" },
      { id: "html", url: "?lang=html" },
      { id: "css", url: "?lang=css" },
      { id: "js", url: "?lang=javascript" },
    ];

    buttons.forEach((button) => {
      const element = document.getElementById(button.id);
      if (element) {
        element.addEventListener("click", function () {
          window.location.href = button.url;
        });
      }
    });
  }
});
```

### Step 14: Forward the GET request argument to the API to filter extensions by language.

```python
    url = "http://127.0.0.1:3000"
    if request.args.get("lang") and request.args.get("lang").isalpha():
        lang = request.args.get("lang")
        url += f"?lang={lang}"
```

#### ✅ Checkpoint: Complete PWA Application Working

```bash
# Verify ALL features work together:
# 1. Ensure BOTH servers are running (api.py and main.py)
#
# 2. Test complete workflow:
#    a) Go to http://localhost:5000
#    b) Click filter buttons → Data should filter by language
#    c) Click "Add Extension" → Form should load
#    d) Submit new extension → Success message should appear
#    e) Return to home → New extension should appear in cards
#
# 3. Test search functionality:
#    - Search for extension names on home page
#    - Should highlight matching text
#
# 4. Test navigation:
#    - All menu links work
#    - Active menu item is highlighted on each page
#
# 5. Test error handling:
#    - Stop API server
#    - Refresh home page → Should show error message
#    - Try to add extension → Should show error message
#    - Restart API → Everything works again
#
# 6. Verify security logs:
#    - main_security_log.log exists and contains entries
#    - api_security_log.log exists and contains entries
```

> **Everything working?** Great! Your PWA is complete and communicating with the API. Next: Step 15 for final logging setup.

---

### Step 15: Configure the logger to log to main_security_log.log

Extend the `main.py` with the implementation below, which should be inserted directly below the `imports`. This will configure the logger to log to a file for security analysis.

```python
app_log = logging.getLogger(__name__)
logging.basicConfig(
    filename="main_security_log.log",
    encoding="utf-8",
    level=logging.DEBUG,
    format="%(asctime)s %(message)s",
```

````

### 15: Configure the logger to log to main_security_log.log

Extend the `main.py` with the implementation below, which should be inserted directly below the `imports`. This will configure the logger to log to a file for security analysis.

```python
app_log = logging.getLogger(__name__)
logging.basicConfig(
    filename="main_security_log.log",
    encoding="utf-8",
    level=logging.DEBUG,
    format="%(asctime)s %(message)s",
)
````

#### ✅ Checkpoint: Project Complete!

```bash
# Final verification checklist:
#
# API (port 3000):
# ✅ GET all extensions works
# ✅ GET filtered by language works
# ✅ POST with authentication works
# ✅ POST without authentication blocked (401)
# ✅ POST with invalid JSON rejected (400)
# ✅ Rate limiting configured
# ✅ CORS enabled
# ✅ api_security_log.log created
#
# PWA (port 5000):
# ✅ Home page displays extension cards
# ✅ Filter buttons work
# ✅ Add extension form works
# ✅ Form validation works
# ✅ Search functionality works
# ✅ Menu navigation works
# ✅ Active menu highlighting works
# ✅ Privacy page renders
# ✅ Error handling displays messages
# ✅ main_security_log.log created
# ✅ CSRF protection enabled
# ✅ CSP headers configured
#
# Integration:
# ✅ PWA successfully requests data from API
# ✅ PWA successfully posts data to API
# ✅ Authentication headers working
# ✅ Both servers run simultaneously
# ✅ Error handling when API is down
```

**🎉 Congratulations!** You've built a complete RESTful API with authentication and a PWA that consumes it!

---

## Extension activities to improve the API and PWA.

1. Create a get_languages method that returns all the languages in the database.
2. Improve exception handling of the `add_extension` endpoint to give more detailed feedback to the user.
3. Use the new get-languages method to define the content that renders in the PWA.
4. Implement a sort extension by function

---

## 🔧 Troubleshooting Common Issues

### API Server Issues

#### **Problem: Flask modules not found**

**Error**: `ModuleNotFoundError: No module named 'flask_cors'` or similar
**Solution**:

```bash
# Install all required packages:
pip install Flask flask_cors flask_limiter
# or try:
pip3 install Flask flask_cors flask_limiter
```

#### **Problem: API server won't start**

**Error**: Various import or syntax errors
**Solution**:

1. Check you're in the correct directory: `pwd`
2. Verify api.py exists: `ls -la`
3. Check Python syntax in api.py
4. Ensure all imports are correct
5. Verify database_manager.py exists in same directory

#### **Problem: "Address already in use" on port 3000**

**Error**: `OSError: [Errno 48] Address already in use`
**Solution**:

```bash
# Kill processes using port 3000:
lsof -ti:3000 | xargs kill -9
# Or use a different port in api.py:
api.run(debug=True, host='0.0.0.0', port=3001)
```

#### **Problem: CORS errors in browser console**

**Error**: `Access to XMLHttpRequest has been blocked by CORS policy`
**Solution**:

1. Verify CORS is imported: `from flask_cors import CORS`
2. Check CORS is applied: `cors = CORS(api)`
3. Ensure API server is running on port 3000
4. Try restarting both servers

### Database Connection Issues

#### **Problem: Database file not found**

**Error**: `sqlite3.OperationalError: unable to open database file`
**Solution**:

1. Check database file exists: `ls database/data_source.db`
2. Verify path in database_manager.py: `database/data_source.db`
3. Check file permissions: `ls -la database/`
4. Ensure you're in project root directory

#### **Problem: No such table: extension**

**Error**: `sqlite3.OperationalError: no such table: extension`
**Solution**:

1. Open database in SQLite3 Editor
2. Run CREATE TABLE query from instructions
3. Verify table was created: `SELECT * FROM extension;`
4. Check table name matches exactly (case-sensitive)

#### **Problem: SQL syntax errors**

**Error**: `sqlite3.OperationalError: near "...": syntax error`
**Solution**:

1. Check for typos in SQL commands
2. Ensure proper quotes around text values
3. Verify column names match exactly
4. Check for missing commas or parentheses
5. Use SQLite3 Editor query validator

### Query Parameter Issues

#### **Problem: Filtering not working**

**Error**: All extensions returned regardless of ?lang= parameter
**Solution**:

1. Check language values in database are uppercase: PYTHON, BASH, etc.
2. Verify SQL query uses LIKE with parameter: `WHERE language LIKE ?`
3. Test with uppercase in URL: `?lang=PYTHON`
4. Check `.upper()` is applied to lang variable in api.py

#### **Problem: Query parameter returns empty array**

**Solution**:

1. Verify language exists in database
2. Check spelling matches exactly
3. Test without parameter - should return all data
4. Check SQL wildcard `%` is used for "all"

### POST Request Issues

#### **Problem: POST returns 415 Unsupported Media Type**

**Error**: 415 error in Thunder Client
**Solution**:

1. Set Content-Type header to `application/json`
2. Ensure body is valid JSON format
3. Use "JSON" body type in Thunder Client, not "Text"

#### **Problem: POST returns 400 Bad Request**

**Error**: `{"error": "Invalid JSON"}`
**Solution**:

1. Validate JSON structure at https://jsonlint.com/
2. Check all required fields are present: name, hyperlink, about, image, language
3. Verify URLs match regex patterns
4. Check language is in ENUM list
5. Remove any extra fields not in schema

#### **Problem: request.get_json() returns None**

**Solution**:

1. Verify Content-Type header is set to `application/json`
2. Check body contains valid JSON
3. Ensure using POST method, not GET
4. Try `request.get_json(force=True)` temporarily for debugging

### JSON Validation Issues

#### **Problem: Validation always fails**

**Error**: All valid JSON rejected with 400
**Solution**:

1. Check jsonschema is installed: `pip install jsonschema`
2. Verify schema definition matches in database_manager.py
3. Test with exact sample JSON from instructions
4. Check regex patterns are raw strings (r"pattern")
5. Print data before validation to see what's being received

#### **Problem: Regex pattern not working**

**Solution**:

1. Test pattern at https://regex101.com/
2. Ensure raw string prefix: `r"^https:\/\/..."`
3. Check for typos in pattern
4. Verify escape characters: `\/` for `/`

#### **Problem: XSS validation blocking valid URLs**

**Solution**:

1. Check URL doesn't contain `<` or `>` characters
2. Ensure `(?!.*[<>])` pattern is in regex
3. Test with simple URL first: `https://test.com`
4. Add complexity gradually

### Database Insert Issues

#### **Problem: Data not saving to database**

**Error**: No error, but data doesn't appear
**Solution**:

1. Check `con.commit()` is called after INSERT
2. Verify `con.close()` is called
3. Check database file permissions
4. Open database in SQLite3 Editor and run SELECT query
5. Ensure validation is passing (returns True)

#### **Problem: Duplicate entry error**

**Error**: `sqlite3.IntegrityError: UNIQUE constraint failed`
**Solution**:

```python
# Add error handling in database_manager.py:
try:
    cur.execute("INSERT INTO extension...")
    con.commit()
except sqlite3.IntegrityError:
    return jsonify({"error": "Extension already exists"}), 400
```

### Authentication Issues

#### **Problem: Authentication always fails (401)**

**Error**: `{"error": "Unauthorised"}` even with correct key
**Solution**:

1. Check header name is exactly `Authorisation` (British spelling)
2. Verify key matches in both api.py and request
3. Check for extra spaces in key value
4. Try copying key directly, don't type it
5. Check header is being sent in request

#### **Problem: Authentication always passes**

**Solution**:

1. Verify if statement is checking header: `request.headers.get("Authorisation")`
2. Check `auth_key` variable is defined
3. Ensure comparison is `==` not `=`
4. Test with wrong key to confirm it blocks

#### **Problem: Header spelling confusion**

**Note**: The code uses British spelling "Authorisation" - this is intentional and must match exactly in:

- api.py: `request.headers.get("Authorisation")`
- main.py: `app_header = {"Authorisation": "..."}`
- Thunder Client: Header name must be `Authorisation`

### Flask Application Issues

#### **Problem: PWA server won't start**

**Error**: Various import or syntax errors
**Solution**:

1. Check you're in the correct directory: `pwd`
2. Verify main.py exists: `ls -la`
3. Check all required packages are installed:

```bash
pip install Flask flask_wtf flask_csp requests
```

4. Verify imports at top of main.py

#### **Problem: "Address already in use" on port 5000**

**Solution**:

```bash
# Kill processes using port 5000:
lsof -ti:5000 | xargs kill -9
# Or use a different port in main.py:
app.run(debug=True, host='0.0.0.0', port=5001)
```

#### **Problem: Template not found**

**Error**: `jinja2.exceptions.TemplateNotFound: index.html`
**Solution**:

1. Check templates folder exists: `ls templates/`
2. Verify file names match exactly (case-sensitive)
3. Check file is in correct location: `templates/index.html`
4. Ensure render_template path starts with `/`: `render_template("/index.html")`

#### **Problem: CSRF validation failed**

**Error**: 400 Bad Request when submitting forms
**Solution**:

1. Check CSRF token in form: `<input type="hidden" name="csrf_token" value="{{ csrf_token() }}" />`
2. Verify CSRFProtect is initialized: `csrf = CSRFProtect(app)`
3. Ensure app.secret_key is set
4. Check form method is POST

### API Integration Issues

#### **Problem: "Failed to retrieve data from the API"**

**Solution**:

1. Verify API server is running: `curl http://localhost:3000`
2. Check API is on port 3000, PWA on port 5000
3. Test API endpoint directly in browser
4. Check for firewall blocking
5. Verify URL in main.py: `http://127.0.0.1:3000`

#### **Problem: Both servers running but no communication**

**Solution**:

1. Test each server independently first
2. Check CORS is enabled on API
3. Verify requests library is installed: `pip install requests`
4. Check browser console for CORS errors
5. Ensure correct ports in code

#### **Problem: API returns data but PWA shows error**

**Solution**:

1. Check response format is JSON
2. Verify `response.json()` is called in main.py
3. Check for exceptions in try/except block
4. Print response status code: `print(response.status_code)`
5. Verify data structure matches template expectations

### JavaScript Issues

#### **Problem: Search not highlighting text**

**Solution**:

1. Check app.js is linked in layout.html
2. Verify highlight CSS class exists in style.css
3. Open browser console for JavaScript errors
4. Check search form has correct ID: `id="search-form"`
5. Verify input has ID: `id="search-input"`

#### **Problem: Filter buttons not working**

**Solution**:

1. Check button IDs match JavaScript: `id="python"`, etc.
2. Verify script checks pathname: `if (window.location.pathname === "/")`
3. Open browser console for errors
4. Check event listeners are attached
5. Verify buttons exist in HTML

#### **Problem: Menu highlighting not working**

**Solution**:

1. Check `aria-current="page"` attribute toggle logic
2. Verify CSS for `.active` class exists
3. Check nav-link class on menu items
4. Confirm currentUrl matches href values exactly
5. Check browser console for errors

### Testing and Validation Issues

#### **Problem: Thunder Client not working in Codespace**

**Note**: Thunder Client doesn't work in browser-based Codespaces
**Solution**:

Use curl instead:

```bash
# GET request:
curl http://localhost:3000

# POST request:
curl -X POST http://localhost:3000/add_extension \
  -H "Content-Type: application/json" \
  -H "Authorisation: 4L50v92nOgcDCYUM" \
  -d '{"name":"test","hyperlink":"https://marketplace.visualstudio.com/items?itemName=test","about":"test","image":"https://test.jpg","language":"PYTHON"}'
```

#### **Problem: Can't see both servers running**

**Solution**:

1. Open two terminal windows/tabs
2. Run api.py in Terminal 1
3. Run main.py in Terminal 2
4. Both should show "Running on..." messages
5. Keep both terminals open while testing

### Quick Diagnostic Commands

When encountering issues, run these commands to gather information:

```bash
# Check Python version
python --version
python3 --version

# Check Flask installation
pip show flask
pip list | grep -i flask

# Check current directory and files
pwd
ls -la

# Test if API responds
curl -I http://localhost:3000

# Test if PWA responds
curl -I http://localhost:5000

# Check database
sqlite3 database/data_source.db ".tables"
sqlite3 database/data_source.db "SELECT COUNT(*) FROM extension;"

# Check for running processes on ports
lsof -i:3000
lsof -i:5000

# View recent log entries
tail -n 20 api_security_log.log
tail -n 20 main_security_log.log
```

### Getting Help

If you're still stuck after trying these solutions:

1. **Read the error message carefully** - it often tells you exactly what's wrong
2. **Use browser DevTools** (F12) to check Console and Network tabs
3. **Test each component separately** - API, then PWA, then together
4. **Check the checkpoints** - verify each step completed successfully
5. **Review previous tutorial** - ensure foundational knowledge is solid
6. **Ask a classmate or teacher** - explain what you've tried already
7. **Search for the specific error message** online

### Common Error Message Patterns

| Error Pattern              | Likely Cause           | Solution Section             |
| -------------------------- | ---------------------- | ---------------------------- |
| `ModuleNotFoundError`      | Missing Python package | API Server Issues            |
| `Address already in use`   | Port conflict          | API/Flask Application Issues |
| `sqlite3.OperationalError` | Database problem       | Database Connection Issues   |
| `CORS policy`              | Cross-origin blocked   | API Server Issues            |
| `401 Unauthorised`         | Auth problem           | Authentication Issues        |
| `400 Bad Request`          | Invalid data           | JSON Validation Issues       |
| `TemplateNotFound`         | Missing HTML file      | Flask Application Issues     |
| `Failed to retrieve data`  | API communication      | API Integration Issues       |

---

<p xmlns:cc="http://creativecommons.org/ns#" xmlns:dct="http://purl.org/dc/terms/"><a property="dct:title" rel="cc:attributionURL" href="https://github.com/TempeHS/Flask_PWA_API_Extension_Task_Source">Flask PWA API Extension Task Source</a> and <a property="dct:title" rel="cc:attributionURL" href="https://github.com/TempeHS/Flask_PWA_API_Extension_Task_Template">Flask PWA API Extension Task Template</a> by <a rel="cc:attributionURL dct:creator" property="cc:attributionName" href="https://github.com/benpaddlejones">Ben Jones</a> is licensed under <a href="https://creativecommons.org/licenses/by-nc-sa/4.0/?ref=chooser-v1" target="_blank" rel="license noopener noreferrer" style="display:inline-block; ">Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International<img style="height:22px!important; margin-left:3px; vertical-align:text-bottom; " src="https://mirrors.creativecommons.org/presskit/icons/cc.svg?ref=chooser-v1" alt=""><img style="height:22px!important; margin-left:3px; vertical-align:text-bottom; " src="https://mirrors.creativecommons.org/presskit/icons/by.svg?ref=chooser-v1" alt=""><img style="height:22px!important; margin-left:3px; vertical-align:text-bottom; " src="https://mirrors.creativecommons.org/presskit/icons/nc.svg?ref=chooser-v1" alt=""><img style="height:22px!important; margin-left:3px; vertical-align:text-bottom; " src="https://mirrors.creativecommons.org/presskit/icons/sa.svg?ref=chooser-v1" alt=""></a></p>
