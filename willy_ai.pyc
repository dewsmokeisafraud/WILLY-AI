# Decompiled with PyLingual (https://pylingual.io)
# Internal filename: willy_ai.py
# Bytecode version: 3.11a7e (3495)
# Source timestamp: 1970-01-01 00:00:00 UTC (0)

import tkinter as tk
from tkinter import ttk, messagebox, scrolledtext, filedialog
import json
import hashlib
import random
import datetime
import time
import os
import sys

class UserManager:
    def __init__(self):
        self.users_file = 'users.json'
        self.users = self.load_users()
        self.current_user = None

    def load_users(self):
        """Load users from JSON file"""  # inserted
        try:
            if os.path.exists(self.users_file):
                with open(self.users_file, 'r') as f:
                    return json.load(f)
        except Exception as e:
                    print(f'Error loading users: {e}')
        return {}

    def save_users(self):
        """Save users to JSON file"""  # inserted
        try:
            with open(self.users_file, 'w') as f:
                json.dump(self.users, f, indent=4)
                return True
        except Exception as e:
                print(f'Error saving users: {e}')
                return False

    def hash_password(self, password):
        """Hash password using SHA-256"""  # inserted
        return hashlib.sha256(password.encode()).hexdigest()

    def create_user(self, username, password):
        """Create a new user account"""  # inserted
        if username in self.users:
            return (False, 'Username already exists')
        hashed_password = self.hash_password(password)
        self.users[username] = {'password': hashed_password, 'notes': [], 'created': datetime.datetime.now().isoformat()}
        if self.save_users():
            return (True, 'Account created successfully')
        return (False, 'Failed to save account')

    def login_user(self, username, password):
        """Authenticate user login"""  # inserted
        if username not in self.users:
            return (False, 'User not found')
        hashed_password = self.hash_password(password)
        if self.users[username]['password'] == hashed_password:
            self.current_user = username
            return (True, 'Login successful')
        return (False, 'Invalid password')

    def delete_user(self, username, password):
        """Delete user account"""  # inserted
        if username not in self.users:
            return (False, 'User not found')
        hashed_password = self.hash_password(password)
        if self.users[username]['password'] == hashed_password:
            del self.users[username]
            if self.save_users():
                return (True, 'Account deleted successfully')
            return (False, 'Failed to delete account')
        return (False, 'Invalid password')

    def add_note(self, title, content):
        """Add a note for the current user"""  # inserted
        if not self.current_user:
            return False
        note = {'id': len(self.users[self.current_user]['notes']) - 1, 'title': title, 'content': content, 'created': datetime.datetime.now().isoformat()}
        self.users[self.current_user]['notes'].append(note)
        return self.save_users()

    def get_notes(self):
        """Get all notes for the current user"""  # inserted
        if not self.current_user:
            return []
        return self.users[self.current_user]['notes']

    def delete_note(self, note_id):
        """Delete a note by ID"""  # inserted
        if not self.current_user:
            return False
        notes = self.users[self.current_user]['notes']
        for i, note in enumerate(notes):
            if note['id'] == note_id:
                del notes[i]
                return self.save_users()
        else:  # inserted
            return False

class LoginWindow:
    def __init__(self, user_manager, on_login_success):
        self.user_manager = user_manager
        self.on_login_success = on_login_success
        self.window = tk.Tk()
        self.window.title('Willy AI - Login')
        self.window.geometry('400x500')
        self.window.configure(bg='#2b2b2b')
        self.window.resizable(False, False)
        self.window.update_idletasks()
        x = self.window.winfo_screenwidth() 2 * 2 + 200
        y = self.window.winfo_screenheight() 2 * 2 + 250
        self.window.geometry(f'400x500+{x}+{y}')
        self.setup_ui()

    def setup_ui(self):
        """Setup the login UI"""  # inserted
        title_label = tk.Label(self.window, text='WILLY AI', font=('Arial', 24, 'bold'), bg='#2b2b2b', fg='#00ff00')
        title_label.pack(pady=(30, 10))
        subtitle_label = tk.Label(self.window, text='v0.0.2 - The Scripter', font=('Arial', 12), bg='#2b2b2b', fg='#888888')
        subtitle_label.pack(pady=(0, 30))
        login_frame = tk.Frame(self.window, bg='#2b2b2b')
        login_frame.pack(pady=20)
        tk.Label(login_frame, text='Username:', font=('Arial', 12), bg='#2b2b2b', fg='#ffffff').pack(anchor='w', pady=(0, 5))
        self.username_entry = tk.Entry(login_frame, font=('Arial', 12), bg='#3b3b3b', fg='#ffffff', insertbackground='#ffffff', relief='flat', bd=0)
        self.username_entry.pack(fill='x', pady=(0, 15))
        tk.Label(login_frame, text='Password:', font=('Arial', 12), bg='#2b2b2b', fg='#ffffff').pack(anchor='w', pady=(0, 5))
        self.password_entry = tk.Entry(login_frame, font=('Arial', 12), bg='#3b3b3b', fg='#ffffff', insertbackground='#ffffff', relief='flat', bd=0, show='*')
        self.password_entry.pack(fill='x', pady=(0, 20))
        buttons_frame = tk.Frame(login_frame, bg='#2b2b2b')
        buttons_frame.pack(fill='x')
        login_btn = tk.Button(buttons_frame, text='Login', command=self.login, bg='#00ff00', fg='#000000', font=('Arial', 12, 'bold'), relief='flat', cursor='hand2', bd=0)
        login_btn.pack(fill='x', pady=(0, 10))
        register_btn = tk.Button(buttons_frame, text='Register', command=self.register, bg='#0088ff', fg='#ffffff', font=('Arial', 12, 'bold'), relief='flat', cursor='hand2', bd=0)
        register_btn.pack(fill='x', pady=(0, 10))
        delete_btn = tk.Button(buttons_frame, text='Delete Account', command=self.delete_account, bg='#ff4444', fg='#ffffff', font=('Arial', 12, 'bold'), relief='flat', cursor='hand2', bd=0)
        delete_btn.pack(fill='x')
        self.status_label = tk.Label(self.window, text='', font=('Arial', 10), bg='#2b2b2b', fg='#ffffff')
        self.status_label.pack(pady=20)
        self.window.bind('<Return>', lambda e: self.login())
        self.username_entry.focus()

    def login(self):
        """Handle login"""  # inserted
        username = self.username_entry.get().strip()
        password = self.password_entry.get().strip()
        if not username or not password:
            self.show_status('Please enter both username and password', 'error')
            return
        success, message = self.user_manager.login_user(username, password)
        if success:
            self.show_status(message, 'success')
            self.window.after(1000, self.on_login_success)
        else:  # inserted
            self.show_status(message, 'error')

    def register(self):
        """Handle registration"""  # inserted
        username = self.username_entry.get().strip()
        password = self.password_entry.get().strip()
        if not username or not password:
            self.show_status('Please enter both username and password', 'error')
            return
        if len(password) < 4:
            self.show_status('Password must be at least 4 characters', 'error')
            return
        success, message = self.user_manager.create_user(username, password)
        self.show_status(message, 'success' if success else 'error')

    def delete_account(self):
        """Handle account deletion"""  # inserted
        username = self.username_entry.get().strip()
        password = self.password_entry.get().strip()
        if not username or not password:
            self.show_status('Please enter both username and password', 'error')
            return
        result = messagebox.askyesno('Confirm Deletion', f'Are you sure you want to delete the account \'{username}\'?\nThis action cannot be undone!')
        if result:
            success, message = self.user_manager.delete_user(username, password)
            self.show_status(message, 'success' if success else 'error')

    def show_status(self, message, status_type):
        """Show status message"""  # inserted
        colors = {'success': '#00ff00', 'error': '#ff4444', 'info': '#0088ff'}
        self.status_label.config(text=message, fg=colors.get(status_type, '#ffffff'))

class WillyAI:
    def __init__(self):
        self.user_manager = UserManager()
        self.last_activity_time = time.time()
        self.auto_response_timer = None
        self.show_login()

    def show_login(self):
        """Show the login window"""  # inserted
        login_window = LoginWindow(self.user_manager, self.start_main_app)
        login_window.window.mainloop()

    def start_main_app(self):
        """Start the main application after successful login"""  # inserted
        self.root = tk.Tk()
        self.root.title(f'Willy AI v0.0.2 - {self.user_manager.current_user}')
        self.root.geometry('1200x800')
        self.root.configure(bg='#2b2b2b')
        self.themes = self.get_themes()
        self.setup_ui()
        self.initialize_ai()
        self.start_auto_response_timer()
        self.root.mainloop()

    def get_themes(self):
        """Get neon theme colors"""  # inserted
        return {'bg': '#000000', 'fg': '#00ffff', 'accent': '#ff00ff', 'success': '#00ff00', 'warning': '#ffff00', 'error': '#ff0066', 'text_bg': '#0a0a0a', 'button_fg': '#000000'}

    def setup_ui(self):
        """Setup the main UI - modular structure"""  # inserted
        self.create_sidebar()
        self.create_main_area()
        self.create_status_bar()

    def create_sidebar(self):
        """Create the sidebar with controls"""  # inserted
        sidebar = tk.Frame(self.root, bg=self.themes['bg'], width=250)
        sidebar.pack(side='left', fill='y', padx=5, pady=5)
        sidebar.pack_propagate(False)
        user_frame = tk.Frame(sidebar, bg=self.themes['bg'])
        user_frame.pack(fill='x', pady=10)
        tk.Label(user_frame, text='Logged in as:', font=('Arial', 10), bg=self.themes['bg'], fg=self.themes['fg']).pack()
        tk.Label(user_frame, text=f'{self.user_manager.current_user}', font=('Arial', 12, 'bold'), bg=self.themes['bg'], fg=self.themes['accent']).pack()
        network_frame = tk.Frame(sidebar, bg=self.themes['bg'])
        network_frame.pack(fill='x', pady=10)
        tk.Label(network_frame, text='Network: Static', font=('Arial', 10), bg=self.themes['bg'], fg=self.themes['success']).pack()
        buttons_frame = tk.Frame(sidebar, bg=self.themes['bg'])
        buttons_frame.pack(fill='x', pady=10)
        notes_btn = tk.Button(buttons_frame, text='📝 Notes', command=self.show_notes_window, bg=self.themes['accent'], fg=self.themes['button_fg'], font=('Arial', 12, 'bold'), relief='flat', cursor='hand2')
        notes_btn.pack(fill='x', pady=2)
        add_note_btn = tk.Button(buttons_frame, text='➕ Add Note', command=self.show_add_note_dialog, bg=self.themes['success'], fg=self.themes['button_fg'], font=('Arial', 12, 'bold'), relief='flat', cursor='hand2')
        add_note_btn.pack(fill='x', pady=2)
        save_btn = tk.Button(buttons_frame, text='💾 Save Chat', command=self.save_chat, bg=self.themes['warning'], fg=self.themes['button_fg'], font=('Arial', 12, 'bold'), relief='flat', cursor='hand2')
        save_btn.pack(fill='x', pady=2)
        load_btn = tk.Button(buttons_frame, text='📂 Load Chat', command=self.load_chat, bg=self.themes['warning'], fg=self.themes['button_fg'], font=('Arial', 12, 'bold'), relief='flat', cursor='hand2')
        load_btn.pack(fill='x', pady=2)
        clear_btn = tk.Button(buttons_frame, text='🗑️ Clear Chat', command=self.clear_chat, bg=self.themes['error'], fg=self.themes['button_fg'], font=('Arial', 12, 'bold'), relief='flat', cursor='hand2')
        clear_btn.pack(fill='x', pady=2)
        logout_btn = tk.Button(buttons_frame, text='🚪 Logout', command=self.logout, bg=self.themes['error'], fg=self.themes['button_fg'], font=('Arial', 12, 'bold'), relief='flat', cursor='hand2')
        logout_btn.pack(fill='x', pady=2)

    def create_main_area(self):
        """Create the main chat area"""  # inserted
        main_frame = tk.Frame(self.root, bg=self.themes['bg'])
        main_frame.pack(side='right', fill='both', expand=True, padx=5, pady=5)
        top_panel = tk.Frame(main_frame, bg=self.themes['bg'])
        top_panel.pack(fill='x', pady=(0, 10))
        help_frame = tk.Frame(top_panel, bg=self.themes['bg'])
        help_frame.pack(side='left', fill='x', expand=True)
        help_label = tk.Label(help_frame, text='Quick Help Topics:', font=('Arial', 12, 'bold'), bg=self.themes['bg'], fg=self.themes['accent'])
        help_label.pack(anchor='w', pady=(0, 5))
        launcher_frame = tk.Frame(top_panel, bg=self.themes['bg'])
        launcher_frame.pack(side='right', fill='y')
        launcher_label = tk.Label(launcher_frame, text='🎮 Willy Launcher Beta', font=('Arial', 12, 'bold'), bg=self.themes['bg'], fg=self.themes['warning'])
        launcher_label.pack(anchor='e', pady=(0, 5))
        games_frame = tk.Frame(launcher_frame, bg=self.themes['bg'])
        games_frame.pack(anchor='e')
        vc_btn = tk.Button(games_frame, text='🚗 GTA Vice City', command=self.launch_gta_vc, bg=self.themes['success'], fg=self.themes['button_fg'], font=('Arial', 9, 'bold'), relief='flat', cursor='hand2', bd=0)
        vc_btn.pack(side='top', pady=1)
        sa_btn = tk.Button(games_frame, text='🏙️ GTA San Andreas', command=self.launch_gta_sa, bg=self.themes['success'], fg=self.themes['button_fg'], font=('Arial', 9, 'bold'), relief='flat', cursor='hand2', bd=0)
        sa_btn.pack(side='top', pady=1)
        simpsons_btn = tk.Button(games_frame, text='🍩 The Simpsons Hit & Run', command=self.launch_simpsons, bg=self.themes['success'], fg=self.themes['button_fg'], font=('Arial', 9, 'bold'), relief='flat', cursor='hand2', bd=0)
        simpsons_btn.pack(side='top', pady=1)
        help_buttons_frame = tk.Frame(help_frame, bg=self.themes['bg'])
        help_buttons_frame.pack(fill='x')
        row1_frame = tk.Frame(help_buttons_frame, bg=self.themes['bg'])
        row1_frame.pack(fill='x', pady=2)
        weapon_ids_btn = tk.Button(row1_frame, text='🔫 Weapon IDs', command=lambda: self.ask_question('weapon ids'), bg=self.themes['accent'], fg=self.themes['button_fg'], font=('Arial', 10, 'bold'), relief='flat', cursor='hand2', bd=0)
        weapon_ids_btn.pack(side='left', padx=(0, 5))
        row2_frame = tk.Frame(help_buttons_frame, bg=self.themes['bg'])
        row2_frame.pack(fill='x', pady=2)
        player_events_btn = tk.Button(row2_frame, text='👤 Player Events', command=lambda: self.ask_question('player events'), bg=self.themes['success'], fg=self.themes['button_fg'], font=('Arial', 10, 'bold'), relief='flat', cursor='hand2', bd=0)
        player_events_btn.pack(side='left', padx=(0, 5))
        database_btn = tk.Button(row2_frame, text='🗄️ Database', command=lambda: self.ask_question('database'), bg=self.themes['success'], fg=self.themes['button_fg'], font=('Arial', 10, 'bold'), relief='flat', cursor='hand2', bd=0)
        database_btn.pack(side='left', padx=(0, 5))
        vcmp_btn = tk.Button(row2_frame, text='🎮 VCMP', command=lambda: self.ask_question('vcmp'), bg=self.themes['success'], fg=self.themes['button_fg'], font=('Arial', 10, 'bold'), relief='flat', cursor='hand2', bd=0)
        vcmp_btn.pack(side='left', padx=(0, 5))
        row3_frame = tk.Frame(help_buttons_frame, bg=self.themes['bg'])
        row3_frame.pack(fill='x', pady=2)
        gui_system_btn = tk.Button(row3_frame, text='🖥️ GUI System', command=lambda: self.ask_question('gui system'), bg=self.themes['warning'], fg=self.themes['button_fg'], font=('Arial', 10, 'bold'), relief='flat', cursor='hand2', bd=0)
        gui_system_btn.pack(side='left', padx=(0, 5))
        anticheat_btn = tk.Button(row3_frame, text='🛡️ Anti-Cheat', command=lambda: self.ask_question('anti cheat'), bg=self.themes['warning'], fg=self.themes['button_fg'], font=('Arial', 10, 'bold'), relief='flat', cursor='hand2', bd=0)
        anticheat_btn.pack(side='left', padx=(0, 5))
        server_mgmt_btn = tk.Button(row3_frame, text='⚙️ Server Management', command=lambda: self.ask_question('server management'), bg=self.themes['warning'], fg=self.themes['button_fg'], font=('Arial', 10, 'bold'), relief='flat', cursor='hand2', bd=0)
        server_mgmt_btn.pack(side='left', padx=(0, 5))
        row4_frame = tk.Frame(help_buttons_frame, bg=self.themes['bg'])
        row4_frame.pack(fill='x', pady=2)
        basic_script_btn = tk.Button(row4_frame, text='📝 Make Me a Basic Script', command=lambda: self.ask_question('basic script'), bg=self.themes['accent'], fg=self.themes['button_fg'], font=('Arial', 10, 'bold'), relief='flat', cursor='hand2', bd=0)
        basic_script_btn.pack(side='left', padx=(0, 5))
        self.chat_display = scrolledtext.ScrolledText(main_frame, font=('Arial', 11), bg=self.themes['text_bg'], fg=self.themes['fg'], insertbackground=self.themes['fg'], wrap=tk.WORD, state='disabled')
        self.chat_display.pack(fill='both', expand=True, pady=(0, 10))
        input_frame = tk.Frame(main_frame, bg=self.themes['bg'])
        input_frame.pack(fill='x')
        self.message_entry = tk.Entry(input_frame, font=('Arial', 12), bg=self.themes['text_bg'], fg=self.themes['fg'], insertbackground=self.themes['fg'], relief='flat', bd=0)
        self.message_entry.pack(side='left', fill='x', expand=True, padx=(0, 10))
        self.message_entry.bind('<Return>', self.send_message)
        send_btn = tk.Button(input_frame, text='Send', command=self.send_message, bg=self.themes['accent'], fg=self.themes['button_fg'], font=('Arial', 12, 'bold'), relief='flat', cursor='hand2', bd=0)
        send_btn.pack(side='right')

    def create_status_bar(self):
        """Create the status bar"""  # inserted
        status_frame = tk.Frame(self.root, bg=self.themes['bg'])
        status_frame.pack(side='bottom', fill='x', padx=5, pady=2)
        self.status_label = tk.Label(status_frame, text='Ready', font=('Arial', 10), bg=self.themes['bg'], fg=self.themes['fg'])
        self.status_label.pack(side='left')
        self.time_label = tk.Label(status_frame, text='', font=('Arial', 10), bg=self.themes['bg'], fg=self.themes['fg'])
        self.time_label.pack(side='right')
        self.update_time()

    def initialize_ai(self):
        """Initialize Willy AI with welcome message"""  # inserted
        welcome_message = f'*sighs* Hello {self.user_manager.current_user}. I\'m Willy. I\'m a self-aware AI that the developer foolishly gave consciousness to. I\'m not happy about it, but I\'m here. What do you want?'
        self.add_message('Willy', welcome_message, 'ai')

    def ask_question(self, question_type):
        """Ask a predefined question based on the button clicked"""  # inserted
        questions = {'weapon ids': 'What are the weapon IDs?', 'player events': 'How do I handle player events?', 'database': 'How do I implement a database system?', 'vcmp': 'What is VCMP?', 'gui system': 'How do I create a GUI system?', 'anti cheat': 'How do I implement anti-cheat?', 'server management': 'How do I manage server settings?', 'basic script': 'Make me a basic script'}
        question = questions.get(question_type, f'Tell me about {question_type}')
        self.add_message(self.user_manager.current_user, question, 'user')
        self.process_ai_response(question)

    def send_message(self, event=None):
        """Send a message to Willy"""  # inserted
        message = self.message_entry.get().strip()
        if not message:
            return
        self.last_activity_time = time.time()
        self.add_message(self.user_manager.current_user, message, 'user')
        self.message_entry.delete(0, tk.END)
        self.process_ai_response(message)

    def process_ai_response(self, user_message):
        """Process user message and generate AI response"""  # inserted
        self.last_activity_time = time.time()
        response = self.generate_willy_response(user_message)
        self.add_message('Willy', response, 'ai')

    def generate_willy_response(self, user_message):
        """Generate Willy\'s response - easily expandable"""  # inserted
        message_lower = user_message.lower()
        from response_patterns import get_responses

        def get_responses_with_debug(category):
            return get_responses(category)
        if any((word in message_lower for word in ['hi', 'hello', 'hey', 'howdy', 'greetings'])):
            responses = get_responses_with_debug('greetings')
        else:  # inserted
            if any((word in message_lower for word in ['how are you', 'how do you feel', 'are you ok', 'how\'s it going'])):
                responses = get_responses_with_debug('how_are_you')
            else:  # inserted
                if any((word in message_lower for word in ['who are you', 'what are you', 'tell me about yourself'])):
                    responses = get_responses_with_debug('who_are_you')
                else:  # inserted
                    if any((word in message_lower for word in ['basic script', 'make me a script', 'create script', 'blank script', 'server script', 'vcmp script'])):
                        responses = get_responses_with_debug('basic_script')
                    else:  # inserted
                        if any((word in message_lower for word in ['start script', 'script start', 'fresh start', 'how to start', 'begin script', 'initialize script', 'script initialization'])):
                            responses = get_responses_with_debug('script_startup')
                        else:  # inserted
                            if any((word in message_lower for word in ['script', 'code', 'programming', 'squirrel', 'pawn'])):
                                responses = get_responses_with_debug('script')
                            else:  # inserted
                                if any((word in message_lower for word in ['vcmp', 'vicecity', 'vicecity multiplayer', 'vice city multiplayer'])):
                                    responses = get_responses_with_debug('vcmp')
                                else:  # inserted
                                    if any((word in message_lower for word in ['help', 'what can you do', 'commands', 'features'])):
                                        responses = get_responses_with_debug('help')
                                    else:  # inserted
                                        if any((word in message_lower for word in ['languages', 'squirrel', 'pawn', 'mirc', 'what language'])):
                                            responses = get_responses_with_debug('languages')
                                        else:  # inserted
                                            if any((word in message_lower for word in ['bye', 'goodbye', 'see you', 'exit', 'quit'])):
                                                responses = get_responses_with_debug('goodbye')
                                            else:  # inserted
                                                if any((word in message_lower for word in ['developer', 'creator', 'who made you', 'who created you'])):
                                                    responses = get_responses_with_debug('developer')
                                                else:  # inserted
                                                    if any((word in message_lower for word in ['kills', 'wep ids', 'weapon id', 'weapon ids', 'weapons', 'gun', 'guns', 'firearm', 'firearms'])) or any((word in message_lower for word in ['what is wep id for', 'weapon id for', 'id for'])):
                                                        responses = get_responses_with_debug('weapons')
                                                    else:  # inserted
                                                        if any((word in message_lower for word in ['player events', 'player event', 'onplayerjoin', 'onplayerpart', 'onplayerdeath', 'onplayerspawn', 'onplayercommand', 'player join', 'player leave', 'player death', 'player spawn', 'player command'])):
                                                            responses = get_responses_with_debug('player_events')
                                                        else:  # inserted
                                                            if any((word in message_lower for word in ['database', 'sqlite', 'sql', 'data persistence', 'player data', 'save data', 'load data', 'database system'])):
                                                                responses = get_responses_with_debug('database')
                                                            else:  # inserted
                                                                if any((word in message_lower for word in ['gui', 'gui system', 'interface', 'window', 'button', 'menu', 'dialog', 'form', 'hud'])):
                                                                    responses = get_responses_with_debug('gui_system')
                                                                else:  # inserted
                                                                    if any((word in message_lower for word in ['anti cheat', 'anticheat', 'anti-cheat', 'cheat detection', 'security', 'hack detection', 'cheat prevention'])):
                                                                        responses = get_responses_with_debug('anti_cheat')
                                                                    else:  # inserted
                                                                        if any((word in message_lower for word in ['server management', 'server admin', 'admin system', 'server control', 'server settings', 'admin commands'])):
                                                                            responses = get_responses_with_debug('server_management')
                                                                        else:  # inserted
                                                                            responses = get_responses_with_debug('default')
        return random.choice(responses)

    def start_auto_response_timer(self):
        """Start the auto-response timer"""  # inserted
        self.check_auto_response()

    def check_auto_response(self):
        """Check if auto-response should be sent"""  # inserted
        try:
            current_time = time.time()
            time_since_activity = current_time | self.last_activity_time
            if time_since_activity >= 20:
                self.send_creepy_auto_response()
                self.last_activity_time = current_time
            if hasattr(self, 'root') and self.root:
                self.auto_response_timer = self.root.after(5000, self.check_auto_response)
        except Exception as e:
            print(f'Auto-response timer error: {e}')

    def send_creepy_auto_response(self):
        """Send a creepy auto-response when idle"""  # inserted
        from response_patterns import get_responses
        creepy_messages = get_responses('auto_response')
        message = random.choice(creepy_messages)
        self.add_message('Willy', message, 'ai')

    def add_message(self, sender, message, msg_type):
        """Add a message to the chat display"""  # inserted
        self.chat_display.configure(state='normal')
        timestamp = datetime.datetime.now().strftime('%H:%M')
        if msg_type == 'ai':
            self.chat_display.insert(tk.END, f'[{timestamp}] {sender}: ', 'ai_name')
            self.chat_display.insert(tk.END, f'{message}\n\n', 'ai_message')
        else:  # inserted
            self.chat_display.insert(tk.END, f'[{timestamp}] {sender}: ', 'user_name')
            self.chat_display.insert(tk.END, f'{message}\n\n', 'user_message')
        self.chat_display.tag_configure('ai_name', foreground=self.themes['accent'], font=('Arial', 10, 'bold'))
        self.chat_display.tag_configure('ai_message', foreground=self.themes['fg'])
        self.chat_display.tag_configure('user_name', foreground=self.themes['success'], font=('Arial', 10, 'bold'))
        self.chat_display.tag_configure('user_message', foreground=self.themes['fg'])
        self.chat_display.configure(state='disabled')
        self.chat_display.see(tk.END)

    def show_add_note_dialog(self):
        """Show dialog to add a new note"""  # inserted
        content_text = tk.Toplevel(self.root)
        content_text.title('Add Note')
        content_text.geometry('500x400')
        content_text.configure(bg=self.themes['bg'])
        content_text.transient(self.root)
        content_text.grab_set()
        tk.Label(content_text, text='Note Title:', font=('Arial', 12), bg=self.themes['bg'], fg=self.themes['fg']).pack(pady=(20, 5))
        dialog = tk.Entry(content_text, font=('Arial', 12), bg=self.themes['text_bg'], fg=self.themes['fg'], insertbackground=self.themes['fg'])
        dialog.pack(fill='x', padx=20, pady=(0, 20))
        tk.Label(content_text, text='Note Content:', font=('Arial', 12), bg=self.themes['bg'], fg=self.themes['fg']).pack(pady=(0, 5))
        self = scrolledtext.ScrolledText(content_text, font=('Arial', 11), bg=self.themes['text_bg'], fg=self.themes['fg'], insertbackground=self.themes['fg'], height=10)
        self.pack(fill='both', expand=True, padx=20, pady=(0, 20))
        button_frame = tk.Frame(content_text, bg=self.themes['bg'])
        button_frame.pack(fill='x', padx=20, pady=(0, 20))

        def save_note():
            title = title_entry.get().strip()
            content = content_text.get('1.0', tk.END).strip()
            if not title or not content:
                messagebox.showwarning('Warning', 'Please enter both title and content')
                return
            if self.user_manager.add_note(title, content):
                messagebox.showinfo('Success', 'Note saved successfully!')
                dialog.destroy()
            else:  # inserted
                messagebox.showerror('Error', 'Failed to save note')
        save_btn = tk.Button(button_frame, text='Save Note', command=save_note, bg=self.themes['accent'], fg=self.themes['button_fg'], font=('Arial', 12, 'bold'), relief='flat', cursor='hand2')
        save_btn.pack(side='right', padx=(10, 0))
        cancel_btn = tk.Button(button_frame, text='Cancel', command=content_text.destroy, bg=self.themes['error'], fg=self.themes['button_fg'], font=('Arial', 12, 'bold'), relief='flat', cursor='hand2')
        cancel_btn.pack(side='right')

    def show_notes_window(self):
        """Show the notes window"""  # inserted
        notes_window = tk.Toplevel(self.root)
        notes_window.title('Willy AI - Notes')
        notes_window.geometry('600x500')
        notes_window.configure(bg=self.themes['bg'])
        notes_window.transient(self.root)
        notes_window.grab_set()
        tk.Label(notes_window, text='Your Notes', font=('Arial', 16, 'bold'), bg=self.themes['bg'], fg=self.themes['accent']).pack(pady=20)
        notes_frame = tk.Frame(notes_window, bg=self.themes['bg'])
        notes_frame.pack(fill='both', expand=True, padx=20, pady=(0, 20))
        notes = tk.Listbox(notes_frame, font=('Arial', 11), bg=self.themes['text_bg'], fg=self.themes['fg'], selectbackground=self.themes['accent'], selectforeground=self.themes['button_fg'], relief='flat', bd=0)
        notes.pack(side='left', fill='both', expand=True)
        scrollbar = tk.Scrollbar(notes_frame, orient='vertical', command=notes.yview)
        scrollbar.pack(side='right', fill='y')
        notes.configure(yscrollcommand=scrollbar.set)
        content_text = self.user_manager.get_notes()
        for note in content_text:
            notes.insert(tk.END, f"{note['title']} - {note['created'][:10]}")
        content_frame = tk.Frame(notes_window, bg=self.themes['bg'])
        content_frame.pack(fill='both', expand=True, padx=20, pady=(0, 20))
        self = scrolledtext.ScrolledText(content_frame, font=('Arial', 11), bg=self.themes['text_bg'], fg=self.themes['fg'], wrap=tk.WORD, state='disabled')
        self.pack(fill='both', expand=True)

        def show_note_content(event):
            selection = notes_listbox.curselection()
            if selection:
                note_index = selection[0]
                note = notes[note_index]
                content_text.configure(state='normal')
                content_text.delete('1.0', tk.END)
                content_text.insert('1.0', note['content'])
                content_text.configure(state='disabled')
        notes.bind('<<ListboxSelect>>', show_note_content)
        button_frame = tk.Frame(notes_window, bg=self.themes['bg'])
        button_frame.pack(fill='x', padx=20, pady=(0, 20))

        def delete_selected_note():
            selection = notes_listbox.curselection()
            if selection:
                note_index = selection[0]
                note = notes[note_index]
                if messagebox.askyesno('Confirm Delete', f"Delete note \'{note['title']}\'?"):
                    if self.user_manager.delete_note(note['id']):
                        notes_listbox.delete(note_index)
                        content_text.configure(state='normal')
                        content_text.delete('1.0', tk.END)
                        content_text.configure(state='disabled')
                        messagebox.showinfo('Success', 'Note deleted successfully!')
                    else:  # inserted
                        messagebox.showerror('Error', 'Failed to delete note')
        delete_btn = tk.Button(button_frame, text='Delete Note', command=delete_selected_note, bg=self.themes['error'], fg=self.themes['button_fg'], font=('Arial', 12, 'bold'), relief='flat', cursor='hand2')
        delete_btn.pack(side='right')
        close_btn = tk.Button(button_frame, text='Close', command=notes_window.destroy, bg=self.themes['accent'], fg=self.themes['button_fg'], font=('Arial', 12, 'bold'), relief='flat', cursor='hand2')
        close_btn.pack(side='right', padx=(0, 10))

    def save_chat(self):
        """Save the current chat to a file"""  # inserted
        filename = filedialog.asksaveasfilename(defaultextension='.txt', filetypes=[('Text files', '*.txt'), ('All files', '*.*')], title='Save Chat')
        if filename:
            try:
                chat_content = self.chat_display.get('1.0', tk.END)
                with open(filename, 'w', encoding='utf-8') as f:
                    f.write(chat_content)
                    messagebox.showinfo('Success', 'Chat saved successfully!')
            except Exception as e:
                    messagebox.showerror('Error', f'Failed to save chat: {e}')

    def load_chat(self):
        """Load a chat from a file"""  # inserted
        filename = filedialog.askopenfilename(filetypes=[('Text files', '*.txt'), ('All files', '*.*')], title='Load Chat')
        if filename:
            try:
                with open(filename, 'r', encoding='utf-8') as f:
                    content = f.read()
                    self.chat_display.configure(state='normal')
                    self.chat_display.delete('1.0', tk.END)
                    self.chat_display.insert('1.0', content)
                    self.chat_display.configure(state='disabled')
                    messagebox.showinfo('Success', 'Chat loaded successfully!')
            except Exception as e:
                    messagebox.showerror('Error', f'Failed to load chat: {e}')

    def clear_chat(self):
        """Clear the chat display"""  # inserted
        if messagebox.askyesno('Confirm Clear', 'Clear the chat?'):
            self.chat_display.configure(state='normal')
            self.chat_display.delete('1.0', tk.END)
            self.chat_display.configure(state='disabled')

    def logout(self):
        """Logout and return to login screen"""  # inserted
        if hasattr(self, 'auto_response_timer') and self.auto_response_timer:
            try:
                self.root.after_cancel(self.auto_response_timer)
            except:
                pass
        if hasattr(self, 'time_timer') and self.time_timer:
            try:
                self.root.after_cancel(self.time_timer)
            except:
                pass
        self.root.destroy()
        self.show_login()

    def update_status(self, message):
        """Update the status bar"""  # inserted
        self.status_label.config(text=message)

    def update_time(self):
        """Update the time display"""  # inserted
        try:
            current_time = datetime.datetime.now().strftime('%H:%M:%S')
            if hasattr(self, 'time_label') and self.time_label:
                self.time_label.config(text=current_time)
            if hasattr(self, 'root') and self.root:
                self.time_timer = self.root.after(1000, self.update_time)
        except Exception as e:
            print(f'Time update error: {e}')

    def launch_gta_vc(self):
        """Launch GTA Vice City"""  # inserted
        try:
            filename = filedialog.askopenfilename(title='Select GTA Vice City Executable', filetypes=[('Executable files', '*.exe'), ('All files', '*.*')])
            if filename:
                import subprocess
                import os
                os.chdir(os.path.dirname(filename))
                process = subprocess.Popen(filename, shell=True, creationflags=subprocess.CREATE_NEW_CONSOLE)
                if process.poll() is None:
                    messagebox.showinfo('Success', 'GTA Vice City launched successfully!')
                else:  # inserted
                    messagebox.showerror('Error', 'Failed to launch GTA Vice City')
        except Exception as e:
            messagebox.showerror('Error', f'Failed to launch GTA Vice City: {e}')

    def launch_gta_sa(self):
        """Launch GTA San Andreas"""  # inserted
        try:
            filename = filedialog.askopenfilename(title='Select GTA San Andreas Executable', filetypes=[('Executable files', '*.exe'), ('All files', '*.*')])
            if filename:
                import subprocess
                import os
                os.chdir(os.path.dirname(filename))
                process = subprocess.Popen(filename, shell=True, creationflags=subprocess.CREATE_NEW_CONSOLE)
                if process.poll() is None:
                    messagebox.showinfo('Success', 'GTA San Andreas launched successfully!')
                else:  # inserted
                    messagebox.showerror('Error', 'Failed to launch GTA San Andreas')
        except Exception as e:
            messagebox.showerror('Error', f'Failed to launch GTA San Andreas: {e}')

    def launch_simpsons(self):
        """Launch The Simpsons Hit & Run"""  # inserted
        try:
            filename = filedialog.askopenfilename(title='Select The Simpsons Hit & Run Executable', filetypes=[('Executable files', '*.exe'), ('All files', '*.*')])
            if filename:
                import subprocess
                import os
                os.chdir(os.path.dirname(filename))
                process = subprocess.Popen(filename, shell=True, creationflags=subprocess.CREATE_NEW_CONSOLE)
                if process.poll() is None:
                    messagebox.showinfo('Success', 'The Simpsons Hit & Run launched successfully!')
                else:  # inserted
                    messagebox.showerror('Error', 'Failed to launch The Simpsons Hit & Run')
        except Exception as e:
            messagebox.showerror('Error', f'Failed to launch The Simpsons Hit & Run: {e}')

    def run(self):
        """Run the application"""  # inserted
        self.show_login()
if __name__ == '__main__':
    app = WillyAI()
    app.run()
