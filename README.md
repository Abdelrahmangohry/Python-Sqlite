# -----------------------------------------------------
# -- Employees Skills & Data Base --
# -----------------------------------------------------


# Organize the data base
# Import SQLite Module
import sqlite3

# Create Database And Connect
db = sqlite3.connect('Skills.db')

# Setting Up The Cursor
cr = db.cursor()

cr.execute('create table if not exists skills(Name text, Progress integer, User_id integer)')

# Save and Close connection 'function'
def commit_and_close():
  db.commit()
  db.close() 
  print("Saved & Connection To Database Is Closed")


# Current User ID
uid = 1

input_message = """
What Do You Want To Do?
Choose Option:
"s" => Show All Skills
"a" => Add New Skill
"d" => Delete A Skill
"u" => Update Skill Progress
"q" => Quit The App
"""

# Input Option Choose
user_input = input(input_message).strip().lower()

# Command List
commands_list = ["s", "a", "d", "u", "q"]

# Define The Methods
def show_skills():
      cr.execute(f"select * from skills where user_id = '{uid}'")
      results = cr.fetchall()
      print(f"You Have {len(results)} Skills.")
      if len(results) > 0:
            print("Showing Skills With Progress: ")
      for row in results:
            print(f"Skill => {row[0]},", end=" ")
            print(f"Progress => {row[1]}%") 
      commit_and_close()

def add_skill():
     sk = input("Write Skill Name: ").strip().capitalize()
     cr.execute(f"select name from skills where name = '{sk}' and user_id = '{uid}'")
     results = cr.fetchone()

     if results == None:
          prog = input("Write Skill Progress ").strip()
          cr.execute(f"insert into skills(name, progress, user_id) values('{sk}', '{prog}', '{uid}')")
          print('your skill added successfully')
     else:
           print('dubplicated value')    
     commit_and_close()

def delete_skill():
    sk = input("Write Skill Name: ").strip().capitalize()
    cr.execute(f"delete from skills where name = '{sk}' and user_id = '{uid}'")
    commit_and_close()

def update_skill():
     sk = input("Write Skill Name: ").strip().capitalize()
     prog = input("Write The New Skill Progress ").strip()
     cr.execute(f"update skills set progress = '{prog}' where name = '{sk}' and user_id = '{uid}'")
     commit_and_close()

# Check If Command Is Exists
if user_input in commands_list:
    if user_input == "s":

        show_skills()

    elif user_input == "a":

        add_skill()

    elif user_input == "d":

        delete_skill()

    elif user_input == "u":

        update_skill()

    else:

        print("App Is Closed.")

        commit_and_close()

else:

    print(f"Sorry This Command \"{user_input}\" Is Not Found")



