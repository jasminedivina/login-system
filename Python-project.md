import hashlib
import os

def hash_password(password):
    return hashlib.sha256(password.encode()).hexdigest()

def load_users():
    users = {}
    if os.path.exists("users.txt"):
        with open("users.txt", "r") as f:
            for line in f:
                username, hashed = line.strip().split(",")
                users[username] = hashed
    return users

def save_user(username, hashed_password):
    with open("users.txt", "a") as f:
        f.write(f"{username},{hashed_password}\n")

def register():
    users = load_users()
    username = input("Enter a new username: ")
    if username in users:
        print("Username already exists!")
        return
    password = input("Enter a password: ")
    hashed = hash_password(password)
    save_user(username, hashed)
    print("Registration successful!")

def login():
    users = load_users()
    username = input("Enter your username: ")
    password = input("Enter your password: ")
    hashed = hash_password(password)

    if users.get(username) == hashed:
        print("Login successful! Welcome,", username)
    else:
        print("Invalid username or password.")

# Main menu
while True:
    print("\n--- Welcome to the Login System ---")
    print("1. Register")
    print("2. Login")
    print("3. Exit")

    choice = input("Choose an option (1-3): ")

    if choice == '1':
        register()
    elif choice == '2':
        login()
    elif choice == '3':
        print("Goodbye!")
        break
    else:
        print("Invalid choice. Try again.")

