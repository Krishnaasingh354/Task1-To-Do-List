# Task1-To-Do-List
import json
import os
from datetime import datetime

class InternshipTodoList:
    def __init__(self, filename="internship_tasks.json"):
        self.filename = filename
        self.tasks = self.load_tasks()
    
    def load_tasks(self):
        """Load tasks from JSON file"""
        if os.path.exists(self.filename):
            with open(self.filename, 'r') as f:
                return json.load(f)
        return []
    
    def save_tasks(self):
        """Save tasks to JSON file"""
        with open(self.filename, 'w') as f:
            json.dump(self.tasks, f, indent=4)
    
    def add_task(self, title, description="", priority="Medium", deadline=""):
        """Add a new task"""
        task = {
            "id": len(self.tasks) + 1,
            "title": title,
            "description": description,
            "priority": priority,
            "deadline": deadline,
            "completed": False,
            "created_at": datetime.now().strftime("%Y-%m-%d %H:%M:%S")
        }
        self.tasks.append(task)
        self.save_tasks()
        print(f"\n✓ Task '{title}' added successfully!")
    
    def view_tasks(self, show_completed=True):
        """Display all tasks"""
        if not self.tasks:
            print("\n📝 No tasks found. Add your first task!")
            return
        
        print("\n" + "="*70)
        print("INTERNSHIP TO-DO LIST".center(70))
        print("="*70)
        
        for task in self.tasks:
            if not show_completed and task["completed"]:
                continue
            
            status = "✓" if task["completed"] else "○"
            priority_symbol = {"High": "🔴", "Medium": "🟡", "Low": "🟢"}.get(task["priority"], "⚪")
            
            print(f"\n[{status}] Task #{task['id']} {priority_symbol}")
            print(f"    Title: {task['title']}")
            if task['description']:
                print(f"    Description: {task['description']}")
            print(f"    Priority: {task['priority']}")
            if task['deadline']:
                print(f"    Deadline: {task['deadline']}")
            print(f"    Status: {'Completed' if task['completed'] else 'Pending'}")
        
        print("\n" + "="*70)
    
    def complete_task(self, task_id):
        """Mark a task as completed"""
        for task in self.tasks:
            if task["id"] == task_id:
                task["completed"] = True
                self.save_tasks()
                print(f"\n✓ Task #{task_id} marked as completed!")
                return
        print(f"\n✗ Task #{task_id} not found.")
    
    def delete_task(self, task_id):
        """Delete a task"""
        for i, task in enumerate(self.tasks):
            if task["id"] == task_id:
                removed = self.tasks.pop(i)
                self.save_tasks()
                print(f"\n✓ Task '{removed['title']}' deleted successfully!")
                return
        print(f"\n✗ Task #{task_id} not found.")
    
    def view_pending_tasks(self):
        """View only pending tasks"""
        self.view_tasks(show_completed=False)

def display_menu():
    """Display the main menu"""
    print("\n" + "="*70)
    print("INTERNSHIP TASK MANAGER - MENU".center(70))
    print("="*70)
    print("1. Add New Task")
    print("2. View All Tasks")
    print("3. View Pending Tasks Only")
    print("4. Mark Task as Completed")
    print("5. Delete Task")
    print("6. Exit")
    print("="*70)

def main():
    todo = InternshipTodoList()
    
    print("\n🎯 Welcome to Internship To-Do List Manager!")
    
    while True:
        display_menu()
        choice = input("\nEnter your choice (1-6): ").strip()
        
        if choice == "1":
            print("\n--- ADD NEW TASK ---")
            title = input("Task Title: ").strip()
            description = input("Description (optional): ").strip()
            priority = input("Priority (High/Medium/Low) [Medium]: ").strip() or "Medium"
            deadline = input("Deadline (optional, e.g., 2024-12-31): ").strip()
            
            if title:
                todo.add_task(title, description, priority, deadline)
            else:
                print("\n✗ Task title cannot be empty!")
        
        elif choice == "2":
            todo.view_tasks()
        
        elif choice == "3":
            todo.view_pending_tasks()
        
        elif choice == "4":
            todo.view_tasks()
            try:
                task_id = int(input("\nEnter Task ID to mark as completed: "))
                todo.complete_task(task_id)
            except ValueError:
                print("\n✗ Invalid Task ID!")
        
        elif choice == "5":
            todo.view_tasks()
            try:
                task_id = int(input("\nEnter Task ID to delete: "))
                confirm = input(f"Are you sure you want to delete task #{task_id}? (yes/no): ").lower()
                if confirm == "yes":
                    todo.delete_task(task_id)
                else:
                    print("\n✗ Deletion cancelled.")
            except ValueError:
                print("\n✗ Invalid Task ID!")
        
        elif choice == "6":
            print("\n👋 Thank you for using Internship Task Manager. Good luck with your internship!")
            break
        
        else:
            print("\n✗ Invalid choice! Please select 1-6.")

if __name__ == "__main__":
    main()
