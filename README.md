import json
from datetime import date

FILENAME = "todo.json"

def load_tasks():
    try:
        with open(FILENAME, "r") as f:
            return json.load(f)
    except FileNotFoundError:
        return {}

def save_tasks(tasks):
    with open(FILENAME, "w") as f:
        json.dump(tasks, f, indent=4)

def show_today(tasks):
    today = str(date.today())
    if today in tasks and tasks[today]:
        print(f"\n✅ Tasks for {today}:")
        for i, t in enumerate(tasks[today], 1):
            print(f"{i}. {t}")
    else:
        print(f"\n📭 No tasks for {today} yet.")

def main():
    tasks = load_tasks()
    today = str(date.today())

    print("=== DAILY TO-DO LIST ===")
    show_today(tasks)

    while True:
        print("\nOptions:")
        print("1. Add new task")
        print("2. Show today's tasks")
        print("3. Exit")
        choice = input("Choose: ")

        if choice == "1":
            task = input("Enter your task: ")
            tasks.setdefault(today, []).append(task)
            save_tasks(tasks)
            print("✅ Task added!")
        elif choice == "2":
            show_today(tasks)
        elif choice == "3":
            break
        else:
            print("⚠️ Invalid choice!")

if __name__ == "__main__":
    main()

