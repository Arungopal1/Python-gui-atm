# ATM SIMULATOR 

import tkinter as tk
from tkinter import messagebox
from getpass import getpass

class ATM_GUI:

    def __init__(self):
        self.pin = ""
        self.balance = 0
        self.wrong_pin_count = 0

        self.root = tk.Tk()
        self.root.title("SBI ATM")

        self.display()

        self.menu_frame = tk.Frame(self.root)
        self.menu_frame.pack()

        self.create_menu()

    def display(self):
        tk.Label(self.root, text="WELCOME to SBI ATM", font=("Helvetica", 16)).pack(pady=10)

    def create_menu(self):
        menu_label = tk.Label(self.menu_frame, text="Please select your transaction:")
        menu_label.grid(row=0, column=0, columnspan=2, pady=10)

        options = ["Create PIN", "Deposit", "Withdraw", "Check Balance", "Exit"]
        for i, option in enumerate(options, start=1):
            btn = tk.Button(self.menu_frame, text=option, width=20, command=lambda opt=option: self.handle_menu(opt))
            btn.grid(row=i, column=0, pady=5)

    def handle_menu(self, option):
        if option == "Create PIN":
            self.create_pin_window()
        elif option == "Deposit":
            self.deposit_window()
        elif option == "Withdraw":
            self.withdraw_window()
        elif option == "Check Balance":
            self.check_balance_window()
        elif option == "Exit":
            self.exit()

    def create_pin_window(self):
        pin_window = tk.Toplevel(self.root)
        pin_window.title("Create PIN")

        tk.Label(pin_window, text="Enter your PIN (4 digits):").pack(pady=10)

        pin_entry = tk.Entry(pin_window, show="*")
        pin_entry.pack()

        def set_pin():
            pin = pin_entry.get()
            if len(pin) == 4 and pin.isdigit():
                self.pin = pin
                messagebox.showinfo("Success", "PIN set successfully.")
                pin_window.destroy()
            else:
                messagebox.showerror("Error", "PIN must be 4 digits and integer.")

        tk.Button(pin_window, text="Set PIN", command=set_pin).pack()

    def deposit_window(self):
        deposit_window = tk.Toplevel(self.root)
        deposit_window.title("Deposit")

        tk.Label(deposit_window, text="Enter your PIN:").pack()
        pin_entry = tk.Entry(deposit_window, show="*")
        pin_entry.pack()

        tk.Label(deposit_window, text="Enter amount to deposit:").pack()
        amount_entry = tk.Entry(deposit_window)
        amount_entry.pack()

        def deposit():
            pin = pin_entry.get()
            amount = amount_entry.get()
            if pin == self.pin:
                try:
                    amount = float(amount)
                    if amount > 0:
                        self.balance += amount
                        messagebox.showinfo("Success", "Deposit successful. Current balance: {}".format(self.balance))
                        deposit_window.destroy()
                    else:
                        messagebox.showerror("Error", "Invalid amount.")
                except ValueError:
                    messagebox.showerror("Error", "Invalid amount.")
            else:
                messagebox.showerror("Error", "Incorrect PIN.")

        tk.Button(deposit_window, text="Deposit", command=deposit).pack()

    def withdraw_window(self):
        withdraw_window = tk.Toplevel(self.root)
        withdraw_window.title("Withdraw")

        tk.Label(withdraw_window, text="Enter your PIN:").pack()
        pin_entry = tk.Entry(withdraw_window, show="*")
        pin_entry.pack()

        tk.Label(withdraw_window, text="Enter amount to withdraw:").pack()
        amount_entry = tk.Entry(withdraw_window)
        amount_entry.pack()

        def withdraw():
            pin = pin_entry.get()
            amount = amount_entry.get()
            if pin == self.pin:
                try:
                    amount = float(amount)
                    if 0 < amount <= self.balance:
                        self.balance -= amount
                        messagebox.showinfo("Success", "Withdrawal successful. Current balance: {}".format(self.balance))
                        withdraw_window.destroy()
                    else:
                        messagebox.showerror("Error", "Invalid amount or insufficient balance.")
                except ValueError:
                    messagebox.showerror("Error", "Invalid amount.")
            else:
                messagebox.showerror("Error", "Incorrect PIN.")

        tk.Button(withdraw_window, text="Withdraw", command=withdraw).pack()

    def check_balance_window(self):
        check_balance_window = tk.Toplevel(self.root)
        check_balance_window.title("Check Balance")

        tk.Label(check_balance_window, text="Enter your PIN:").pack()
        pin_entry = tk.Entry(check_balance_window, show="*")
        pin_entry.pack()

        def check_balance():
            pin = pin_entry.get()
            if pin == self.pin:
                messagebox.showinfo("Balance", "Your current balance is: {}".format(self.balance))
                check_balance_window.destroy()
            else:
                messagebox.showerror("Error", "Incorrect PIN.")

        tk.Button(check_balance_window, text="Check Balance", command=check_balance).pack()

    def exit(self):
        self.root.destroy()


if __name__ == "__main__":
    atm = ATM_GUI()
    atm.root.mainloop()
