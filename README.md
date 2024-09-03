# Python_Sqlite_Code
import sqlite3

# Connect to SQLite database (or create it if it doesn't exist)
conn = sqlite3.connect('students.db')
cursor = conn.cursor()

# Create the 'student' table if it doesn't already exist
cursor.execute('''
CREATE TABLE IF NOT EXISTS student (
    Rollno INTEGER PRIMARY KEY,
    Name TEXT,
    Sub1 INTEGER,
    Sub2 INTEGER,
    Sub3 INTEGER,
    Total INTEGER
)
''')

# 1. Insert at least 5 to 10 records
students = [
    (1, 'Alice', 85, 90, 88, 263),
    (2, 'Bob', 78, 82, 80, 240),
    (3, 'Charlie', 92, 91, 89, 272),
    (4, 'David', 70, 75, 80, 225),
    (5, 'Eve', 88, 92, 85, 265)
]

cursor.executemany('''
INSERT INTO student (Rollno, Name, Sub1, Sub2, Sub3, Total)
VALUES (?, ?, ?, ?, ?, ?)
''', students)

# 2. Update a Specific Record
# Update record where Rollno = 2
cursor.execute('''
UPDATE student
SET Name = ?, Sub1 = ?, Sub2 = ?, Sub3 = ?, Total = ?
WHERE Rollno = ?
''', ('Bob', 80, 85, 82, 247, 2))

# 3. Delete a Specific Record
# Delete record where Rollno = 4
cursor.execute('''
DELETE FROM student
WHERE Rollno = ?
''', (4,))

# 4. Display student detail who got highest total marks
cursor.execute('''
SELECT * FROM student
ORDER BY Total DESC
LIMIT 1
''')

highest_total_student = cursor.fetchone()
print('Student with highest total marks:', highest_total_student)

# Commit changes and close the connection
conn.commit()
conn.close()
