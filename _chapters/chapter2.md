# Chapter 2: Installation and Setup

## Installing Love2D

### Windows Installation

**Method 1: Direct Download**
1. Visit [love2d.org](https://love2d.org)
2. Click "Download" and select the Windows version
3. Download the ZIP file (love-11.x-win64.zip or win32)
4. Extract the ZIP file to a folder (e.g., `C:\love2d\`)
5. (Optional) Add the folder to your system PATH for easier access

**Method 2: Using Package Managers**
```bash
# Using Chocolatey
choco install love

# Using Scoop
scoop install love
```

### macOS Installation

**Method 1: Homebrew (Recommended)**
```bash
# Install Homebrew if you haven't already
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# Install Love2D
brew install love
```

**Method 2: Direct Download**
1. Visit [love2d.org](https://love2d.org)
2. Download the macOS version
3. Open the DMG file and drag LÖVE to Applications
4. Run from Applications or Launchpad

### Linux Installation

**Ubuntu/Debian:**
```bash
sudo apt update
sudo apt install love
```

**Fedora:**
```bash
sudo dnf install love
```

**Arch Linux:**
```bash
sudo pacman -S love
```

**From Source:**
```bash
# Install dependencies first
sudo apt install build-essential cmake libopenal-dev libvorbis-dev libtheora-dev libfreetype6-dev libphysfs-dev libluajit-5.1-dev libsdl2-dev

# Clone and build
git clone https://github.com/love2d/love.git
cd love
mkdir build && cd build
cmake ..
make -j$(nproc)
sudo make install
```

## Setting Up Your Development Environment

### Project Structure

Create a well-organized project structure:

```
my-awesome-game/
├── main.lua              # Entry point
├── conf.lua              # Configuration
├── README.md             # Project documentation
├── assets/               # Game assets
│   ├── images/           # Sprites, backgrounds, UI
│   │   ├── player.png
│   │   ├── enemies/
│   │   └── ui/
│   ├── sounds/           # Sound effects and music
│   │   ├── sfx/
│   │   └── music/
│   ├── fonts/            # Custom fonts
│   └── data/             # Game data files
├── src/                  # Source code modules
│   ├── player.lua
│   ├── enemy.lua
│   ├── gamestate.lua
│   └── utils.lua
├── libs/                 # External libraries
└── build/                # Build output
```

### Text Editors and IDEs

**Recommended Editors:**

1. **Visual Studio Code** (Free)
   - Install the "Lua" extension by sumneko
   - Install "Love2D Support" extension
   - Excellent debugging and IntelliSense support

2. **ZeroBrane Studio** (Free, Lua-specific)
   - Built-in Love2D support
   - Integrated debugger
   - Lua-focused features

3. **Sublime Text** (Paid)
   - Fast and lightweight
   - Install "Lua Dev" package
   - Good for quick edits

4. **Atom** (Free, discontinued but still usable)
   - Install "love-ide" package
   - Community packages available

**VS Code Setup for Love2D:**
1. Install Visual Studio Code
2. Install extensions:
   - "Lua" by sumneko
   - "Love2D Support" by pixelbyte-studios
3. Create `.vscode/settings.json`:

```json
{
    "lua.diagnostics.globals": [
        "love"
    ],
    "lua.workspace.library": [
        "/path/to/love2d/api"
    ]
}
```

### Running Your Game

**Command Line:**
```bash
# Navigate to your game directory
cd my-awesome-game

# Run with Love2D
love .

# Or specify the directory
love my-awesome-game/

# On Windows (if not in PATH)
"C:\love2d\love.exe" .
```

**Drag and Drop:**
- Drag your game folder onto the Love2D executable
- On macOS, drag onto the LÖVE app icon

**File Association:**
- Associate `.love` files with Love2D
- Double-click `.love` files to run them

### Your First Program

Create `main.lua`:

```lua
function love.draw()
    love.graphics.print("Hello, Love2D World!", 100, 100)
end
```

**Output:** A window displaying "Hello, Love2D World!" at position (100, 100).

### Adding Configuration

Create `conf.lua` to customize your game:

```lua
function love.conf(t)
    -- Game identity (for save files)
    t.identity = "my-awesome-game"
    
    -- Love2D version
    t.version = "11.4"
    
    -- Window settings
    t.window.title = "My Awesome Game"
    t.window.icon = "assets/images/icon.png"
    t.window.width = 800
    t.window.height = 600
    t.window.resizable = false
    t.window.minwidth = 400
    t.window.minheight = 300
    t.window.fullscreen = false
    t.window.fullscreentype = "desktop"
    t.window.vsync = 1
    t.window.msaa = 0
    t.window.display = 1
    t.window.highdpi = false
    t.window.usedpiscale = true
    t.window.borderless = false
    t.window.centered = true
    
    -- Console (Windows only)
    t.console = false
    
    -- Modules (disable unused ones for smaller builds)
    t.modules.audio = true
    t.modules.event = true
    t.modules.graphics = true
    t.modules.image = true
    t.modules.joystick = true
    t.modules.keyboard = true
    t.modules.math = true
    t.modules.mouse = true
    t.modules.physics = true
    t.modules.sound = true
    t.modules.system = true
    t.modules.thread = true
    t.modules.timer = true
    t.modules.touch = true
    t.modules.video = false
    t.modules.window = true
end
```

### Common Setup Issues

**Issue: "love: command not found"**
- Solution: Add Love2D to your system PATH or use the full path

**Issue: Game window doesn't appear**
- Check for syntax errors in your Lua files
- Ensure `main.lua` exists in your project root
- Check the console for error messages

**Issue: Assets not loading**
- Verify file paths are correct (case-sensitive on Linux/macOS)
- Ensure assets are in the correct directory structure
- Use forward slashes in paths, even on Windows

**Issue: Performance problems**
- Update graphics drivers
- Check if integrated graphics are being used instead of dedicated GPU
- Reduce window size or disable vsync for testing

### Development Tips

1. **Use Version Control**: Initialize a Git repository for your project
2. **Start Small**: Begin with simple projects to learn the basics
3. **Test Early**: Run your game frequently during development
4. **Read the Wiki**: The official documentation is comprehensive
5. **Join the Community**: The Love2D forum and Discord are very helpful

### Next Steps

Now that you have Love2D installed and configured, you're ready to create your first interactive game. In the next chapter, we'll explore the basic game structure and create a simple moving character.

---

**Chapter Summary:**
- Love2D can be installed on Windows, macOS, and Linux
- Proper project structure helps organize your game
- Various text editors support Love2D development
- Configuration files customize your game's behavior
- Common setup issues have straightforward solutions
- Development best practices improve your workflow