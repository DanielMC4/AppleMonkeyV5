<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Apple Monkey HTML Editor</title>
    <style>
        :root {
            --primary-red: #c41e3a;
            --dark-red: #8b0000;
            --light-red: #ffcccb;
            --accent-red: #ff6b6b;
            --text-light: #f5f5f5;
            --text-dark: #333;
            --bg-dark: #2a2a2a;
            --bg-darker: #1a1a1a;
            --border-radius: 8px;
        }
        
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }
        
        body {
            background-color: var(--bg-darker);
            color: var(--text-light);
            line-height: 1.6;
            overflow-x: hidden;
        }
        
        .container {
            display: flex;
            min-height: 100vh;
        }
        
        /* Sidebar Styles */
        .sidebar {
            width: 250px;
            background-color: var(--dark-red);
            padding: 20px;
            display: flex;
            flex-direction: column;
            box-shadow: 2px 0 5px rgba(0, 0, 0, 0.3);
            z-index: 10;
        }
        
        .logo {
            display: flex;
            align-items: center;
            margin-bottom: 30px;
            padding-bottom: 15px;
            border-bottom: 1px solid rgba(255, 255, 255, 0.2);
        }
        
        .logo h1 {
            font-size: 24px;
            font-weight: bold;
            color: white;
            margin-left: 10px;
        }
        
        .logo-icon {
            font-size: 28px;
        }
        
        .nav-menu {
            list-style: none;
            margin-bottom: 30px;
        }
        
        .nav-menu li {
            margin-bottom: 15px;
        }
        
        .nav-menu a {
            color: var(--text-light);
            text-decoration: none;
            display: flex;
            align-items: center;
            padding: 10px;
            border-radius: var(--border-radius);
            transition: background-color 0.3s;
        }
        
        .nav-menu a:hover, .nav-menu a.active {
            background-color: var(--primary-red);
        }
        
        .nav-menu i {
            margin-right: 10px;
            font-size: 18px;
        }
        
        .script-list {
            flex-grow: 1;
            overflow-y: auto;
        }
        
        .script-item {
            background-color: rgba(255, 255, 255, 0.1);
            padding: 10px;
            margin-bottom: 10px;
            border-radius: var(--border-radius);
            cursor: pointer;
            transition: background-color 0.3s;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        
        .script-item:hover {
            background-color: rgba(255, 255, 255, 0.2);
        }
        
        .script-item.active {
            background-color: var(--primary-red);
        }
        
        .delete-btn {
            background: none;
            border: none;
            color: #ff6b6b;
            cursor: pointer;
            font-size: 16px;
            padding: 2px 5px;
            border-radius: 3px;
        }
        
        .delete-btn:hover {
            background-color: rgba(255, 255, 255, 0.2);
        }
        
        /* Main Content Styles */
        .main-content {
            flex-grow: 1;
            display: flex;
            flex-direction: column;
            padding: 20px;
            position: relative;
        }
        
        .header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 20px;
            padding-bottom: 15px;
            border-bottom: 1px solid rgba(255, 255, 255, 0.1);
        }
        
        .header h2 {
            font-size: 24px;
            color: var(--primary-red);
        }
        
        .btn {
            background-color: var(--primary-red);
            color: white;
            border: none;
            padding: 10px 15px;
            border-radius: var(--border-radius);
            cursor: pointer;
            transition: background-color 0.3s;
            font-weight: bold;
        }
        
        .btn:hover {
            background-color: var(--accent-red);
        }
        
        .btn-secondary {
            background-color: #555;
        }
        
        .btn-secondary:hover {
            background-color: #666;
        }
        
        /* Editor Section */
        .editor-container {
            flex-grow: 1;
            display: flex;
            flex-direction: column;
            background-color: var(--bg-dark);
            border-radius: var(--border-radius);
            overflow: hidden;
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
        }
        
        .editor-toolbar {
            background-color: var(--dark-red);
            padding: 10px 15px;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        
        .editor-toolbar .btn {
            padding: 5px 10px;
            font-size: 14px;
            margin-left: 5px;
        }
        
        .script-name {
            font-weight: bold;
            color: white;
        }
        
        .editor {
            flex-grow: 1;
            padding: 15px;
            background-color: #1e1e1e;
            overflow-y: auto;
        }
        
        textarea {
            width: 100%;
            height: 100%;
            background-color: #1e1e1e;
            color: #d4d4d4;
            border: none;
            resize: none;
            font-family: 'Courier New', monospace;
            font-size: 14px;
            line-height: 1.5;
            min-height: 400px;
        }
        
        textarea:focus {
            outline: none;
        }
        
        /* Forum Styles */
        .forum-container {
            display: none;
            flex-grow: 1;
            background-color: var(--bg-dark);
            border-radius: var(--border-radius);
            padding: 20px;
            overflow-y: auto;
        }
        
        .forum-post {
            background-color: rgba(255, 255, 255, 0.05);
            padding: 15px;
            margin-bottom: 15px;
            border-radius: var(--border-radius);
        }
        
        .forum-post h3 {
            color: var(--primary-red);
            margin-bottom: 10px;
        }
        
        .forum-post .meta {
            font-size: 12px;
            color: #aaa;
            margin-bottom: 10px;
        }
        
        /* Script Store Styles */
        .store-container {
            display: none;
            flex-grow: 1;
            background-color: var(--bg-dark);
            border-radius: var(--border-radius);
            padding: 20px;
            overflow-y: auto;
        }
        
        .script-grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
            gap: 20px;
        }
        
        .script-card {
            background-color: rgba(255, 255, 255, 0.05);
            border-radius: var(--border-radius);
            padding: 15px;
            transition: transform 0.3s;
        }
        
        .script-card:hover {
            transform: translateY(-5px);
        }
        
        .script-card h3 {
            color: var(--primary-red);
            margin-bottom: 10px;
        }
        
        .script-card p {
            font-size: 14px;
            margin-bottom: 15px;
            color: #ccc;
        }
        
        .script-card .btn {
            width: 100%;
            padding: 8px;
            font-size: 14px;
        }
        
        /* Settings Styles */
        .settings-container {
            display: none;
            flex-grow: 1;
            background-color: var(--bg-dark);
            border-radius: var(--border-radius);
            padding: 20px;
            overflow-y: auto;
        }
        
        .settings-section {
            margin-bottom: 30px;
        }
        
        .settings-section h3 {
            color: var(--primary-red);
            margin-bottom: 15px;
        }
        
        .setting-item {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 10px 0;
            border-bottom: 1px solid rgba(255, 255, 255, 0.1);
        }
        
        /* About Styles */
        .about-container {
            display: none;
            flex-grow: 1;
            background-color: var(--bg-dark);
            border-radius: var(--border-radius);
            padding: 20px;
            overflow-y: auto;
        }
        
        .about-content {
            max-width: 800px;
        }
        
        .about-content h3 {
            color: var(--primary-red);
            margin: 20px 0 10px;
        }
        
        /* Active states */
        .content-section.active {
            display: flex;
            flex-direction: column;
        }
        
        /* New Script Modal */
        .modal {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(0, 0, 0, 0.7);
            z-index: 100;
            justify-content: center;
            align-items: center;
        }
        
        .modal-content {
            background-color: var(--bg-dark);
            padding: 20px;
            border-radius: var(--border-radius);
            width: 400px;
            max-width: 90%;
        }
        
        .modal h3 {
            color: var(--primary-red);
            margin-bottom: 15px;
        }
        
        .modal input {
            width: 100%;
            padding: 10px;
            margin-bottom: 15px;
            border-radius: var(--border-radius);
            border: 1px solid #555;
            background-color: #333;
            color: white;
        }
        
        .modal-buttons {
            display: flex;
            justify-content: flex-end;
            gap: 10px;
        }
        
        /* Responsive adjustments */
        @media (max-width: 768px) {
            .container {
                flex-direction: column;
            }
            
            .sidebar {
                width: 100%;
                height: auto;
            }
            
            .script-list {
                max-height: 200px;
            }
            
            .editor-toolbar {
                flex-direction: column;
                gap: 10px;
            }
            
            .editor-toolbar > div {
                width: 100%;
                display: flex;
                justify-content: space-between;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <!-- Sidebar -->
        <div class="sidebar">
            <div class="logo">
                <span class="logo-icon">🐵</span>
                <h1>Apple Monkey</h1>
            </div>
            
            <ul class="nav-menu">
                <li><a href="#" class="active" data-section="editor-section"><i>📝</i> HTML Editor</a></li>
                <li><a href="#" data-section="store-section"><i>🛒</i> HTML Store</a></li>
                <li><a href="#" data-section="forum-section"><i>💬</i> Forum</a></li>
                <li><a href="#" data-section="settings-section"><i>⚙️</i> Settings</a></li>
                <li><a href="#" data-section="about-section"><i>ℹ️</i> About</a></li>
            </ul>
            
            <div class="script-list">
                <div style="display: flex; justify-content: space-between; align-items: center; margin-bottom: 10px;">
                    <h3>My HTML Files</h3>
                    <button class="btn" id="new-script-btn" style="padding: 5px 10px; font-size: 12px;">+ New</button>
                </div>
                <div class="script-item active" data-script="script1">
                    <span>Orange Hello HTML</span>
                    <button class="delete-btn">×</button>
                </div>
                <div class="script-item" data-script="script2">
                    <span>Graspy Hello HTML</span>
                    <button class="delete-btn">×</button>
                </div>
                <div class="script-item" data-script="script3">
                    <span>Docker HTML</span>
                    <button class="delete-btn">×</button>
                </div>
                <div class="script-item" data-script="script4">
                    <span>Server Equipment HTML</span>
                    <button class="delete-btn">×</button>
                </div>
                <div class="script-item" data-script="script5">
                    <span>HTML TEST</span>
                    <button class="delete-btn">×</button>
                </div>
            </div>
        </div>
        
        <!-- Main Content -->
        <div class="main-content">
            <div class="header">
                <h2 id="section-title">HTML Editor</h2>
                <button class="btn" id="save-btn">Save HTML</button>
            </div>
            
            <!-- Editor Section -->
            <div id="editor-section" class="content-section editor-container active">
                <div class="editor-toolbar">
                    <div class="script-name">Orange Hello HTML</div>
                    <div>
                        <button class="btn" id="download-btn">Download</button>
                    </div>
                </div>
                <div class="editor">
                    <textarea id="html-editor" spellcheck="false"></textarea>
                </div>
            </div>
            
            <!-- HTML Store Section -->
            <div id="store-section" class="content-section store-container">
                <h2>HTML Templates Store</h2>
                <p>Browse and download HTML templates</p>
                <div class="script-grid">
                    <div class="script-card">
                        <h3>Portfolio Template</h3>
                        <p>Clean portfolio website template</p>
                        <button class="btn">Download</button>
                    </div>
                    <div class="script-card">
                        <h3>Business Template</h3>
                        <p>Professional business website template</p>
                        <button class="btn">Download</button>
                    </div>
                    <div class="script-card">
                        <h3>Blog Template</h3>
                        <p>Modern blog template with responsive design</p>
                        <button class="btn">Download</button>
                    </div>
                    <div class="script-card">
                        <h3>E-commerce Template</h3>
                        <p>Online store template with product listings</p>
                        <button class="btn">Download</button>
                    </div>
                </div>
            </div>
            
            <!-- Forum Section -->
            <div id="forum-section" class="content-section forum-container">
                <h2>HTML Forum</h2>
                <p>Discuss HTML topics and get help</p>
                <div class="forum-post">
                    <h3>How to create a responsive layout?</h3>
                    <div class="meta">Posted by: WebDesigner • 2 days ago</div>
                    <p>I'm having trouble making my layout responsive. Any tips on using CSS media queries effectively?</p>
                </div>
                <div class="forum-post">
                    <h3>Best practices for HTML5 semantics</h3>
                    <div class="meta">Posted by: SemanticExpert • 1 week ago</div>
                    <p>Share your tips for using HTML5 semantic elements correctly.</p>
                </div>
                <div class="forum-post">
                    <h3>Common issues with flexbox</h3>
                    <div class="meta">Posted by: FlexMaster • 3 days ago</div>
                    <p>Let's discuss common flexbox problems and their solutions.</p>
                </div>
                <div class="forum-post">
                    <h3>HTML TEST - How?</h3>
                    <div class="meta">Posted by: HTMLUser • 1 day ago</div>
                    <p>I'm trying to test HTML scripts. Any suggestions on how to properly set them up?</p>
                </div>
            </div>
            
            <!-- Settings Section -->
            <div id="settings-section" class="content-section settings-container">
                <h2>Settings</h2>
                <p>Customize your Apple Monkey HTML Editor experience</p>
                <div class="settings-section">
                    <h3>Editor Preferences</h3>
                    <div class="setting-item">
                        <span>Theme</span>
                        <select style="background: #333; color: white; padding: 5px; border-radius: 4px;">
                            <option>Red Theme</option>
                            <option>Dark Theme</option>
                            <option>Light Theme</option>
                        </select>
                    </div>
                    <div class="setting-item">
                        <span>Font Size</span>
                        <select style="background: #333; color: white; padding: 5px; border-radius: 4px;">
                            <option>Small</option>
                            <option selected>Medium</option>
                            <option>Large</option>
                        </select>
                    </div>
                    <div class="setting-item">
                        <span>Auto-save</span>
                        <input type="checkbox" checked>
                    </div>
                </div>
                
                <div class="settings-section">
                    <h3>Editor Behavior</h3>
                    <div class="setting-item">
                        <span>Syntax Highlighting</span>
                        <input type="checkbox" checked>
                    </div>
                    <div class="setting-item">
                        <span>Line Numbers</span>
                        <input type="checkbox" checked>
                    </div>
                </div>
            </div>
            
            <!-- About Section -->
            <div id="about-section" class="content-section about-container">
                <h2>About Apple Monkey HTML Editor</h2>
                <div class="about-content">
                    <p>Apple Monkey is a powerful HTML editor designed for creating and managing HTML files.</p>
                    
                    <h3>Features</h3>
                    <ul>
                        <li>Easy HTML creation and editing</li>
                        <li>HTML template store with community contributions</li>
                        <li>Forum for discussion and support</li>
                        <li>Downloadable HTML files</li>
                        <li>Customizable editor interface</li>
                    </ul>
                    
                    <h3>Version</h3>
                    <p>Apple Monkey HTML Editor v1.0.0</p>
                    
                    <h3>Author</h3>
                    <p>Orange</p>
                </div>
            </div>
        </div>
    </div>
    
    <!-- New Script Modal -->
    <div class="modal" id="new-script-modal">
        <div class="modal-content">
            <h3>Create New HTML File</h3>
            <input type="text" id="new-script-name" placeholder="Enter HTML file name">
            <div class="modal-buttons">
                <button class="btn btn-secondary" id="cancel-new-script">Cancel</button>
                <button class="btn" id="create-new-script">Create</button>
            </div>
        </div>
    </div>

    <script>
        // Navigation between sections
        document.querySelectorAll('.nav-menu a').forEach(link => {
            link.addEventListener('click', function(e) {
                e.preventDefault();
                
                // Remove active class from all links
                document.querySelectorAll('.nav-menu a').forEach(item => {
                    item.classList.remove('active');
                });
                
                // Add active class to clicked link
                this.classList.add('active');
                
                // Get the target section
                const targetSection = this.getAttribute('data-section');
                
                // Hide all sections
                document.querySelectorAll('.content-section').forEach(section => {
                    section.classList.remove('active');
                });
                
                // Show target section
                document.getElementById(targetSection).classList.add('active');
                
                // Update section title
                document.getElementById('section-title').textContent = this.textContent.trim();
                
                // Show/hide save button based on current section
                const saveBtn = document.getElementById('save-btn');
                if (targetSection === 'editor-section') {
                    saveBtn.style.display = 'block';
                } else {
                    saveBtn.style.display = 'none';
                }
            });
        });
        
        // Script selection
        document.querySelectorAll('.script-item').forEach(item => {
            const deleteBtn = item.querySelector('.delete-btn');
            
            // Select script
            item.addEventListener('click', function(e) {
                if (e.target !== deleteBtn) {
                    // Remove active class from all script items
                    document.querySelectorAll('.script-item').forEach(script => {
                        script.classList.remove('active');
                    });
                    
                    // Add active class to clicked script
                    this.classList.add('active');
                    
                    // Update script name in editor toolbar
                    document.querySelector('.script-name').textContent = this.querySelector('span').textContent;
                    
                    // Load script content
                    const scriptId = this.getAttribute('data-script');
                    const scriptContent = getScriptContent(scriptId);
                    document.getElementById('html-editor').value = scriptContent;
                }
            });
            
            // Delete script
            deleteBtn.addEventListener('click', function(e) {
                e.stopPropagation();
                const scriptItem = this.parentElement;
                const scriptName = scriptItem.querySelector('span').textContent;
                
                if (confirm(`Are you sure you want to delete "${scriptName}"?`)) {
                    scriptItem.remove();
                    
                    // If we deleted the active script, select the first one
                    const activeScript = document.querySelector('.script-item.active');
                    if (!activeScript && document.querySelectorAll('.script-item').length > 0) {
                        document.querySelector('.script-item').click();
                    }
                }
            });
        });
        
        // Function to get script content
        function getScriptContent(scriptId) {
            const scriptContent = {
                'script1': `<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Orange Hello HTML</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            max-width: 800px;
            margin: 0 auto;
            padding: 20px;
            background-color: #f5f5f5;
            color: #333;
        }
        h1 {
            color: #c41e3a;
        }
        .container {
            background: white;
            padding: 20px;
            border-radius: 8px;
            box-shadow: 0 2px 5px rgba(0,0,0,0.1);
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>Hello from Orange HTML!</h1>
        <p>This is a sample HTML file created with Apple Monkey HTML Editor.</p>
    </div>
</body>
</html>`,
                'script2': `<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Graspy Hello HTML</title>
</head>
<body>
    <h1>Graspy HTML File</h1>
    <p>This is the Graspy HTML template.</p>
</body>
</html>`,
                'script3': `<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Docker HTML</title>
</head>
<body>
    <h1>Docker Configuration</h1>
    <p>HTML template for Docker documentation.</p>
</body>
</html>`,
                'script4': `<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Server Equipment HTML</title>
</head>
<body>
    <h1>Server Equipment</h1>
    <p>HTML template for server equipment documentation.</p>
</body>
</html>`,
                'script5': `<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>HTML TEST</title>
</head>
<body>
    <h1>HTML TEST Page</h1>
    <p>This is a test HTML page.</p>
</body>
</html>`
            };
            
            return scriptContent[scriptId] || `<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>New HTML File</title>
</head>
<body>
    <h1>Your New HTML File</h1>
    <p>Start editing your HTML code here.</p>
</body>
</html>`;
        }
        
        // Save button functionality
        document.getElementById('save-btn').addEventListener('click', function() {
            const scriptName = document.querySelector('.script-name').textContent;
            const scriptContent = document.getElementById('html-editor').value;
            
            // In a real app, you would save to local storage or a server
            alert(`HTML file "${scriptName}" saved successfully!`);
        });
        
        // Download button functionality
        document.getElementById('download-btn').addEventListener('click', function() {
            const scriptName = document.querySelector('.script-name').textContent;
            const scriptContent = document.getElementById('html-editor').value;
            
            // Create a blob and download link
            const blob = new Blob([scriptContent], { type: 'text/html' });
            const url = URL.createObjectURL(blob);
            const a = document.createElement('a');
            a.href = url;
            a.download = scriptName.replace(/\s+/g, '_').toLowerCase() + '.html';
            document.body.appendChild(a);
            a.click();
            document.body.removeChild(a);
            URL.revokeObjectURL(url);
        });
        
        // New script modal functionality
        document.getElementById('new-script-btn').addEventListener('click', function() {
            document.getElementById('new-script-modal').style.display = 'flex';
        });
        
        document.getElementById('cancel-new-script').addEventListener('click', function() {
            document.getElementById('new-script-modal').style.display = 'none';
            document.getElementById('new-script-name').value = '';
        });
        
        document.getElementById('create-new-script').addEventListener('click', function() {
            const scriptName = document.getElementById('new-script-name').value.trim();
            if (scriptName) {
                // Create new script item
                const scriptList = document.querySelector('.script-list');
                const newScript = document.createElement('div');
                newScript.className = 'script-item';
                newScript.setAttribute('data-script', 'script' + (document.querySelectorAll('.script-item').length + 1));
                
                newScript.innerHTML = `
                    <span>${scriptName}</span>
                    <button class="delete-btn">×</button>
                `;
                
                // Add click event to new script
                newScript.addEventListener('click', function(e) {
                    if (e.target.classList.contains('delete-btn')) return;
                    
                    document.querySelectorAll('.script-item').forEach(item => {
                        item.classList.remove('active');
                    });
                    this.classList.add('active');
                    document.querySelector('.script-name').textContent = this.querySelector('span').textContent;
                    document.getElementById('html-editor').value = `<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>${scriptName}</title>
</head>
<body>
    <h1>${scriptName}</h1>
    <p>Your HTML content goes here.</p>
</body>
</html>`;
                });
                
                // Add delete functionality
                newScript.querySelector('.delete-btn').addEventListener('click', function(e) {
                    e.stopPropagation();
                    const scriptItem = this.parentElement;
                    const scriptName = scriptItem.querySelector('span').textContent;
                    
                    if (confirm(`Are you sure you want to delete "${scriptName}"?`)) {
                        scriptItem.remove();
                    }
                });
                
                scriptList.appendChild(newScript);
                
                // Close modal and reset input
                document.getElementById('new-script-modal').style.display = 'none';
                document.getElementById('new-script-name').value = '';
                
                // Switch to editor and select new script
                document.querySelector('.nav-menu a[data-section="editor-section"]').click();
                newScript.click();
            }
        });
        
        // Initialize with first script
        document.querySelector('.script-item.active').click();
    </script>
</body>
</html>
