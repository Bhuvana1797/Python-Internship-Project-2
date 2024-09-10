# Task Manager

## Introduction

*Task Manager* is a console-based to-do list application in Python. It lets users add, edit, delete, and mark tasks as complete, with data persistence through file storage.

## Features

- *Add Tasks*: Create new tasks.
- *Edit Tasks*: Update task descriptions.
- *Delete Tasks*: Remove tasks.
- *Mark as Complete*: Set tasks as done.
- *Data Persistence*: Save and load tasks from a file.

## Technologies

- *Python*: Main programming language.
- *File Handling*: For task storage.
- *IPyWidgets*: For interactive elements in Jupyter.


import json
import os
from IPython.display import display
import ipywidgets as widgets

TASK_FILE = 'tasks.json'

def load_tasks():
    """Load tasks from the file."""
    if os.path.exists(TASK_FILE):
        with open(TASK_FILE, 'r') as file:
            return json.load(file)
    return []

def save_tasks(tasks):
    """Save tasks to the file."""
    with open(TASK_FILE, 'w') as file:
        json.dump(tasks, file, indent=4)

def display_tasks(tasks):
    """Display the current list of tasks."""
    output.clear_output()
    if not tasks:
        print("No tasks found.")
    else:
        for idx, task in enumerate(tasks, 1):
            status = 'Complete' if task['complete'] else 'Incomplete'
            print(f"{idx}. {task['description']} - {status}")

def add_task(description):
    """Add a new task."""
    if description:
        tasks.append({'description': description, 'complete': False})
        save_tasks(tasks)
        display_tasks(tasks)
    else:
        print("Task description cannot be empty!")

def edit_task(task_id, new_description):
    """Edit an existing task."""
    try:
        task_id = int(task_id) - 1
        print(f"Editing task with ID: {task_id}")  # Debugging line
        if 0 <= task_id < len(tasks):
            if new_description:
                tasks[task_id]['description'] = new_description
                save_tasks(tasks)
                display_tasks(tasks)
            else:
                print("New description cannot be empty!")
        else:
            print("Invalid task number. Please ensure the task number is correct.")
    except ValueError:
        print("Invalid input. Please enter a number.")

def delete_task(task_id):
    """Delete a task."""
    try:
        task_id = int(task_id) - 1
        print(f"Deleting task with ID: {task_id}")  # Debugging line
        if 0 <= task_id < len(tasks):
            tasks.pop(task_id)
            save_tasks(tasks)
            display_tasks(tasks)
        else:
            print("Invalid task number. Please ensure the task number is correct.")
    except ValueError:
        print("Invalid input. Please enter a number.")

def mark_task_complete(task_id):
    """Mark a task as complete."""
    try:
        task_id = int(task_id) - 1
        print(f"Marking task as complete with ID: {task_id}")  # Debugging line
        if 0 <= task_id < len(tasks):
            tasks[task_id]['complete'] = True
            save_tasks(tasks)
            display_tasks(tasks)
        else:
            print("Invalid task number. Please ensure the task number is correct.")
    except ValueError:
        print("Invalid input. Please enter a number.")

# Load initial tasks
tasks = load_tasks()

# Create widgets for user interaction
description_text = widgets.Text(placeholder='Enter task description')
add_button = widgets.Button(description="Add Task")

edit_task_id = widgets.Text(placeholder='Enter task number to edit')
new_description = widgets.Text(placeholder='Enter new description')
edit_button = widgets.Button(description="Edit Task")

delete_task_id = widgets.Text(placeholder='Enter task number to delete')
delete_button = widgets.Button(description="Delete Task")

complete_task_id = widgets.Text(placeholder='Enter task number to mark complete')
complete_button = widgets.Button(description="Mark Complete")

# Output area for displaying tasks and messages
output = widgets.Output()

def on_add_button_clicked(b):
    add_task(description_text.value)
    description_text.value = ''

def on_edit_button_clicked(b):
    edit_task(edit_task_id.value, new_description.value)
    edit_task_id.value = ''
    new_description.value = ''

def on_delete_button_clicked(b):
    delete_task(delete_task_id.value)
    delete_task_id.value = ''

def on_complete_button_clicked(b):
    mark_task_complete(complete_task_id.value)
    complete_task_id.value = ''

add_button.on_click(on_add_button_clicked)
edit_button.on_click(on_edit_button_clicked)
delete_button.on_click(on_delete_button_clicked)
complete_button.on_click(on_complete_button_clicked)

# Display widgets
display(description_text, add_button, 
        edit_task_id, new_description, edit_button, 
        delete_task_id, delete_button, 
        complete_task_id, complete_button, 
        output)

# Initial display of tasks
display_tasks(tasks)
