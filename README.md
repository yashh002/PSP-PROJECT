# PSP-PROJECT
print('Personal Finance Tracker\n')

import tkinter as tk
from tkinter import messagebox
import matplotlib.pyplot as plt

total_income = 0
bud = {}
exp = {}

def get_income():
    global total_income
    src = int(input("Enter Your Income Sources: "))
    total_income = 0
    for i in range(1, src + 1):
        income = int(input(f"Enter Income from Source {i}: "))
        total_income += income
    print(f"\nYour total Income is {total_income}\n")
    return total_income

def get_budget():
    global bud, exp
    print("Enter your monthly budget\n")
    ch = 'y'
    while ch.lower() == 'y':
        category = input("Enter your Budget category: ")
        amount = int(input(f"Enter {category} amount: "))
        bud[category] = amount
        exp[category] = 0
        ch = input("Do you want to add another category? (y/n): ")
    
    budget = sum(bud.values())
    if budget > total_income:
        print(f"\nYour total budget ({budget}) exceeds your income ({total_income})\n")
    else:
        print(f"\nYour total monthly budget is {budget}\n")
    return bud, exp, budget

def get_expense():
    global exp
    if not bud:
        print("\nPlease enter your budget before adding expenses.\n")
        return {}, 0

    print("Enter your actual monthly expenses\n")
    for category in exp:
        exp[category] = int(input(f"Enter expense for {category}: "))
    expense = sum(exp.values())
    print(f"\nYour actual monthly expense is {expense}\n")
    return exp, expense

def display():
    if not bud:
        print("\nNo budget data found. Please enter a budget first.\n")
        return
    print("\n----- Summary -----\n")
    for category in bud:
        budget_amt = bud[category]
        expense_amt = exp.get(category, 0)
        status = "Saved" if expense_amt < budget_amt else "Over Budget" if expense_amt > budget_amt else "Exact"
        print(f"{category:<10} Budget = {budget_amt:<10} Expense = {expense_amt:<10} --> {status}")

    
    total_budget = sum(bud.values())
    total_expense = sum(exp.values())
    print("\n-------------------------------------")
    print(f"Total Budget: {total_budget}")
    print(f"Total Expense: {total_expense}")
    print(f"Difference: {total_budget - total_expense} ({'Saved' if total_expense < total_budget else 'Over Budget'})")
    print("-------------------------------------\n")

def update():
    global total_income, bud, exp
    ch = int(input("Enter Choice:\n1. Add Extra Income\n2. Update Budget\n3. Update Expense\n"))
    
    if 'Other' not in bud:
        bud['Other'] = 0
    if 'Other' not in exp:
        exp['Other'] = 0

    if ch == 1:
        extra_income = int(input("Enter Extra Income: "))
        total_income += extra_income
        print(f"\nYour total Income is now {total_income}\n")

    elif ch == 2:
        print("Do you want to:")
        print("1. Add amount to 'Other' category")
        print("2. Add another budget category")
        choice = int(input("Enter choice (1/2): "))
        
        if choice == 1:
            amt = int(input("Enter amount to add to 'Other': "))
            bud['Other'] += amt
            print(f"'Other' budget updated. New amount: {bud['Other']}\n")
        elif choice == 2:
            extra_category = input("Enter Budget Category to Add/Update: ")
            amt = int(input(f"Enter amount for {extra_category}: "))
            if extra_category in bud:
                bud[extra_category] += amt
            else:
                bud[extra_category] = amt
                exp[extra_category] = 0
            print(f"Budget updated for {extra_category}\n")
        else:
            print("Invalid choice.\n")

    elif ch == 3:
        if not bud:
            print("\nPlease enter your budget before updating expenses.\n")
            return

        print("Do you want to:")
        print("1. Add amount to 'Other' category")
        print("2. Add another expense category")
        choice = int(input("Enter choice (1/2): "))

        if choice == 1:
            amt1 = int(input("Enter expense amount to add to 'Other': "))
            exp['Other'] += amt1
            print(f"'Other' expense updated. New amount: {exp['Other']}\n")
        elif choice == 2:
            extra_category = input("Enter Expense Category to Add/Update: ")
            amt1 = int(input(f"Enter expense for {extra_category}: "))
            if extra_category in exp:
                exp[extra_category] += amt1
            else:
                exp[extra_category] = amt1
                if extra_category not in bud:
                    bud[extra_category] = 0
            print(f"Expense updated for {extra_category}\n")
        else:
            print("Invalid choice.\n")

    else:
        print("Invalid Choice")

def show_bar_graph():
    if not bud:
        messagebox.showinfo("Info", "No budget data available. Please enter budget and expenses.")
        return
    
    categories = list(bud.keys())
    budget_values = [bud[cat] for cat in categories]
    expense_values = [exp.get(cat, 0) for cat in categories]
    
    x = range(len(categories))
    
    plt.figure(figsize=(10, 6))
    plt.bar(x, budget_values, width=0.4, label='Budget', align='center')
    plt.bar([i + 0.4 for i in x], expense_values, width=0.4, label='Expense', align='center')
    
    plt.xlabel('Categories')
    plt.ylabel('Amount')
    plt.title('Budget vs Expense')
    plt.xticks([i + 0.2 for i in x], categories)
    plt.legend()
    plt.tight_layout()
    plt.show()

def main():
    while True:
        choice = int(input("Enter Choice:\n1. Add Income\n2. Add Budget\n3. Add Expense\n4. Display\n5. Update\n6. Show Graph\n7. Exit\n"))
        if choice == 1:
            get_income()
        elif choice == 2:
            get_budget()
        elif choice == 3:
            get_expense()
        elif choice == 4:
            display()
        elif choice == 5:
            update()
        elif choice == 6:
            show_bar_graph()
        elif choice == 7:
            break
        else:
            print("Invalid Choice")

main()

