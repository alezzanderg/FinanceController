import os
import tkinter as tk
from tkinter import ttk
from tkcalendar import DateEntry
import csv


def add_record(is_expense=True):
    date = expense_date_entry.get() if is_expense else income_date_entry.get()
    category = expense_category_entry.get() if is_expense else income_source_entry.get()
    amount = expense_amount_entry.get() if is_expense else income_amount_entry.get()

    if date and category and amount:
        record_type = "Expense" if is_expense else "Income"
        with open("finance_data.csv", mode="a", newline="") as file:
            writer = csv.writer(file)
            writer.writerow([date, category, amount, record_type])
            if is_expense:
                expense_date_entry.delete(0, tk.END)
                expense_category_entry.delete(0, tk.END)
                expense_amount_entry.delete(0, tk.END)
            else:
                income_date_entry.delete(0, tk.END)
                income_source_entry.delete(0, tk.END)
                income_amount_entry.delete(0, tk.END)
            update_view()
    else:
        field = "expense" if is_expense else "income"
        status_label.config(text=f"Please fill all the fields for {field}!", fg="red")

def delete_record():
    selected_item = finance_tree.selection()
    if selected_item:
        item_text = finance_tree.item(selected_item, "values")
        new_rows = []
        with open("finance_data.csv", mode="r") as file:
            reader = csv.reader(file)
            for row in reader:
                if row != list(item_text):
                    new_rows.append(row)

        with open("finance_data.csv", mode="w", newline="") as file:
            writer = csv.writer(file)
            writer.writerows(new_rows)

def update_view():
    total_expense, total_income = 0, 0
    finance_tree.delete(*finance_tree.get_children())

    if os.path.exists("finance_data.csv"):
        with open("finance_data.csv", mode="r") as file:
            reader = csv.reader(file)
            for row in reader:
                date, category, amount, record_type = row

                if record_type == "Expense":
                    total_expense += float(amount)
                elif record_type == "Income":
                    total_income += float(amount)

                finance_tree.insert("", tk.END, values=(date, category, amount, record_type))

    balance = total_income - total_expense
    balance_label.config(text=f"Balance: {balance:.2f}")

# Create the main application window
root = tk.Tk()
root.title("Finance Tracker")

# Create labels and entries for adding expenses
expense_date_label = tk.Label(root, text="Expense Date (YYYY-MM-DD):")
expense_date_label.grid(row=0, column=0, padx=5, pady=5)
expense_date_entry = tk.Entry(root)
expense_date_entry.grid(row=0, column=1, padx=5, pady=5)

expense_category_label = tk.Label(root, text="Expense Category:")
expense_category_label.grid(row=1, column=0, padx=5, pady=5)
expense_category_entry = tk.Entry(root)
expense_category_entry.grid(row=1, column=1, padx=5, pady=5)

expense_amount_label = tk.Label(root, text="Expense Amount:")
expense_amount_label.grid(row=2, column=0, padx=5, pady=5)
expense_amount_entry = tk.Entry(root)
expense_amount_entry.grid(row=2, column=1, padx=5, pady=5)

add_expense_button = tk.Button(root, text="Add Expense", command=lambda: add_record(True))
add_expense_button.grid(row=3, column=0, columnspan=2, padx=5, pady=10)

# Create labels and entries for adding incomes
income_date_label = tk.Label(root, text="Income Date (YYYY-MM-DD):")
income_date_label.grid(row=4, column=0, padx=5, pady=5)
income_date_entry = tk.Entry(root)
income_date_entry.grid(row=4, column=1, padx=5, pady=5)

income_source_label = tk.Label(root, text="Income Source:")
income_source_label.grid(row=5, column=0, padx=5, pady=5)
income_source_entry = tk.Entry(root)
income_source_entry.grid(row=5, column=1, padx=5, pady=5)

income_amount_label = tk.Label(root, text="Income Amount:")
income_amount_label.grid(row=6, column=0, padx=5, pady=5)
income_amount_entry = tk.Entry(root)
income_amount_entry.grid(row=6, column=1, padx=5, pady=5)

add_income_button = tk.Button(root, text="Add Income", command=lambda: add_record(False))
add_income_button.grid(row=7, column=0, columnspan=2, padx=5, pady=10)

# Create a treeview to display finances
columns = ("Date", "Category", "Amount", "Type")
finance_tree = ttk.Treeview(root, columns=columns, show="headings")
finance_tree.heading("Date", text="Date")
finance_tree.heading("Category", text="Category")
finance_tree.heading("Amount", text="Amount")
finance_tree.heading("Type", text="Type")
finance_tree.grid(row=8, column=0, columnspan=3, padx=5, pady=5)

# Create a label to show the status of record addition and deletion
status_label = tk.Label(root, text="", fg="green")
status_label.grid(row=9, column=0, columnspan=2, padx=5, pady=5)

# Create buttons to view and delete records
view_button = tk.Button(root, text="Update View", command=update_view)
view_button.grid(row=10, column=0, padx=5, pady=10)

delete_button = tk.Button(root, text="Delete Record", command=delete_record)
delete_button.grid(row=10, column=1, padx=5, pady=10)

# Create a label to display the balance
balance_label = tk.Label(root, text="")
balance_label.grid(row=11, column=0, columnspan=2, padx=5, pady=5)

# Check if the 'finance_data.csv' file exists; create it with headers if it doesn't
if not os.path.exists("finance_data.csv"):
    with open("finance_data.csv", "w", newline="") as file:
        writer = csv.writer(file)
        writer.writerow(["Date", "Category", "Amount", "Type"])


# Display existing records on application start
update_view()

root.mainloop()
