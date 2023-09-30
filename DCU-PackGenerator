import tkinter as tk
from tkinter import filedialog, messagebox, Toplevel, Listbox, Button, Entry, Label, StringVar, ttk
import os
import json
import zipfile
import shutil
from datetime import date
from functools import partial
from tkinter import Text
import tkinter as tk


resource_pack_dir = "resource_pack"
uploaded_files = []
custom_model_data_counter = 1



# Map of Minecraft versions to their corresponding pack_format numbers
PACK_FORMAT_MAP = {
    "1.13-1.14": 4,
    "1.15-1.16": 5,
    "1.16-1.16.5": 6,
    "1.17": 7,
    "1.18-1.18.2": 8,
    "1.19-1.19.2": 9,
    "1.19.3": 12,
    "1.19.4": 13,
    "1.20-1.20.1": 15
}

class SymbolBox:
    def __init__(self, master):
        self.top = tk.Toplevel(master, bg='#2c2c2c')
        self.top.title("Symbols to copy")
        self.create_widgets()


    def create_widgets(self):
        symbols = """★ ☆ ✡ ✦ ✧ ✩ ✪ ✫ ✬ ✭ ✮ ✯ ✰ ⁂ ⁎ ⁑ ✢ ✣ ✤ ✥ ✱ ✲ ✳ ✴ ✵ ✶ ✷ ✸ ✹ ✺ ✻ ✼ ✽ ✾ ✿ ❀ ❁ ❂ ❃ ❇ ❈ ❉ ❊ ❋ ❄ ❆ ❅ ⋆ ≛ ᕯ ✲ ࿏ ꙰ ۞ 〈 〉 《 》 「 」 『 』 【 】 〔 〕 ︵ ︶ ︷ ︸ ︹ ︺ ︻ ︼ ︽ ︾ ︿ ﹀ ﹁ ﹂ ﹃ ﹄ ﹙ ﹚ ﹛ ﹜ ﹝ ﹞ ﹤ ﹥ （ ） ＜ ＞ ｛ ｝ 〖 〗 〘 〙 〚 〛 « » ‹ › 〈 〉 〱 ↕ ↖ ↗ ↘ ↙ ↚ ↛ ↜ ↝ ↞ ↟ ↠ ↡ ↢ ↣ ↤ ↥ ↦ ↧ ↨ ↩ ↪ ↫ ↬ ↭ ↮ ↯ ↰ ↱ ↲ ↳ ↴ ↶ ↷ ↸ ↹ ↺ ↻ ↼ ↽ ↾ ↿ ⇀ ⇁ ⇂ ⇃ ⇄ ⇅ ⇆ ⇇ ⇈ ⇉ ⇊ ⇋ ⇌ ⇍ ⇎ ⇏ ⇕ ⇖ ⇗ ⇘ ⇙ ⇚ ⇛ ⇜ ⇝ ⇞ ⇟ ⇠ ⇡ ⇢ ⇣ ⇤ ⇥ ⇦ ⇧ ⇨ ⇩ ⇪ ⌅ ⌆ ⌤ ⏎ ▶ ☇ ☈ ☊ ☋ ☌ ☍ ➔ ➘ ➙ ➚ ➛ ➜ ➝ ➞ ➟ ➠ ➡ ➢ ➣ ➤ ➥ ➦ ➧ ➨ ➩ ➪ ➫ ➬ ➭ ➮ ➯ ➱ ➲ ➳ ➴ ➵ ➶ ➷ ➸ ➹ ➺ ➻ ➼ ➽ ➾ ⤴ ⤵ ↵ ↓ ↔ ← → ↑ ⌦ ⌫ ⌧ ⇰ ⇫ ⇬ ⇭ ⇳ ⇮ ⇯ ⇱ ⇲ ⇴ ⇵ ⇷ ⇸ ⇹ ⇺ ⇑ ⇓ ⇽ ⇾ ⇿ ⬳ ⟿ ⤉ ⤈ ⇻ ⇼ ⬴ ⤀ ⬵ ⤁ ⬹ ⤔ ⬺ ⤕ ⬶ ⤅ ⬻ ⤖ ⬷ ⤐ ⬼ ⤗ ⬽ ⤘ ⤝ ⤞ ⤟ ⤠ ⤡ ⤢ ⤣ ⤤ ⤥ ⤦ ⤪ ⤨ ⤧ ⤩ ⤭ ⤮ ⤯ ⤰ ⤱ ⤲ ⤫ ⤬ ⬐ ⬎ ⬑ ⬏ ⤶ ⤷ ⥂ ⥃ ⥄ ⭀ ⥱ ⥶ ⥸ ⭂ ⭈ ⭊ ⥵ ⭁ ⭇ ⭉ ⥲ ⭋ ⭌ ⥳ ⥴ ⥆ ⥅ ⥹ ⥻ ⬰ ⥈ ⬾ ⥇ ⬲ ⟴ ⥷ ⭃ ⥺ ⭄ ⥉ ⥰ ⬿ ⤳ ⥊ ⥋ ⥌ ⥍ ⥎ ⥏ ⥐ ⥑ ⥒ ⥓ ⥔ ⥕ ⥖ ⥗ ⥘ ⥙ ⥚ ⥛ ⥜ ⥝ ⥞ ⥟ ⥠ ⥡ ⥢ ⥤ ⥣ ⥥ ⥦ ⥨ ⥧ ⥩ ⥮ ⥯ ⥪ ⥬ ⥫ ⥭ ⤌ ⤍ ⤎ ⤏ ⬸ ⤑ ⬱ ⟸ ⟹ ⟺ ⤂ ⤃ ⤄ ⤆ ⤇ ⤊ ⤋ ⭅ ⭆ ⟰ ⟱ ⇐ ⇒ ⇔ ⇶ ⟵ ⟶ ⟷ ⬄ ⬀ ⬁ ⬂ ⬃ ⬅ ⬆ ⬇ ⬈ ⬉ ⬊ ⬋ ⬌ ⬍ ⟻ ⟼ ⤒ ⤓ ⤙ ⤚ ⤛ ⤜ ⥼ ⥽ ⥾ ⥿ ⤼ ⤽ ⤾ ⤿ ⤸ ⤺ ⤹ ⤻ ⥀ ⥁ ⟲ ⟳"""
        # Create a canvas for the scrollbar
        self.canvas = tk.Canvas(self.top, bg='#2c2c2c')
        self.canvas.pack(side=tk.LEFT, fill=tk.BOTH, expand=True)


        # Add a scrollbar to the canvas
        scrollbar = tk.Scrollbar(self.top, orient="vertical", command=self.canvas.yview)
        scrollbar.pack(side=tk.RIGHT, fill=tk.Y)

        # Configure the canvas to respond to the scrollbar
        self.canvas.configure(yscrollcommand=scrollbar.set)

        # Bind the canvas to the mousewheel event
        self.canvas.bind_all("<MouseWheel>", self._on_mousewheel)

        # Create a frame inside the canvas to hold the buttons
        self.frame = tk.Frame(self.canvas, bg='#2c2c2c')
        self.canvas.create_window((0, 0), window=self.frame, anchor='nw')

        for symbol in symbols.split():
            btn = tk.Button(self.frame, text=symbol, command=partial(self.copy_to_clipboard, symbol), font=("Arial", 18)) # Increased the font size
            btn.pack(side=tk.TOP, padx=10, pady=10) # Increased the padding

        # Update the scroll region after adding buttons
        self.frame.update_idletasks()
        self.canvas.config(scrollregion=self.canvas.bbox('all'))

        # Set the height of the Toplevel window
        self.top.geometry("10x500")  # Setting width x height, you can adjust as needed

    def copy_to_clipboard(self, symbol):
        self.top.clipboard_clear()
        self.top.clipboard_append(symbol)
        self.top.update()

    def _on_mousewheel(self, event):
        self.canvas.yview_scroll(-1*(event.delta//120), "units")


# ToolTip class to show a tooltip when hovering over the color buttons
class ToolTip(object):
    def __init__(self, widget, text):
        self.widget = widget
        self.text = text
        self.tip_window = None

    def show_tip(self):
        if self.tip_window:
            return
        x, y, _, _ = self.widget.bbox("insert")
        x += self.widget.winfo_rootx() + 25
        y += self.widget.winfo_rooty() + 25
        self.tip_window = tw = tk.Toplevel(self.widget)
        tw.wm_overrideredirect(True)
        tw.wm_geometry(f"+{x}+{y}")
        label = tk.Label(tw, text=self.text, bg="white", relief="solid", borderwidth=1)
        label.pack(ipadx=1)

    def hide_tip(self):
        tw = self.tip_window
        self.tip_window = None
        if tw:
            tw.destroy()

COLOR_NAME_MAP = {
    "#000000": "Black",
    "#0000AA": "Dark Blue",
    "#00AA00": "Dark Green",
    "#00AAAA": "Dark Aqua",
    "#AA0000": "Dark Red",
    "#AA00AA": "Dark Purple",
    "#FFAA00": "Gold",
    "#AAAAAA": "Gray",
    "#555555": "Dark Gray",
    "#5555FF": "Blue",
    "#55FF55": "Green",
    "#55FFFF": "Aqua",
    "#FF5555": "Red",
    "#FF55FF": "Light Purple",
    "#FFFF55": "Yellow",
    "#FFFFFF": "White"
}

# Map of Minecraft color codes to their names
COLOR_CODE_MAP = {
    "#000000": "§0",  # black
    "#0000AA": "§1",  # dark_blue
    "#00AA00": "§2",  # dark_green
    "#00AAAA": "§3",  # dark_aqua
    "#AA0000": "§4",  # dark_red
    "#AA00AA": "§5",  # dark_purple
    "#FFAA00": "§6",  # gold
    "#AAAAAA": "§7",  # gray
    "#555555": "§8",  # dark_gray
    "#5555FF": "§9",  # blue
    "#55FF55": "§a",  # green
    "#55FFFF": "§b",  # aqua
    "#FF5555": "§c",  # red
    "#FF55FF": "§d",  # light_purple
    "#FFFF55": "§e",  # yellow
    "#FFFFFF": "§f"   # white
}


STYLE_CODE_MAP = {
    "bold": "§l",
    "italic": "§o",
    "underline": "§n",
    "strikethrough": "§m"
}

def initialize_resource_pack():
    os.makedirs(os.path.join(resource_pack_dir, "assets", "minecraft", "models", "item"), exist_ok=True)
    os.makedirs(os.path.join(resource_pack_dir, "assets", "minecraft", "textures", "item"), exist_ok=True)

    # Initialize paper.json if not present
    paper_json_path = os.path.join(resource_pack_dir, "assets", "minecraft", "models", "item", "paper.json")
    if not os.path.exists(paper_json_path):
        with open(paper_json_path, 'w') as f:
            json.dump({
                "parent": "item/generated",
                "textures": {"layer0": "item/paper"},
                "overrides": []
            }, f)

def add_model_and_texture(model_path, texture_path):
    global custom_model_data_counter

    # Process and add to resource pack
    model_name = f"custom_{custom_model_data_counter}.json"
    texture_name = f"custom_{custom_model_data_counter}.png"

    # Adjust the JSON model to use the uploaded texture
    with open(model_path, 'r') as model_file:
        model_json = json.load(model_file)
    first_texture_key = list(model_json["textures"].keys())[0]
    model_json["textures"][first_texture_key] = f"item/{texture_name.replace('.png', '')}"

    # Extract the model's name
    model_display_name = os.path.basename(model_path).replace('.json', '')

    with open(os.path.join(resource_pack_dir, "assets", "minecraft", "models", "item", model_name), 'w') as model_file:
        json.dump(model_json, model_file)

    # Copy the texture to the desired location
    shutil.copy(texture_path, os.path.join(resource_pack_dir, "assets", "minecraft", "textures", "item", texture_name))
    
    uploaded_files.append((model_name, texture_name, model_display_name, custom_model_data_counter))

    add_model_to_paper_json(model_name, custom_model_data_counter)

    custom_model_data_counter += 1

def add_model_to_paper_json(generated_model_name, cmd_value):
    paper_json_path = os.path.join(resource_pack_dir, "assets", "minecraft", "models", "item", "paper.json")
    
    # Load existing paper.json or create a new one if it doesn't exist
    if os.path.exists(paper_json_path):
        with open(paper_json_path, 'r') as f:
            paper_json = json.load(f)
    else:
        paper_json = {
            "parent": "item/generated",
            "textures": {
                "layer0": "item/paper"
            },
            "overrides": []
        }
    
    model_path = f"item/{generated_model_name.replace('.json', '')}"
    
    # Check if this model override already exists
    existing_override = next((o for o in paper_json["overrides"] if o["model"] == model_path), None)
    if not existing_override:
        override = {
            "predicate": {"custom_model_data": cmd_value},
            "model": model_path
        }
        paper_json["overrides"].append(override)

    # Save back the paper.json using custom formatting for overrides
    with open(paper_json_path, 'w') as f:
        f.write('{\n    "parent": "item/generated",\n    "textures": {\n        "layer0": "item/paper"\n    },\n    "overrides": [\n')
        for i, o in enumerate(paper_json["overrides"]):
            if i < len(paper_json["overrides"]) - 1:
                f.write(f'        {json.dumps(o)},\n')
            else:
                f.write(f'        {json.dumps(o)}\n')
        f.write('    ]\n}')

def compile_pack(pack_name, version):
    global desc_entry, color_var
    # Create pack.mcmeta based on the selected version
    pack_format = PACK_FORMAT_MAP.get(version, 5)  # Default to 5 if version isn't recognize
    description = desc_entry.get()
    selected_color = COLOR_CODE_MAP[color_var.get()]
    colored_description = selected_color + description
    pack_mcmeta = {
        "pack": {
            "pack_format": pack_format,
            "description": colored_description
        }
    }
    with open(os.path.join(resource_pack_dir, "pack.mcmeta"), 'w') as f:
        json.dump(pack_mcmeta, f)

    formatted_date = date.today().strftime('%Y%m%d')  # Format: YYYYMMDD
    zip_path = f"{pack_name}_{version}_{formatted_date}.zip"
    with zipfile.ZipFile(zip_path, 'w') as zipf:
        for root, _, files in os.walk(resource_pack_dir):
            for file in files:
                if not file.endswith('.py'):  # Exclude any .py files
                    zipf.write(os.path.join(root, file), os.path.relpath(os.path.join(root, file), resource_pack_dir))
    messagebox.showinfo("Success", f"Resource pack compiled as {zip_path}")

def update_custom_model_data_counter(entry_widget):
    global custom_model_data_counter
    try:
        new_value = int(entry_widget.get())
        custom_model_data_counter = new_value
        messagebox.showinfo("Success", f"Custom Model Data counter updated to {new_value}")
    except ValueError:
        messagebox.showerror("Error", "Please enter a valid integer for Custom Model Data counter")

def remove_selected_file(lstbox):
    selected = lstbox.curselection()
    if not selected:
        return
    index = selected[0]
    model_name, texture_name, _, cmd_value = uploaded_files[index]

    # Remove from the filesystem
    os.remove(os.path.join(resource_pack_dir, "assets", "minecraft", "models", "item", model_name))
    os.remove(os.path.join(resource_pack_dir, "assets", "minecraft", "textures", "item", texture_name))

    # Remove from the paper.json
    paper_json_path = os.path.join(resource_pack_dir, "assets", "minecraft", "models", "item", "paper.json")
    with open(paper_json_path, 'r') as f:
        paper_json = json.load(f)
    paper_json["overrides"] = [o for o in paper_json["overrides"] if o["predicate"]["custom_model_data"] != cmd_value]
    
    # Save back the paper.json using custom formatting for overrides
    with open(paper_json_path, 'w') as f:
        f.write('{\n    "parent": "item/generated",\n    "textures": {\n        "layer0": "item/paper"\n    },\n    "overrides": [\n')
        for i, o in enumerate(paper_json["overrides"]):
            if i < len(paper_json["overrides"]) - 1:
                f.write(f'        {json.dumps(o)},\n')
            else:
                f.write(f'        {json.dumps(o)}\n')
        f.write('    ]\n}')

    # Remove from the list
    lstbox.delete(index)
    uploaded_files.pop(index)

def update_uploaded_files(lstbox):
    lstbox.delete(0, tk.END)
    for model_file, texture_file, model_display_name, cmd in uploaded_files:
        lstbox.insert(tk.END, f"Model: {model_display_name} (CMD: {cmd}), Texture: {texture_file}")

def upload_files(lstbox):
    model_path = filedialog.askopenfilename(title="Select the JSON model", filetypes=[("JSON files", "*.json")])
    if not model_path:
        return

    texture_path = filedialog.askopenfilename(title="Select the texture", filetypes=[("PNG files", "*.png")])
    if not texture_path:
        return

    add_model_and_texture(model_path, texture_path)
    update_uploaded_files(lstbox)
    messagebox.showinfo("Success", "Model and Texture added to the pack. You can add more or compile the pack.")


def apply_color_to_description(color_code, color_name):
    # Get the current cursor position
    cursor_position = desc_entry.index(tk.INSERT)
    
    # Insert the chosen color at the cursor position
    desc_entry.insert(cursor_position, color_code)

def apply_style_to_description(style_code, style_name):
    # Get the current cursor position
    cursor_position = desc_entry.index(tk.INSERT)
    
    # Insert the chosen style at the cursor position
    desc_entry.insert(cursor_position, style_code)


def show_description_popup():
    global popup
    global display_label  # <-- Add this line    
    try:
        # Destroy the previous popup if it exists
        popup.destroy()
    except:
        pass

    # Create a new Toplevel window to show the description
    popup = tk.Toplevel()
    popup.title("Description Preview")
    popup.configure(bg='#2c2c2c')

    description = desc_entry.get()
    
    # List to store styled text fragments
    styled_fragments = []
    
    # Start processing description
    current_color = "#FFFFFF"
    current_style = "normal"
    current_text = ""

    i = 0
    while i < len(description):
        if description[i] == "§":
            # If there's any text accumulated, append it to the styled_fragments list
            if current_text:
                styled_fragments.append((current_text, current_color, current_style))
                current_text = ""

            code = description[i+1]
            
            # Check if it's a color code
            if code in [c[-1] for c in COLOR_CODE_MAP.values()]:
                current_color = [color for color, color_code in COLOR_CODE_MAP.items() if color_code == f"§{code}"][0]
            # Check if it's a style code
            elif code == "l":
                current_style = "bold"
            elif code == "o":
                current_style = "italic"
            # Reset to defaults for "r" code
            elif code == "r":
                current_color = "#FFFFFF"
                current_style = "normal"
            # Extend for more styles as needed
            i += 2
        else:
            current_text += description[i]
            i += 1
            
    # If there's any remaining text, append it to the styled_fragments list
    if current_text:
        styled_fragments.append((current_text, current_color, current_style))
        
    # Display label for the description
    display_label = tk.Frame(popup, bg='#2c2c2c')
    display_label.pack(pady=20)

    # Add the styled fragments to the display_label frame
    for text, color, style in styled_fragments:
        canvas = tk.Canvas(display_label, bg='#2c2c2c', bd=0, highlightthickness=0)
        font_tuple = ("Helvetica", 12, style)
        text_id = canvas.create_text(5, 5, text=text, anchor=tk.NW, fill=color, font=font_tuple)  # added an offset of 5 pixels in x and y
        (x0, y0, x1, y1) = canvas.bbox(text_id)
        canvas.configure(width=x1-x0+10, height=y1-y0+10)  # adjusted the width and height of the canvas by adding 10 pixels
        canvas.pack(side=tk.LEFT, padx=(0, 5))

def upload_pack_icon():
    """Handles the upload of the pack icon."""
    file_path = filedialog.askopenfilename(title="Select an Image", filetypes=[("Image files", "*.png;*.jpg;*.jpeg;*.gif;*.bmp")])
    if not file_path:
        return

    # Open the image using PIL
    with Image.open(file_path) as img:
        # Resize the image to 256x256
        img_resized = img.resize((256, 256))
        
        # Save the image as pack.png in the resource pack directory
        img_resized.save(os.path.join(resource_pack_dir, 'pack.png'), 'PNG')

    messagebox.showinfo("Success", "Pack icon uploaded and resized successfully!")
    
def load_resource_pack(lstbox):
    """
    Load a resource pack, unzip its contents, and display the contents of the items folder.
    """
    global resource_pack_dir

    # Ask the user to select a resource pack
    file_path = filedialog.askopenfilename(title="Select a Resource Pack", filetypes=[("Resource Packs", "*.zip")])
    if not file_path:
        return

    # Create a temporary directory to extract the resource pack
    temp_dir = os.path.join(resource_pack_dir, "temp_pack")
    os.makedirs(temp_dir, exist_ok=True)

    # Extract the resource pack to the temporary directory
    with zipfile.ZipFile(file_path, 'r') as zip_ref:
        zip_ref.extractall(temp_dir)

    # Update the resource pack directory to the new path
    resource_pack_dir = temp_dir

    # Load existing models from the paper.json
    load_existing_models_from_paper_json(lstbox)
 
def add_model_to_paper_json(generated_model_name, cmd_value):
    paper_json_path = os.path.join(resource_pack_dir, "assets", "minecraft", "models", "item", "paper.json")
    
    # Load existing paper.json or create a new one if it doesn't exist
    if os.path.exists(paper_json_path):
        with open(paper_json_path, 'r') as f:
            paper_json = json.load(f)
    else:
        paper_json = {
            "parent": "item/generated",
            "textures": {
                "layer0": "item/paper"
            },
            "overrides": []
        }
    
    model_path = f"item/{generated_model_name.replace('.json', '')}"
    
    # Check if this model override already exists
    existing_override = next((o for o in paper_json["overrides"] if o["model"] == model_path), None)
    
    # If the model exists, update its custom_model_data
    if existing_override:
        existing_override["predicate"]["custom_model_data"] = cmd_value
    else:
        override = {
            "predicate": {"custom_model_data": cmd_value},
            "model": model_path
        }
        paper_json["overrides"].append(override)

    save_paper_json(paper_json)



def setup_all_items():
    """Sets up all items with custom model data in paper.json."""

    paper_json_path = os.path.join(resource_pack_dir, "assets", "minecraft", "models", "item", "paper.json")
    
    # Load existing paper.json or create a new one if it doesn't exist
    if os.path.exists(paper_json_path):
        with open(paper_json_path, 'r') as f:
            paper_json = json.load(f)
    else:
        paper_json = {
            "parent": "item/generated",
            "textures": {
                "layer0": "item/paper"
            },
            "overrides": []
        }

    # Extract all existing CMD values
    existing_cmds = [o["predicate"]["custom_model_data"] for o in paper_json["overrides"]]

    # Get all item models
    item_folder = os.path.join(resource_pack_dir, "assets", "minecraft", "models", "item")
    item_files = [f for f in os.listdir(item_folder) if f.endswith('.json') and f != 'paper.json']

    global custom_model_data_counter

    # Set up CMD for each item
    for item in item_files:
        model_path = f"item/{item.replace('.json', '')}"
        
        # Check if this model override already exists
        existing_override = next((o for o in paper_json["overrides"] if o["model"] == model_path), None)
        
        # If the model exists, update its custom_model_data
        if existing_override:
            # Ensure the CMD doesn't clash with existing values
            while custom_model_data_counter in existing_cmds:
                custom_model_data_counter += 1
            existing_override["predicate"]["custom_model_data"] = custom_model_data_counter
        else:
            # Ensure the CMD doesn't clash with existing values
            while custom_model_data_counter in existing_cmds:
                custom_model_data_counter += 1
            override = {
                "predicate": {"custom_model_data": custom_model_data_counter},
                "model": model_path
            }
            paper_json["overrides"].append(override)

        # Increment the counter for the next item
        custom_model_data_counter += 1  

        # Update the GUI listbox
        update_uploaded_files(lstbox)

    # Save back the paper.json
    save_paper_json(paper_json)
    # Refresh the Listbox with the updated items
    update_uploaded_files(lstbox)

    messagebox.showinfo("Success", "All items have been set up in paper.json with custom model data.")
    
    

def load_existing_models_from_paper_json(lstbox):
    """
    Load existing models from the paper.json and populate the uploaded_files list with them.
    """
    global custom_model_data_counter

    paper_json_path = os.path.join(resource_pack_dir, "assets", "minecraft", "models", "item", "paper.json")
    
    # Check if the paper.json exists
    if os.path.exists(paper_json_path):
        with open(paper_json_path, 'r') as f:
            paper_json = json.load(f)
        
        # Loop through the overrides to find the existing CMD values
        for override in paper_json.get("overrides", []):
            cmd = override["predicate"]["custom_model_data"]
            model_name = os.path.basename(override["model"]) + ".json"
            texture_name = model_name.replace('.json', '.png')
            model_display_name = model_name.replace('.json', '')
            
            # Populate the uploaded_files list with the existing models
            uploaded_files.append((model_name, texture_name, model_display_name, cmd))
            
            # Update the custom_model_data_counter to the highest CMD value found
            custom_model_data_counter = max(custom_model_data_counter, cmd + 1)
        
        # Update the GUI listbox
        update_uploaded_files(lstbox)

def save_paper_json(paper_json):
    """
    Saves the provided paper_json dictionary to the paper.json file.
    """
    paper_json_path = os.path.join(resource_pack_dir, "assets", "minecraft", "models", "item", "paper.json")
    # Save back the paper.json using custom formatting for overrides
    with open(paper_json_path, 'w') as f:
        f.write('{\n    "parent": "item/generated",\n    "textures": {\n        "layer0": "item/paper"\n    },\n    "overrides": [\n')
        for i, o in enumerate(paper_json["overrides"]):
            if i < len(paper_json["overrides"]) - 1:
                f.write(f'        {json.dumps(o)},\n')
            else:
                f.write(f'        {json.dumps(o)}\n')
        f.write('    ]\n}')
        
def initialize_resource_pack():
    # Only create directories if they don't already exist
    if not os.path.exists(os.path.join(resource_pack_dir, "assets", "minecraft", "models", "item")):
        os.makedirs(os.path.join(resource_pack_dir, "assets", "minecraft", "models", "item"), exist_ok=True)
    if not os.path.exists(os.path.join(resource_pack_dir, "assets", "minecraft", "textures", "item")):
        os.makedirs(os.path.join(resource_pack_dir, "assets", "minecraft", "textures", "item"), exist_ok=True)

    # Initialize paper.json if not present
    paper_json_path = os.path.join(resource_pack_dir, "assets", "minecraft", "models", "item", "paper.json")
    if not os.path.exists(paper_json_path):
        with open(paper_json_path, 'w') as f:
            json.dump({
                "parent": "item/generated",
                "textures": {"layer0": "item/paper"},
                "overrides": []
            }, f)

 
def main():
    global desc_entry, display_label, color_var, popup, lstbox


    initialize_resource_pack()

    root = tk.Tk()
    root.title("[DCU-MMU] RESOURCEPACK GENERATOR")
    root.grid_columnconfigure(0, minsize=300)  # Adjust 300 to a value that suits your needs

    color_var = tk.StringVar(value="#FFFFFF")  # Initialize with a default value after creating the root window
   
    
    # Styling
    root.configure(bg='#2c2c2c')
    
    default_font = ("Helvetica", 12)
    
    # Resource Pack Name
    pack_name_label = Label(root, text="Resource Pack Name:", font=default_font, bg='#2c2c2c', fg='#ffffff')
    pack_name_label.grid(row=0, column=0, padx=20, pady=10, sticky='w')
    pack_name_entry = Entry(root, font=default_font, width=40)
    pack_name_entry.grid(row=1, column=0, padx=20, pady=10, sticky='w')
    
    # Version Dropdown
    version_label = Label(root, text="Minecraft Version:", font=default_font, bg='#2c2c2c', fg='#ffffff')
    version_label.grid(row=2, column=0, padx=20, pady=10, sticky='w')
    version_var = StringVar(root)
    versions = list(PACK_FORMAT_MAP.keys())
    version_dropdown = ttk.Combobox(root, textvariable=version_var, values=versions, font=default_font)
    version_dropdown.set(versions[0])  # Set the default version
    version_dropdown.grid(row=3, column=0, padx=20, pady=10, sticky='w')

    # Resource Pack Description
    desc_label = Label(root, text="Resource Pack Description:", font=default_font, bg='#2c2c2c', fg='#ffffff')
    desc_label.grid(row=9, column=0, padx=20, pady=10, sticky='w')

    # Frame to hold the description entry and "View" button
    desc_frame = tk.Frame(root, bg='#2c2c2c')
    desc_frame.grid(row=10, column=0, padx=20, pady=10, sticky='w')

    desc_entry = Entry(desc_frame, font=default_font, width=40)
    desc_entry.pack(side=tk.LEFT)

    # Initialize the symbol box
    SymbolBox(root)

    # Adding "View" button next to description entry
    view_btn = tk.Button(desc_frame, text="View", font=default_font, bg='#0077b6', fg='white', command=show_description_popup)
    view_btn.pack(side=tk.LEFT, padx=(10, 0))  # Adjust padx to your preference

    # Colour Buttons for Description
    color_frame = tk.Frame(root, bg='#2c2c2c')
    color_frame.grid(row=11, column=0, padx=20, pady=10, sticky='w')
    for color_name, color_code in COLOR_CODE_MAP.items():
        btn = Button(color_frame, bg=color_name, width=2, height=1, command=partial(apply_color_to_description, color_code, COLOR_NAME_MAP[color_name]))
        btn.pack(side=tk.LEFT, padx=5)
        tooltip = ToolTip(btn, COLOR_NAME_MAP[color_name])
        btn.bind("<Enter>", lambda event, t=tooltip: t.show_tip())
        btn.bind("<Leave>", lambda event, t=tooltip: t.hide_tip())

    # Style Buttons
    style_frame = tk.Frame(root, bg='#2c2c2c')
    style_frame.grid(row=12, column=0, padx=20, pady=5, sticky='w')
    for style, code in STYLE_CODE_MAP.items():
        btn = Button(style_frame, text=style.capitalize(), command=partial(apply_style_to_description, code, style.capitalize()))
        btn.pack(side=tk.LEFT, padx=5)


    # Custom Model Data Counter
    cmd_label = Label(root, text="Custom Model Data Start:", font=default_font, bg='#2c2c2c', fg='#ffffff')
    cmd_label.grid(row=4, column=0, padx=20, pady=10, sticky='w')
    cmd_entry = Entry(root, font=default_font, width=40)
    cmd_entry.grid(row=5, column=0, padx=20, pady=10, sticky='w')
    cmd_entry.insert(0, str(custom_model_data_counter))  # set the default value
    cmd_set_btn = tk.Button(root, text="Set CMD Start", font=default_font, bg='#CB0EB7', fg='white', command=lambda: update_custom_model_data_counter(cmd_entry))
    cmd_set_btn.grid(row=6, column=0, padx=20, pady=10, sticky='w')

    upload_btn = tk.Button(root, text="Upload Model & Texture", font=default_font, bg='#0077b6', fg='white', command=lambda: upload_files(lstbox))
    upload_btn.grid(row=7, column=0, padx=20, pady=10, sticky='w')


    lstbox = Listbox(root, width=50, height=20, bg="#121212", fg="#a2d2ff", font=default_font)
    lstbox.grid(row=0, column=1, rowspan=9, padx=20, pady=20)

    rm_btn = Button(root, text="Remove Selected", font=default_font, bg='#e63946', fg='white', command=lambda: remove_selected_file(lstbox))
    rm_btn.grid(row=9, column=1, padx=20, pady=10)

    # Moved the compile_btn below the rm_btn and changed its color to green
    compile_btn = tk.Button(root, text="Compile Resource Pack", font=default_font, bg='#00b894', fg='white', command=lambda: compile_pack(pack_name_entry.get(), version_var.get()))
    compile_btn.grid(row=10, column=1, padx=20, pady=10)

    # Add the new button to the GUI
    icon_btn = tk.Button(root, text="Upload Pack Icon", font=default_font, bg='#0077b6', fg='white', command=upload_pack_icon)
    icon_btn.grid(row=7, column=0, padx=10, pady=10)

    # Add a button to the GUI for loading a resource pack
    load_btn = tk.Button(root, text="Load Resource Pack", font=default_font, bg='#0077b6', fg='white', command=lambda: load_resource_pack(lstbox))
    load_btn.grid(row=8, column=0, padx=20, pady=10, sticky='w')

    setup_btn = tk.Button(root, text="Setup All Items", font=default_font, bg='#d00000', fg='white', command=setup_all_items)
    setup_btn.grid(row=8, column=0, padx=10, pady=0)


    root.mainloop()
    
if __name__ == "__main__":
    main()
