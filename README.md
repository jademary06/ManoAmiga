import tkinter as tk
from tkinter import messagebox, ttk

class DonationApp:
    def __init__(self, root):
        self.root = root
        self.root.title("\u2605 ManoAmiga Donation System \u2605")
        self.root.geometry("900x500")
        self.root.resizable(False, False)
        self.root.configure(bg="#1E1E1E")

        self.donors = []
        self.recipients = []
        self.donations = []

        # Main Menu
        self.main_menu()

    def main_menu(self):
        self.clear_frame()

        tk.Label(
            self.root,
            text="\u2605 ManoAmiga Donation Management System \u2605",
            font=("Arial", 18, "bold"),
            bg="#1E1E1E",
            fg="#F5F5DC"
        ).pack(pady=20)

        button_style = {
            "bg": "#4A2F2F",
            "fg": "#F5F5DC",
            "activebackground": "#6A5F5F",
            "width": 20,
            "font": ("Arial", 12, "bold"),
            "relief": "flat",
            "bd": 0
        }

        tk.Button(self.root, text="Add Donation", command=self.add_donation_gui, **button_style).pack(pady=10)
        tk.Button(self.root, text="Add Recipient", command=self.add_recipient_gui, **button_style).pack(pady=10)
        tk.Button(self.root, text="View Recipients", command=self.view_recipients_gui, **button_style).pack(pady=10)
        tk.Button(self.root, text="List of Donations", command=self.list_donations_gui, **button_style).pack(pady=10)
        tk.Button(self.root, text="Exit App", command=self.root.quit, **button_style).pack(pady=10)

    def add_donation_gui(self):
        self.clear_frame()

        tk.Label(
            self.root, text="Add Donation", font=("Arial", 14, "bold"), bg="#1E1E1E", fg="#F5F5DC"
        ).pack(pady=10)

        tk.Label(self.root, text="Donor Name:", bg="#1E1E1E", fg="#F5F5DC").pack()
        donor_name = tk.Entry(self.root)
        donor_name.pack(pady=5)

        tk.Label(self.root, text="Email:", bg="#1E1E1E", fg="#F5F5DC").pack()
        email = tk.Entry(self.root)
        email.pack(pady=5)

        tk.Label(self.root, text="Address:", bg="#1E1E1E", fg="#F5F5DC").pack()
        address = tk.Entry(self.root)
        address.pack(pady=5)

        tk.Label(self.root, text="Donation Type:", bg="#1E1E1E", fg="#F5F5DC").pack()
        donation_type = tk.StringVar(self.root)
        donation_type.set("Select")
        tk.OptionMenu(self.root, donation_type, "Money", "Food", "Clothes", "Toys", "Medical Supply", "School Supply", "Hygiene Kit").pack(pady=5)

        tk.Label(self.root, text="Quantity:", bg="#1E1E1E", fg="#F5F5DC").pack()
        quantity = tk.Entry(self.root)
        quantity.pack(pady=5)

        tk.Label(self.root, text="Donation Date (YYYY-MM-DD):", bg="#1E1E1E", fg="#F5F5DC").pack()
        date = tk.Entry(self.root)
        date.pack(pady=5)

        def save_donation():
            if not (donor_name.get() and email.get() and address.get() and donation_type.get() != "Select" and quantity.get() and date.get()):
                messagebox.showerror("Error", "All fields must be filled!")
                return

            donor = {
                "name": donor_name.get(),
                "email": email.get(),
                "address": address.get()
            }
            donation = {
                "type": donation_type.get(),
                "quantity": quantity.get(),
                "date": date.get()
            }

            self.donors.append(donor)
            self.donations.append({"donor": donor, "donation": donation})

            messagebox.showinfo("Success", "Donation added successfully!")
            self.main_menu()

        button_style = {
            "bg": "#4A2F2F",
            "fg": "#F5F5DC",
            "activebackground": "#6A5F5F",
            "width": 15,
            "font": ("Arial", 12, "bold"),
            "relief": "flat",
            "bd": 0
        }
        tk.Button(self.root, text="Save Donation", command=save_donation, **button_style).pack(pady=10)
        tk.Button(self.root, text="Back", command=self.main_menu, **button_style).pack()

    def add_recipient_gui(self):
        self.clear_frame()

        tk.Label(
            self.root, text="Add Recipient", font=("Arial", 14, "bold"), bg="#1E1E1E", fg="#F5F5DC"
        ).pack(pady=10)

        tk.Label(self.root, text="Recipient Name:", bg="#1E1E1E", fg="#F5F5DC").pack()
        recipient_name = tk.Entry(self.root)
        recipient_name.pack(pady=5)

        tk.Label(self.root, text="Location:", bg="#1E1E1E", fg="#F5F5DC").pack()
        location = tk.Entry(self.root)
        location.pack(pady=5)

        def save_recipient():
            if not (recipient_name.get() and location.get()):
                messagebox.showerror("Error", "All fields must be filled!")
                return

            recipient = {
                "name": recipient_name.get(),
                "location": location.get()
            }

            self.recipients.append(recipient)
            messagebox.showinfo("Success", "Recipient added successfully!")
            self.main_menu()

        button_style = {
            "bg": "#4A2F2F",
            "fg": "#F5F5DC",
            "activebackground": "#6A5F5F",
            "width": 15,
            "font": ("Arial", 12, "bold"),
            "relief": "flat",
            "bd": 0
        }
        tk.Button(self.root, text="Save Recipient", command=save_recipient, **button_style).pack(pady=10)
        tk.Button(self.root, text="Back", command=self.main_menu, **button_style).pack()

    def view_recipients_gui(self):
        self.clear_frame()

        tk.Label(
            self.root, text="List of Recipients", font=("Arial", 14, "bold"), bg="#1E1E1E", fg="#F5F5DC"
        ).pack(pady=10)

        tree = ttk.Treeview(self.root, columns=("Name", "Location"), show="headings", height=15)
        tree.heading("Name", text="Name")
        tree.heading("Location", text="Location")
        tree.column("Name", anchor="center", width=300)
        tree.column("Location", anchor="center", width=300)

        for recipient in self.recipients:
            tree.insert("", tk.END, values=(recipient["name"], recipient["location"]))

        tree.pack(pady=10)

        style = ttk.Style()
        style.configure("Treeview", background="#1E1E1E", foreground="#F5F5DC", fieldbackground="#1E1E1E", rowheight=25)
        style.configure("Treeview.Heading", background="#4A2F2F", foreground="#F5F5DC", font=("Arial", 12, "bold"))

        button_style = {
            "bg": "#4A2F2F",
            "fg": "#F5F5DC",
            "activebackground": "#6A5F5F",
            "width": 15,
            "font": ("Arial", 12, "bold"),
            "relief": "flat",
            "bd": 0
        }
        tk.Button(self.root, text="Back", command=self.main_menu, **button_style).pack(pady=10)

    def list_donations_gui(self):
        self.clear_frame()

        tk.Label(
            self.root, text="List of Donations", font=("Arial", 14, "bold"), bg="#1E1E1E", fg="#F5F5DC"
        ).pack(pady=10)

        tree = ttk.Treeview(self.root, columns=("Donor", "Type", "Quantity", "Date"), show="headings", height=15)
        tree.heading("Donor", text="Donor")
        tree.heading("Type", text="Type")
        tree.heading("Quantity", text="Quantity")
        tree.heading("Date", text="Date")
        tree.column("Donor", anchor="center", width=200)
        tree.column("Type", anchor="center", width=150)
        tree.column("Quantity", anchor="center", width=100)
        tree.column("Date", anchor="center", width=150)

        for record in self.donations:
            donor = record["donor"]
            donation = record["donation"]
            tree.insert("", tk.END, values=(donor["name"], donation["type"], donation["quantity"], donation["date"]))

        tree.pack(pady=10)

        style = ttk.Style()
        style.configure("Treeview", background="#1E1E1E", foreground="#F5F5DC", fieldbackground="#1E1E1E", rowheight=25)
        style.configure("Treeview.Heading", background="#4A2F2F", foreground="#F5F5DC", font=("Arial", 12, "bold"))

        button_style = {
            "bg": "#4A2F2F",
            "fg": "#F5F5DC",
            "activebackground": "#6A5F5F",
            "width": 15,
            "font": ("Arial", 12, "bold"),
            "relief": "flat",
            "bd": 0
        }
        tk.Button(self.root, text="Back", command=self.main_menu, **button_style).pack(pady=10)

    def clear_frame(self):
        for widget in self.root.winfo_children():
            widget.destroy()

if __name__ == "__main__":
    root = tk.Tk()
    app = DonationApp(root)
    root.mainloop()
