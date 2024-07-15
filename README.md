# table
import tkinter as tk
from tkinter import ttk
import pandas as pd

class TableApp:
    def __init__(self, root):
        self.root = root
        self.root.title("Table App")
        self.root.geometry("600x400")

        # Dataframe to store table data
        self.df = pd.DataFrame(columns=["Name", "Age", "Country"])

        # Setup UI components
        self.setup_ui()

    def setup_ui(self):
        self.name_label = tk.Label(self.root, text="Name")
        self.name_label.pack(pady=5)
        self.name_entry = tk.Entry(self.root)
        self.name_entry.pack(pady=5)

        self.age_label = tk.Label(self.root, text="Age")
        self.age_label.pack(pady=5)
        self.age_entry = tk.Entry(self.root)
        self.age_entry.pack(pady=5)

        self.country_label = tk.Label(self.root, text="Country")
        self.country_label.pack(pady=5)
        self.country_entry = tk.Entry(self.root)
        self.country_entry.pack(pady=5)

        self.add_button = tk.Button(self.root, text="Add to Table", command=self.add_to_table)
        self.add_button.pack(pady=10)

        self.table_frame = tk.Frame(self.root)
        self.table_frame.pack(pady=10)

        self.table = ttk.Treeview(self.table_frame, columns=("Name", "Age", "Country"), show='headings')
        self.table.heading("Name", text="Name")
        self.table.heading("Age", text="Age")
        self.table.heading("Country", text="Country")
        self.table.pack()

    def add_to_table(self):
        name = self.name_entry.get()
        age = self.age_entry.get()
        country = self.country_entry.get()

        if name and age and country:
            self.df = self.df.append({"Name": name, "Age": age, "Country": country}, ignore_index=True)
            self.update_table()

            # Clear the entry fields
            self.name_entry.delete(0, tk.END)
            self.age_entry.delete(0, tk.END)
            self.country_entry.delete(0, tk.END)

    def update_table(self):
        # Clear the table
        for row in self.table.get_children():
            self.table.delete(row)

        # Insert new data
        for index, row in self.df.iterrows():
            self.table.insert("", tk.END, values=(row["Name"], row["Age"], row["Country"]))

# Create the main window
root = tk.Tk()
app = TableApp(root)
root.mainloop()
