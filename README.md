# -
Подилько Сергей 
import tkinter as tk
from tkinter import messagebox, ttk
import random
import json
import os

# Путь к файлу истории
HISTORY_FILE = "tasks.Json"

class RandomTaskGenerator:
 def __init__(self, root):
 self.Root = root
 self.Root.Title("Random Task Generator")
 self.Root.Geometry("600x500")

 # Список предопределённых задач
 self.Tasks = [
 {"type": "учёба", "task": "Прочитать статью"},
 {"type": "спорт", "task": "Сделать зарядку"},
 {"type": "работа", "task": "Ответить на письма"},
 {"type": "быт", "task": "Помыть посуду"},
 {"type": "творчество", "task": "Нарисовать эскиз"}
]

 # Загрузка истории из файла
 self.History = self.Load_history()

 self.Setup_ui()

 def setup_ui(self):
 # Заголовок
 tk.Label(self.Root, text="Генератор случайных задач", font=("Arial", 16)).Pack(pady=10)

 # Кнопка генерации
 tk.Button(self.Root, text="Сгенерировать задачу", command=self.Generate_task).Pack(pady=5)

 # Поле для отображения сгенерированной задачи
 self.Task_label = tk.Label(self.Root, text="", font=("Arial", 12), wraplength=500)
 self.Task_label.Pack(pady=10)

 # Фильтр по типу
 tk.Label(self.Root, text="Фильтр по типу:").Pack(pady=5)
 self.Filter_var = tk.StringVar(value="все")
 filter_combo = ttk.Combobox(self.Root, textvariable=self.Filter_var,
 values=["все","учёба","спорт","работа","быт","творчество"])
 filter_combo.Pack(pady=5)

 # Кнопка фильтрации
 tk.Button(self.Root, text="Применить фильтр", command=self.Filter_tasks).Pack(pady=5)

 # Список истории
 tk.Label(self.Root, text="История задач:", font=("Arial", 10)).Pack(pady=5)
 self.History_list = tk.Listbox(self.Root, width=60, height=10)
 self.History_list.Pack(pady=5)

 # Обновление списка истории
 self.Update_history_list()

 def generate_task(self):
 task = random.Choice(self.Tasks)
 self.Task_label.Config(text=task["task"])
 self.History.Append(task)
 self.Save_history()
 self.Update_history_list()

 def load_history(self):
 if os.Path.Exists(HISTORY_FILE):
 with open(HISTORY_FILE,"r", encoding="utf-8") as f:
 return json.Load(f)
 return []

 def save_history(self):
 with open(HISTORY_FILE,"w", encoding="utf-8") as f:
 json.Dump(self.History, f, ensure_ascii=False, indent=2)

 def update_history_list(self):
 self.History_list.Delete(0, tk.END)
 for task in self.History:
 self.History_list.Insert(tk.END, f"{task['type']}: {task['task']}")
