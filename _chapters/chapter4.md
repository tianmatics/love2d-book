---
order: 5
---
# Chapter 4: Basic Game Structure

## Complete Love2D Callback Reference

Love2D provides many callback functions that handle different aspects of your game. Understanding these callbacks is essential for creating robust games.

### Core Callbacks

```lua
-- Called once when the game starts
function love.load(arg)
    -- arg contains command line arguments
    -- Initialize variables, load assets, set up game state
end

-- Called every frame to update game logic
function love.update(dt)
    -- dt = delta time in seconds since last frame
    -- Update positions, handle continuous input, run AI, etc.
end

-- Called every frame to render graphics
function love.draw()
    -- Draw everything to the screen
    -- Order matters - things drawn later appear on top
end

-- Called when the game is about to quit
function love.quit()
    -- Save game data, clean up resources
    -- Return true to prevent quitting
end
```

### Input Callbacks

```lua
-- Keyboard events
function love.keypressed(key, scancode, isrepeat)
    -- Called once when a key is pressed
    -- key: the key that was pressed
    -- scancode: platform-specific scancode
    -- isrepeat: true if this is a key repeat event
end

function love.keyreleased(key, scancode)
    -- Called once when a key is released
end

function love.textinput(text)
    -- Called when text is typed (good for text input fields)
    -- Handles special characters and international input
end

-- Mouse events
function love.mousepressed(x, y, button, istouch, presses)
    -- Called when a mouse button is pressed
    -- x, y: mouse position
    -- button: 1=left, 2=right, 3=middle
    -- istouch: true if this came from a touch screen
    -- presses: number of presses (for double-click detection)
end

function love.mousereleased(x, y, button, istouch, presses)
    -- Called when a mouse button is released
end

function love.mousemoved(x, y, dx, dy, istouch)
    -- Called when the mouse moves
    -- dx, dy: relative movement since last call
end

function love.wheelmoved(x
