# Python-Sql
import sqlite3

# Connect to SQLite (creates file if not exists)
conn = sqlite3.connect("students.db")
cursor = conn.cursor()

# Create table
cursor.execute("""
CREATE TABLE IF NOT EXISTS students (
    id INTEGER PRIMARY KEY,
    name TEXT NOT NULL,
    age INTEGER NOT NULL
)
""")
conn.commit()

def add_student():
    id = int(input("Enter Student ID: "))
    name = input("Enter Student Name: ")
    age = int(input("Enter Student Age: "))

    cursor.execute("INSERT INTO students (id, name, age) VALUES (?, ?, ?)", (id, name, age))
    conn.commit()
    print(" Student added successfully!")

def view_students():
    cursor.execute("SELECT * FROM students")
    rows = cursor.fetchall()
    if rows:
        print("\n--- Student List ---")
        for row in rows:
            print(f"ID: {row[0]} | Name: {row[1]} | Age: {row[2]}")
    else:
        print("No students found!")

def search_student():
    id = int(input("Enter Student ID to search: "))
    cursor.execute("SELECT * FROM students WHERE id=?", (id,))
    row = cursor.fetchone()
    if row:
        print(f"Student Found → ID: {row[0]} | Name: {row[1]} | Age: {row[2]}")
    else:
        print(" Student not found.")

def main():
    while True:
        print("\n=== Student Management System ===")
        print("1. Add Student")
        print("2. View Students")
        print("3. Search Student by ID")
        print("4. Exit")

        choice = input("Enter choice: ")

        if choice == "1":
            add_student()
        elif choice == "2":
            view_students()
        elif choice == "3":
            search_student()
        elif choice == "4":
            print("Exiting... Thank you!")
            break
        else:
            print("Invalid choice! Try again.")

    conn.close()

if __name__ == "__main__":
    main()
