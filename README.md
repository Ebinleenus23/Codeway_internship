# Working on to-do list using Graphical User Interface (GUI)
# importing tkinter
import tkinter as tk
from tkinter import *  
from PIL import Image,ImageTk

# Creating a dashboard
root = tk.Tk()
root.title("To-Do List Dashboard")
root.geometry("450x650+400+100")  # Correct format: widthxheight
root.resizable(False, False)  # Prevent resizing

task_list= []

# Function for adding to-do list
def addTask():
    task = task_entry.get()
    task_entry.delete(0, END)
  
    if task:
        with open("tasklist.txt", 'a') as taskfile:
            taskfile.write(f"\n(task)")
        task_list.append(task)
        listbox.insert( END, task)
        
# Function for deleting the to-do list
def deleteTask():
    global task_list
    task = str(listbox.get(ANCHOR))
    if task in task_list:
        task_list.remove(task)
        with open("tasklist.txt",'w') as taskfie:
            for task in task_list:
                taskfie.write(task+"\n")
                
        listbox.delete( ANCHOR)



def openTaskFile():
    
    try:
        global task_list
        with open("tasklist.txt","r") as taskfile:
            tasks = taskfile.readlines()
        
        for task in tasks:
            if task !="\n":
                task_list.append(task)
                listbox.insert(END,task)
    except:
        file = open('tasklist.txt','w')
        file.close()
        

# Correctly load and convert the image to a PhotoImage
icon_image = ImageTk.PhotoImage(Image.open("task.png"))
Top_image = ImageTk.PhotoImage(Image.open("topbar.png"))
dock_image = ImageTk.PhotoImage(Image.open("dock.png")) 
note_image = ImageTk.PhotoImage(Image.open("task.png")) 


# Set the window icon
root.iconphoto(False, icon_image)

# Top bar
Label(root,image=Top_image).pack()

# Dock 
Label(root, image=dock_image, bg="#32405b").place(x=30,y=25)

# Note
Label(root, image=note_image, bg="#32405b").place(x=30,y=25)

# Heading
heading = Label(root, text= "TASK LIST", font="arial 20 bold",fg="black",bg="#32405b")
heading.place(x=130,y=20)

# Main 
frame = Frame(root, width=450,height=50,bg="white") 
frame.place(x=0,y=180)

task = StringVar()
task_entry = Entry(frame, width=18, font="arial 20", bd=0)
task_entry.place (x=10,y=7)
task_entry.focus()

#Button Functionality
button = Button(frame, text="ADD", font="arial 20 bold", width=6,bg="#5a95ff",fg="#fff", bd=0, command=addTask) 
button.place (x=300,y=0)


# Listbox
frame1 = Frame(root,bd=3,width=700,height=280,bg="#32405b")
frame1.pack(pady=(160,0))

listbox = Listbox(frame1,font=('arial',12),width=40,height=16,bg="#32405b",fg="white",cursor="hand2",selectbackground="#5a95ff")
listbox.pack(side=LEFT, fill=BOTH, padx=2)
scrollbar = Scrollbar(frame1)
scrollbar.pack(side = RIGHT, fill = BOTH)

listbox.config(yscrollcommand=scrollbar.set)
scrollbar.config(command = listbox.yview)

# Calling the function
openTaskFile()

#delete
delete_image = ImageTk.PhotoImage(Image.open("delete.png"))
Button(root, image=delete_image, bd=0, command = deleteTask).pack(side=BOTTOM,pady=13)


# Your app logic and widgets here
root.mainloop()

############################################################################################################################################

# Working on creating a calculator which can be used for arithematic operations
# creating arthimetic functions
def add(x, y):
    return x + y

def subtract(x, y):
    return x - y

def multiply(x, y):
    return x * y

def divide(x, y):
    if y != 0:
        return x / y
    else:
        return "Cannot divide by zero."
    
# Calculator function
def calculator():
    print("Simple Calculator")
    print("Operations:")
    print("1. Addition (+)")
    print("2. Subtraction (-)")
    print("3. Multiplication (*)")
    print("4. Division (/)")

    try:
        num1 = float(input("Enter the first number: "))
        num2 = float(input("Enter the second number: "))
        operation = input("Choose operation (1-4): ")

        if operation == '1':
            result = add(num1, num2)
            print(f"Result: {num1} + {num2} = {result}")
        elif operation == '2':
            result = subtract(num1, num2)
            print(f"Result: {num1} - {num2} = {result}")
        elif operation == '3':
            result = multiply(num1, num2)
            print(f"Result: {num1} * {num2} = {result}")
        elif operation == '4':
            result = divide(num1, num2)
            print(f"Result: {num1} / {num2} = {result}")
        else:
            print("Invalid operation. Please choose a number between 1 and 4.")

    except ValueError:
        print("Invalid input. Please enter valid numbers.")

if __name__ == "__main__":
    calculator()

#####################################################################################################################################################

# Creating a Password generator

import secrets
import string

def generate_password(length=12, complexity="medium"):
    """
    Generate a random password based on the specified length and complexity level.

    Parameters:
    - length (int): Desired length of the password (default is 12).
    - complexity (str): Complexity level of the password (low, medium, high; default is medium).

    Returns:
    - str: The generated password.
    """
    if complexity == "low":
        characters = string.ascii_letters + string.digits
    elif complexity == "medium":
        characters = string.ascii_letters + string.digits + string.punctuation
    elif complexity == "high":
        characters = string.ascii_letters + string.digits + string.punctuation + string.ascii_letters.upper()
    else:
        print("Invalid complexity level. Using medium complexity.")
        characters = string.ascii_letters + string.digits + string.punctuation

    password = ''.join(secrets.choice(characters) for _ in range(length))
    return password

def main():
    """
    Main function to interact with the user and generate a password.
    """
    print("Password Generator")
    
    try:
        # Get user input for password length
        length = int(input("Enter the desired length of the password: "))
        
        # Check if the entered length is valid
        if length <= 0:
            print("Invalid length. Please enter a positive integer.")
            return

        # Get user input for complexity level
        complexity = input("Choose complexity level (low, medium, high): ").lower()

        # Generate and display the password
        password = generate_password(length, complexity)
        print(f"Generated Password: {password}")

    except ValueError:
        print("Invalid input. Please enter a valid integer for the password length.")

if __name__ == "__main__":
    main()

########################################################################################################################################

# Weather forcasting application

import requests

def get_weather(api_key, location):
    base_url = "http://api.openweathermap.org/data/2.5/weather"
    params = {
        "q": location,
        "appid": "9aee2c863ca33fff08381c1dea14da9c",
        "units": "metric"  
    }

    try:
        response = requests.get(base_url, params=params)
        response.raise_for_status()  
        weather_data = response.json()
        return weather_data
    except requests.exceptions.RequestException as e:
        print(f"Error fetching weather data: {e}")
        return None

def display_weather(weather_data):
    if weather_data:
        print("\nCurrent Weather Conditions:")
        print(f"City: {weather_data['name']}")
        print(f"Temperature: {weather_data['main']['temp']}°C")
        print(f"Humidity: {weather_data['main']['humidity']}%")
        print(f"Wind Speed: {weather_data['wind']['speed']} m/s")
        print(f"Description: {weather_data['weather'][0]['description']}")
    else:
        print("Unable to fetch weather data.")

def main():
    print("Weather Forecast Application")
    location = input("Enter the name of a city or a zip code: ")
    api_key = '9aee2c863ca33fff08381c1dea14da9c'
    weather_data = get_weather(api_key, location)
    display_weather(weather_data)

if __name__ == "__main__":
    main()

#########################################################################################################################

# Quiz game on python

def new_game():

    guesses = []
    correct_guesses = 0
    question_num = 1

    for key in questions:
        print("-------------------------")
        print(key)
        for i in options[question_num-1]:
            print(i)
        guess = input("Enter (A, B, C, or D): ")
        guess = guess.upper()
        guesses.append(guess)

        correct_guesses += check_answer(questions.get(key), guess)
        question_num += 1

    display_score(correct_guesses, guesses)


def check_answer(answer, guess):

    if answer == guess:
        print("CORRECT!")
        return 1
    else:
        print("WRONG!")
        return 0

    
def display_score(correct_guesses, guesses):
    print("-------------------------")
    print("RESULTS")
    print("-------------------------")

    print("Answers: ", end="")
    for i in questions:
        print(questions.get(i), end=" ")
    print()

    print("Guesses: ", end="")
    for i in guesses:
        print(i, end=" ")
    print()

    score = int((correct_guesses/len(questions))*100)
    print("Your score is: "+str(score)+"%")


def play_again():

    response = input("Do you want to play again? (yes or no): ")
    response = response.upper()

    if response == "YES":
        return True
    else:
        return False



questions = {
    "What is the world's largest ocean?": "C",
    "Who wrote the novel Frankenstein?": "A",
    "What is the tallest mountain on Earth?": "B",
    "What year did the first iPhone launch?": "D",
    "What is the chemical formula for water?": "A",
    "What country invented the printing press?": "C",
    "What is the largest living organism on Earth?": "D",
    "What is the speed of light in a vacuum?": "C"
}

options = [
    ["A. Pacific Ocean", "B. Atlantic Ocean", "C. Indian Ocean", "D. Arctic Ocean"],
    ["A. Mary Shelley", "B. Bram Stoker", "C. H.G. Wells", "D. Edgar Allan Poe"],
    ["A. Mount Everest", "B. Mount K2", "C. Mount Kilimanjaro", "D. Mount Fuji"],
    ["A. 2004", "B. 2005", "C. 2006", "D. 2007"],
    ["A. H2O", "B. CO2", "C. O2", "D. N2"],
    ["A. China", "B. Germany", "C. France", "D. England"],
    ["A. Blue whale", "B. Giant sequoia tree", "C. Honey fungus", "D. Great Barrier Reef"],
    ["A. 186,000 miles per second", "B. 299,792 kilometers per second", "C. 3 x 10^8 meters per second", "D. All of the above"]
]

new_game()

while play_again():
    new_game()

print("Byeeeeee!")

##################################################################################################################################################
