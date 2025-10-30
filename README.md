# KlingonCalculator.py
I decided to combine the wonderful Alien language of klingon and a simple code of a calculator. Fusing two worlds together. Lets imagine Alien used calculators maybe this is how it would look like. Note, I am still learning and getting better everyday.
import tkinter as tk
from tkinter import font as tkfont

# Klingon number mapping (pIqaD numerals roughly transliterated)
klingon_digits = {
    '0': '',  # example glyphs — depend on your pIqaD font
    '1': '',
    '2': '',
    '3': '',
    '4': '',
    '5': '',
    '6': '',
    '7': '',
    '8': '',
    '9': ''
}

def to_klingon(num_str):
    return ''.join(klingon_digits.get(ch, ch) for ch in num_str)

def from_klingon(klingon_str):
    reverse = {v: k for k, v in klingon_digits.items()}
    return ''.join(reverse.get(ch, ch) for ch in klingon_str)

# GUI setup
root = tk.Tk()
root.title("Klingon Calculator ⚔️")
root.config(bg="black")

expression = ""

# Try to use Klingon font if installed
available_fonts = list(tkfont.families())
if "Klingon pIqaD HaSta" in available_fonts:
    klingon_font = ("Klingon pIqaD HaSta", 26)
else:
    klingon_font = ("Courier", 24, "bold")  # fallback

def press(key):
    global expression
    expression += key
    display_var.set(to_klingon(expression))

def clear():
    global expression
    expression = ""
    display_var.set("")

def evaluate():
    global expression
    try:
        result = str(eval(from_klingon(expression)))
        expression = result
        display_var.set(to_klingon(result))
    except Exception:
        display_var.set("⚠️ ERROR")
        expression = ""

display_var = tk.StringVar()

# Display field
display = tk.Entry(root, textvariable=display_var, font=klingon_font,
                   bg="black", fg="#00FF00", justify='right', bd=10, relief="flat")
display.grid(row=0, column=0, columnspan=4, padx=5, pady=5, sticky="nsew")

# Buttons
buttons = [
    ('7', 1, 0), ('8', 1, 1), ('9', 1, 2), ('/', 1, 3),
    ('4', 2, 0), ('5', 2, 1), ('6', 2, 2), ('*', 2, 3),
    ('1', 3, 0), ('2', 3, 1), ('3', 3, 2), ('-', 3, 3),
    ('0', 4, 0), ('.', 4, 1), ('=', 4, 2), ('+', 4, 3)
]

for (text, row, col) in buttons:
    if text == '=':
        btn = tk.Button(root, text='=', bg="#880000", fg="white",
                        font=("Courier", 20, "bold"), command=evaluate,
                        height=2, width=5)
    else:
        btn = tk.Button(root, text=to_klingon(text) if text.isdigit() else text,
                        bg="#222", fg="#00FF00", font=klingon_font,
                        command=lambda t=text: press(t), height=2, width=5)
    btn.grid(row=row, column=col, padx=3, pady=3, sticky="nsew")

# Clear button
clear_btn = tk.Button(root, text="CLEAR", bg="#550000", fg="white",
                      font=("Courier", 16, "bold"), command=clear, height=2, width=22)
clear_btn.grid(row=5, column=0, columnspan=4, pady=5)

# Make layout responsive
for i in range(6):
    root.grid_rowconfigure(i, weight=1)
for j in range(4):
    root.grid_columnconfigure(j, weight=1)

root.mainloop()
