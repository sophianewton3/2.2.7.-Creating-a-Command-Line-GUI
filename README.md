import tkinter as tk
import tkinter.scrolledtext as tksc
import subprocess
from tkinter.filedialog import asksaveasfilename

# This function runs the network commands
def do_command(command):
    # Clear the text box for a fresh start
    output_display.delete(1.0, tk.END)
    
    # Get the URL the user typed
    target = url_entry.get()
    
    # If the box is empty, use this default IP so it doesn't error out
    if target == "":
        target = "8.8.8.8"
    
    output_display.insert(tk.END, "Running " + command + " on " + target + "...\n")
    output_display.update()

    # Runs the command and sends the text back to the display box
    # 'shell=True' makes it work on school computers/Windows easily
    process = subprocess.Popen(command + " " + target, stdout=subprocess.PIPE, shell=True, universal_newlines=True)
    
    for line in process.stdout:
        output_display.insert(tk.END, line)
        output_display.update()

# This function saves the output to a text file
def save_to_file():
    file_path = asksaveasfilename(defaultextension=".txt")
    if file_path:
        with open(file_path, "w") as f:
            content = output_display.get(1.0, tk.END)
            f.write(content)

# --- CREATE THE WINDOW ---
app = tk.Tk()
app.title("Network Tool")

# Input Section
tk.Label(app, text="Enter Website URL:").pack(pady=5)
url_entry = tk.Entry(app)
url_entry.pack(pady=5)

# Action Buttons
tk.Button(app, text="Run Ping", command=lambda: do_command("ping")).pack(pady=2)
tk.Button(app, text="Run Tracert", command=lambda: do_command("tracert")).pack(pady=2)
tk.Button(app, text="Run NSLookup", command=lambda: do_command("nslookup")).pack(pady=2)

# Output Box
output_display = tksc.ScrolledText(app, width=65, height=15)
output_display.pack(pady=10)

# Save Button
tk.Button(app, text="Save", command=save_to_file).pack(pady=5)

app.mainloop()
