# To-do-list
import tkinter as tk
from tkinter import messagebox, filedialog

class ToDoApp:
    def __init__(self, root):
        self.root = root
        self.root.title("📝 To-Do List")
        self.root.geometry("400x500")
        self.root.configure(bg="#2c3e50")

        self.tasks = []

        self.create_widgets()

    def create_widgets(self):
        tk.Label(self.root, text="To-Do List", font=("Helvetica", 18, "bold"), bg="#2c3e50", fg="white").pack(pady=10)

        self.task_entry = tk.Entry(self.root, font=("Helvetica", 13), width=25)
        self.task_entry.pack(pady=10)

        add_btn = tk.Button(self.root, text="Add Task", width=15, command=self.add_task, bg="#27ae60", fg="white")
        add_btn.pack()

        self.listbox = tk.Listbox(self.root, font=("Helvetica", 13), width=30, height=10, selectbackground="#34495e")
        self.listbox.pack(pady=10)

        done_btn = tk.Button(self.root, text="Mark as Done ✅", width=15, command=self.mark_done, bg="#2980b9", fg="white")
        done_btn.pack(pady=5)

        delete_btn = tk.Button(self.root, text="Delete Task 🗑", width=15, command=self.delete_task, bg="#c0392b", fg="white")
        delete_btn.pack(pady=5)

        clear_btn = tk.Button(self.root, text="Clear All", width=15, command=self.clear_all, bg="#7f8c8d", fg="white")
        clear_btn.pack(pady=5)

        save_btn = tk.Button(self.root, text="Save Tasks 💾", width=15, command=self.save_tasks, bg="#8e44ad", fg="white")
        save_btn.pack(pady=5)

        load_btn = tk.Button(self.root, text="Load Tasks 📂", width=15, command=self.load_tasks, bg="#f39c12", fg="white")
        load_btn.pack(pady=5)

    def add_task(self):
        task = self.task_entry.get().strip()
        if task != "":
            self.tasks.append(task)
            self.listbox.insert(tk.END, task)
            self.task_entry.delete(0, tk.END)
        else:
            messagebox.showwarning("Input Error", "Please enter a task.")

    def mark_done(self):
        selected = self.listbox.curselection()
        if selected:
            index = selected[0]
            task = self.listbox.get(index)
            if not task.endswith("✅"):
                self.listbox.delete(index)
                task += " ✅"
                self.listbox.insert(index, task)
        else:
            messagebox.showwarning("Selection Error", "Please select a task.")

    def delete_task(self):
        selected = self.listbox.curselection()
        if selected:
            index = selected[0]
            self.listbox.delete(index)
            del self.tasks[index]
        else:
            messagebox.showwarning("Selection Error", "Please select a task.")

    def clear_all(self):
        self.tasks.clear()
        self.listbox.delete(0, tk.END)

    def save_tasks(self):
        file_path = filedialog.asksaveasfilename(defaultextension=".txt", title="Save Tasks")
        if file_path:
            with open(file_path, "w") as f:
                for i in range(self.listbox.size()):
                    f.write(self.listbox.get(i) + "\n")
            messagebox.showinfo("Saved", "Tasks saved successfully!")

    def load_tasks(self):
        file_path = filedialog.askopenfilename(title="Load Tasks")
        if file_path:
            self.clear_all()
            with open(file_path, "r") as f:
                for line in f:
                    task = line.strip()
                    self.tasks.append(task)
                    self.listbox.insert(tk.END, task)
            messagebox.showinfo("Loaded", "Tasks loaded successfully!")

if __name__ == "__main__":
    root = tk.Tk()
    app = ToDoApp(root)
    root.mainloop()
