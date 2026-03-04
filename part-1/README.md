# Part 1: Basic Flask with SQLite Database

## One-Line Summary
Basic Flask app with SQLite connection and one simple table (Create & Read)

## Prerequisites
- Flask basics (routes, templates, render_template)
- Completed flask-basics course

## How to Run
```bash
# Navigate to this folder
cd part-1

# Activate virtual environment
# Windows:
venv\Scripts\activate
# Mac/Linux:
source venv/bin/activate

# Install Flask (if not installed)
pip install flask

# Run the app
python app.py
```

## Test It
1. Open browser: http://localhost:5000
2. You'll see empty student list
3. Click "Add Sample Student" button
4. See the student appear in the table!

## Key Files
```
part-1/
├── app.py              <- Main Flask app with database code
├── templates/
│   └── index.html      <- Display students table
├── students.db         <- Database file (created when you run app)
└── README.md           <- You are here
```

## Key Concepts
| Concept | Explanation |
|---------|-------------|
| `sqlite3.connect()` | Opens connection to database file |
| `conn.execute()` | Runs SQL command |
| `conn.commit()` | Saves changes to database |
| `conn.close()` | Closes the connection |
| `fetchall()` | Gets all rows from SELECT query |

## Learned
- Connecting Flask to SQLite database
- Creating a table
- Inserting data (Create)
- Reading data (Read)

## Exercise performed
```
- modified 'add_sample_student()' to add different students with different names!
```

