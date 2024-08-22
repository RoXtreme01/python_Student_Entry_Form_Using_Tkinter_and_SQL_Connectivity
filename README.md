# python_Student_Entry_Form_Using_Tkinter_and_SQL_Connectivity
#"This is a simple Student Entry Form in Which you can only Stores Data in Mysql database"
# This is only for Pc

## First Of all Run these Commands In your cmd if you don't have required Libraries ##

# Important
# 1. pip install tkinter
# 2. pip install mysql.connector

# Program Begins
import tkinter as tk  # Import the Tkinter library for GUI creation
import mysql.connector as m  # Import MySQL connector for database operations

# Initialize the main window
root = tk.Tk()
root.geometry("950x450")  # Set the size of the window
root.title("Entry Form")  # Set the title of the window
root.configure(bg="#333333")  # Set the background color of the window
root.resizable(False, False)  # Disable window resizing

# Connect to the MySQL database
mycon = m.connect(host="localhost", user="root", password="123456", database="detail")
cur = mycon.cursor()  # Create a cursor object for executing SQL queries

# Label to display completion messages (like success or error messages)
complete = tk.Label(root, text="", bg="#333333", font="Arial 20 bold")
complete.place(x=255, y=380)

# Function to handle the submission of form data
def submit():
    # Retrieve the data entered by the user
    name = entry1.get()
    class1 = entry2.get()
    section = entry3.get()
    rollno = entry4.get()

    # Check if any of the fields are empty
    if name == "" or class1 == "" or section == "" or rollno == "":
        complete.config(text="MUST FILL PROPER DETAILS !", fg="red")
        return

    try:
        # Insert the data into the MySQL database
        data = "insert into student_info values('{}', {}, '{}', {})".format(name, class1, section, rollno)
        cur.execute(data)  # Execute the SQL query
        mycon.commit()  # Commit the transaction to the database
        complete.config(text="Your Record Added Successfully!!!", fg="green")
    except Exception as e:
        # If an error occurs, display the error message
        complete.config(text=f"{e} !!!", fg="red", font="Arial 10 bold")

# Function to clear the entry fields and reset the completion message
def clear_entries():
    entry1.delete(0, tk.END)  # Clear the content of entry1 (name field)
    entry2.delete(0, tk.END)  # Clear the content of entry2 (class field)
    entry3.delete(0, tk.END)  # Clear the content of entry3 (section field)
    entry4.delete(0, tk.END)  # Clear the content of entry4 (roll number field)
    complete.config(text="")  # Reset the completion message

# Label for the form title
label = tk.Label(root, text="STUDENT ENTRY FORM", font="Arial 18 bold", bg="#333333", fg="#FF3399")
label.pack(pady=30)

# Entry fields for user input
entry1 = tk.Entry(root, width=25, font="Arial 18")  # Entry for student name
entry1.pack(pady=10)

entry2 = tk.Entry(root, width=25, font="Arial 18")  # Entry for class (numeric)
entry2.pack(pady=10)

entry3 = tk.Entry(root, width=25, font="Arial 18")  # Entry for section (alphabet)
entry3.pack(pady=10)

entry4 = tk.Entry(root, width=25, font="Arial 18")  # Entry for roll number
entry4.pack(pady=15)

# Labels for each entry field
Name = tk.Label(root, text="Student Name", font="Arial 15 bold", bg="#333333", fg="white")
Name.place(x=130, y=110)

Class = tk.Label(root, text="Class (numeric)", font="Arial 15 bold", bg="#333333", fg="white")
Class.place(x=130, y=160)

Section = tk.Label(root, text="Section (alphabet)", font="Arial 15 bold", bg="#333333", fg="white")
Section.place(x=130, y=210)

Boards_Rollno = tk.Label(root, text="Boards Roll no.", font="Arial 15 bold", bg="#333333", fg="white")
Boards_Rollno.place(x=130, y=260)

# Submit button to trigger the submit function
submit_button = tk.Button(root, text="Submit", font="Arial 13 bold", fg="white", bg="#FF3399", command=submit)
submit_button.pack(pady=15)

# Next Record button to clear the entries for the next record
next_button = tk.Button(root, text="Next Record", font="Arial 13 bold", width=13, fg="white", bg="green", cursor="hand2", command=clear_entries)
next_button.place(x=780, y=400)

# Start the main loop to run the application
root.mainloop()
