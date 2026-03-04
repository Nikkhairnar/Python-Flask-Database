# Part 2: Full CRUD Operations with HTML Forms

## One-Line Summary
Full CRUD operations (Create, Read, Update, Delete) with HTML forms

## Prerequisites
- Complete part-1 first
- Understand basic database connection

## How to Run
```bash
cd part-2
python app.py
```
Open: http://localhost:5000

## CRUD Operations Explained

| Operation | HTTP Method | SQL Command | Route | Description |
|-----------|-------------|-------------|-------|-------------|
| **C**reate | POST | INSERT INTO | `/add` | Add new student |
| **R**ead | GET | SELECT | `/` | Display all students |
| **U**pdate | POST | UPDATE | `/edit/<id>` | Modify existing student |
| **D**elete | GET | DELETE | `/delete/<id>` | Remove student |

## Key Files
```
part-2/
├── app.py              <- CRUD operations
├── templates/
│   ├── index.html      <- List all students with Edit/Delete buttons
│   ├── add.html        <- Form to add new student
│   └── edit.html       <- Form to edit existing student
└── README.md
```

## New Concepts

### 1. Form Handling
```python
@app.route('/add', methods=['GET', 'POST'])
def add_student():
    if request.method == 'POST':  # Form submitted
        name = request.form['name']  # Get form data
```

### 2. Redirect After Action
```python
return redirect(url_for('index'))  # Go to home page
```

### 3. Flash Messages
```python
flash('Student added!', 'success')  # Show message once
```

## Learned:
- HTML forms with POST method
- request.form to get form data
- UPDATE and DELETE SQL commands
- redirect() and url_for() functions
- Flash messages for user feedback

## Exercises Performed
1. Added a "Search" feature to find students by name

@app.route('/')
def index():
    # This line grabs what you typed in the search box
    search = request.args.get('search', '') [cite: 1, 2]

    conn = get_db_connection() [cite: 1, 2]

    if search:
        # This SQL command finds names that "LOOK LIKE" your search term
        students = conn.execute(
            'SELECT * FROM students WHERE name LIKE ? ORDER BY id DESC',
            (f'%{search}%',) # The % signs mean "anything can be before or after the term"
        ).fetchall() [cite: 1, 2]
    else:
        # If no search term, just show everyone
        students = conn.execute(
            'SELECT * FROM students ORDER BY id ASC'
        ).fetchall() [cite: 1, 2]

    conn.close() [cite: 1, 2]
    return render_template('index.html', students=students, search=search) [cite: 1, 2]

2. Added validation to check if email already exists before adding

# Inside the @app.route('/add', methods=['GET', 'POST']) function:
if request.method == 'POST':
    # ... (code to get name, email, course from form) ...

    conn = get_db_connection() [cite: 1, 2]

    # This SQL command checks if the email already exists in the table
    existing_email = conn.execute(
        'SELECT 1 FROM students WHERE email = ?',
        (email,)
    ).fetchone() [cite: 1, 2]

    # If the database found a match (existing_email is not None)
    if existing_email:
        conn.close() [cite: 1, 2]
        # Send a red error message to the user
        flash('Email already exists! Please use a different email.', 'danger') [cite: 1, 2]
        # Stop everything and send them back to the add page
        return redirect(url_for('add_student')) [cite: 1, 2]

    # Only if the email is UNIQUE does the code reach this part:
    conn.execute(
        'INSERT INTO students (name, email, course) VALUES (?, ?, ?)',
        (name, email, course)
    ) [cite: 1, 2]