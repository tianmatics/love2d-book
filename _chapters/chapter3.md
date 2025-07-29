---
title: Your First Love2D Game
order: 3
---
# Chapter 3: Your First Love2D Game

## Understanding the Game Loop

Every Love2D game is built around a simple game loop with three main phases:

1. **Update**: Process input, update game logic, handle physics
2. **Draw**: Render graphics to the screen
3. **Events**: Handle special events like key presses or window resizing

Love2D handles this loop automatically and calls specific functions in your code at the right times.

## Basic Love2D Callbacks

Love2D uses callback functions that you define to handle different parts of the game loop:

```lua
-- Called once when the game starts
function love.load()
    -- Initialize variables, load assets
end

-- Called every frame to update game logic
function love.update(dt)
    -- dt = delta time (seconds since last frame)
    -- Update positions, handle input, etc.
end

-- Called every frame to draw graphics
function love.draw()
    -- Draw sprites, shapes, text, etc.
end

-- Called when a key is pressed
function love.keypressed(key)
    -- Handle single key press events
end

-- Called when a key is released
function love.keyreleased(key)
    -- Handle key release events
end
```

## Your First Interactive Game

Let's create a simple game where you control a circle with the arrow keys:

```lua
-- Game variables
local player = {
    x = 400,
    y = 300,
    speed = 200,
    size = 20
}

-- Called once at startup
function love.load()
    love.window.setTitle("My First Love2D Game")
    love.graphics.setBackgroundColor(0.1, 0.1, 0.2) -- Dark blue background
end

-- Called every frame (dt = delta time)
function love.update(dt)
    -- Check for continuous key presses
    if love.keyboard.isDown("left") or love.keyboard.isDown("a") then
        player.x = player.x - player.speed * dt
    end
    
    if love.keyboard.isDown("right") or love.keyboard.isDown("d") then
        player.x = player.x + player.speed * dt
    end
    
    if love.keyboard.isDown("up") or love.keyboard.isDown("w") then
        player.y = player.y - player.speed * dt
    end
    
    if love.keyboard.isDown("down") or love.keyboard.isDown("s") then
        player.y = player.y + player.speed * dt
    end
    
    -- Keep player on screen
    if player.x < player.size then
        player.x = player.size
    elseif player.x > love.graphics.getWidth() - player.size then
        player.x = love.graphics.getWidth() - player.size
    end
    
    if player.y < player.size then
        player.y = player.size
    elseif player.y > love.graphics.getHeight() - player.size then
        player.y = love.graphics.getHeight() - player.size
    end
end

-- Called every frame to draw
function love.draw()
    -- Draw player as a white circle
    love.graphics.setColor(1, 1, 1) -- White color (RGB values 0-1)
    love.graphics.circle("fill", player.x, player.y, player.size)
    
    -- Draw instructions
    love.graphics.setColor(0.8, 0.8, 0.8) -- Light gray
    love.graphics.print("Use WASD or arrow keys to move", 10, 10)
    love.graphics.print("Press ESC to quit", 10, 30)
    
    -- Display player position
    love.graphics.print("Position: " .. math.floor(player.x) .. ", " .. math.floor(player.y), 10, 60)
end

-- Handle single key press events
function love.keypressed(key)
    if key == "escape" then
        love.event.quit()
    end
    
    -- Reset player position with spacebar
    if key == "space" then
        player.x = 400
        player.y = 300
    end
end
```

**Output:** A white circle that moves smoothly with WASD or arrow keys, stays within screen bounds, and shows position information.

## Adding Color and Visual Feedback

Let's enhance our game with colors and visual effects:

```lua
local player = {
    x = 400,
    y = 300,
    speed = 200,
    size = 20,
    color = {r = 1, g = 1, b = 1}, -- White
    isMoving = false
}

local trail = {} -- Store previous positions for a trail effect

function love.load()
    love.window.setTitle("Enhanced Movement Game")
    love.graphics.setBackgroundColor(0.05, 0.05, 0.1)
end

function love.update(dt)
    local oldX, oldY = player.x, player.y
    player.isMoving = false
    
    -- Movement with different colors
    if love.keyboard.isDown("left", "a") then
        player.x = player.x - player.speed * dt
        player.color = {r = 1, g = 0.3, b = 0.3} -- Red when moving left
        player.isMoving = true
    end
    
    if love.keyboard.isDown("right", "d") then
        player.x = player.x + player.speed * dt
        player.color = {r = 0.3, g = 1, b = 0.3} -- Green when moving right
        player.isMoving = true
    end
    
    if love.keyboard.isDown("up", "w") then
        player.y = player.y - player.speed * dt
        player.color = {r = 0.3, g = 0.3, b = 1} -- Blue when moving up
        player.isMoving = true
    end
    
    if love.keyboard.isDown("down", "s") then
        player.y = player.y + player.speed * dt
        player.color = {r = 1, g = 1, b = 0.3} -- Yellow when moving down
        player.isMoving = true
    end
    
    -- Return to white when not moving
    if not player.isMoving then
        player.color = {r = 1, g = 1, b = 1}
    end
    
    -- Keep player on screen
    player.x = math.max(player.size, math.min(love.graphics.getWidth() - player.size, player.x))
    player.y = math.max(player.size, math.min(love.graphics.getHeight() - player.size, player.y))
    
    -- Add to trail if position changed
    if player.x ~= oldX or player.y ~= oldY then
        table.insert(trail, {
            x = oldX,
            y = oldY,
            life = 1.0,
            color = {r = player.color.r, g = player.color.g, b = player.color.b}
        })
    end
    
    -- Update trail
    for i = #trail, 1, -1 do
        trail[i].life = trail[i].life - dt * 2
        if trail[i].life <= 0 then
            table.remove(trail, i)
        end
    end
end

function love.draw()
    -- Draw trail
    for _, point in ipairs(trail) do
        love.graphics.setColor(point.color.r, point.color.g, point.color.b, point.life * 0.5)
        love.graphics.circle("fill", point.x, point.y, player.size * point.life)
    end
    
    -- Draw player
    love.graphics.setColor(player.color.r, player.color.g, player.color.b)
    love.graphics.circle("fill", player.x, player.y, player.size)
    
    -- Draw a border around the player
    love.graphics.setColor(1, 1, 1)
    love.graphics.circle("line", player.x, player.y, player.size)
    
    -- UI Text
    love.graphics.setColor(0.9, 0.9, 0.9)
    love.graphics.print("Use WASD or arrow keys to move", 10, 10)
    love.graphics.print("Player changes color based on direction!", 10, 30)
    love.graphics.print("Space: Reset position | ESC: Quit", 10, 50)
    
    local speedText = player.isMoving and "Moving" or "Stopped"
    love.graphics.print("Status: " .. speedText, 10, 80)
end

function love.keypressed(key)
    if key == "escape" then
        love.event.quit()
    elseif key == "space" then
        player.x = love.graphics.getWidth() / 2
        player.y = love.graphics.getHeight() / 2
        trail = {} -- Clear trail
    end
end
```

**Output:** A circle that changes color based on movement direction, leaves a fading trail, and has enhanced visual feedback.

## Adding Sound Effects

Let's add audio feedback to make the game more engaging:

```lua
local player = {
    x = 400,
    y = 300,
    speed = 200,
    size = 20
}

local sounds = {}

function love.load()
    love.window.setTitle("Movement Game with Sound")
    
    -- Create simple sound effects programmatically
    -- Note: In a real game, you'd load sound files
    -- sounds.move = love.audio.newSource("assets/sounds/move.wav", "static")
    -- sounds.reset = love.audio.newSource("assets/sounds/reset.wav", "static")
    
    -- For this example, we'll use placeholder sounds
    -- You can add actual sound files to your assets folder
end

function love.update(dt)
    local wasMoving = player.isMoving or false
    player.isMoving = false
    
    if love.keyboard.isDown("left", "a", "right", "d", "up", "w", "down", "s") then
        if love.keyboard.isDown("left", "a") then
            player.x = player.x - player.speed * dt
        end
        if love.keyboard.isDown("right", "d") then
            player.x = player.x + player.speed * dt
        end
        if love.keyboard.isDown("up", "w") then
            player.y = player.y - player.speed * dt
        end
        if love.keyboard.isDown("down", "s") then
            player.y = player.y + player.speed * dt
        end
        
        player.isMoving = true
        
        -- Play movement sound when starting to move
        if not wasMoving and sounds.move then
            sounds.move:play()
        end
    end
    
    -- Keep player on screen
    player.x = math.max(player.size, math.min(love.graphics.getWidth() - player.size, player.x))
    player.y = math.max(player.size, math.min(love.graphics.getHeight() - player.size, player.y))
end

function love.draw()
    -- Draw player
    love.graphics.setColor(0.3, 0.7, 1) -- Light blue
    love.graphics.circle("fill", player.x, player.y, player.size)
    
    -- Draw border
    love.graphics.setColor(1, 1, 1)
    love.graphics.circle("line", player.x, player.y, player.size)
    
    -- Instructions
    love.graphics.print("WASD/Arrow keys: Move", 10, 10)
    love.graphics.print("Space: Reset | ESC: Quit", 10, 30)
    love.graphics.print("Position: " .. math.floor(player.x) .. ", " .. math.floor(player.y), 10, 60)
end

function love.keypressed(key)
    if key == "escape" then
        love.event.quit()
    elseif key == "space" then
        -- Play reset sound
        if sounds.reset then
            sounds.reset:play()
        end
        
        player.x = love.graphics.getWidth() / 2
        player.y = love.graphics.getHeight() / 2
    end
end
```

## Game Structure Best Practices

Even for simple games, good organization helps:

```lua
-- Game state
local game = {
    state = "playing", -- "playing", "paused", "menu"
    score = 0,
    time = 0
}

local player = {
    x = 400,
    y = 300,
    speed = 200,
    size = 20
}

-- Separate update functions for clarity
function updatePlayer(dt)
    if love.keyboard.isDown("left", "a") then
        player.x = player.x - player.speed * dt
    end
    if love.keyboard.isDown("right", "d") then
        player.x = player.x + player.speed * dt
    end
    if love.keyboard.isDown("up", "w") then
        player.y = player.y - player.speed * dt
    end
    if love.keyboard.isDown("down", "s") then
        player.y = player.y + player.speed * dt
    end
    
    -- Boundary checking
    player.x = math.max(player.size, math.min(love.graphics.getWidth() - player.size, player.x))
    player.y = math.max(player.size, math.min(love.graphics.getHeight() - player.size, player.y))
end

function updateGame(dt)
    game.time = game.time + dt
    game.score = math.floor(game.time * 10) -- Score based on time
end

-- Separate draw functions
function drawPlayer()
    love.graphics.setColor(0.2, 0.8, 0.2) -- Green
    love.graphics.circle("fill", player.x, player.y, player.size)
    
    love.graphics.setColor(1, 1, 1)
    love.graphics.circle("line", player.x, player.y, player.size)
end

function drawUI()
    love.graphics.setColor(1, 1, 1)
    love.graphics.print("Score: " .. game.score, 10, 10)
    love.graphics.print("Time: " .. math.floor(game.time), 10, 30)
    love.graphics.print("WASD: Move | P: Pause | ESC: Quit", 10, 60)
    
    if game.state == "paused" then
        love.graphics.setColor(1, 1, 0)
        love.graphics.print("PAUSED - Press P to resume", 300, 200)
    end
end

-- Main Love2D callbacks
function love.load()
    love.window.setTitle("Structured Game Example")
    love.graphics.setBackgroundColor(0.1, 0.1, 0.1)
end

function love.update(dt)
    if game.state == "playing" then
        updatePlayer(dt)
        updateGame(dt)
    end
end

function love.draw()
    if game.state == "playing" or game.state == "paused" then
        drawPlayer()
        drawUI()
    end
end

function love.keypressed(key)
    if key == "escape" then
        love.event.quit()
    elseif key == "p" then
        if game.state == "playing" then
            game.state = "paused"
        elseif game.state == "paused" then
            game.state = "playing"
        end
    elseif key == "space" then
        -- Reset game
        player.x = love.graphics.getWidth() / 2
        player.y = love.graphics.getHeight() / 2
        game.score = 0
        game.time = 0
    end
end
```

**Output:** A well-structured game with pause functionality, score system, and organized code.

## Common Beginner Mistakes

1. **Forgetting Delta Time**: Always multiply movement by `dt` for frame-rate independent movement
2. **Color Range Confusion**: Love2D uses 0-1 for colors, not 0-255
3. **Missing love.graphics.setColor()**: Remember to set colors before drawing
4. **Coordinate System**: (0,0) is top-left, Y increases downward
5. **File Path Issues**: Use forward slashes and check case sensitivity

## Next Steps

You've created your first interactive Love2D game! In the next chapter, we'll dive deeper into the game structure and explore the complete set of Love2D callbacks and their uses.

---

**Chapter Summary:**
- Love2D games use callback functions for different phases
- `love.update(dt)` handles game logic and input
- `love.draw()` renders graphics to screen
- Delta time (`dt`) ensures smooth, frame-rate independent movement
- Good code organization improves maintainability
- Color values in Love2D range from 0 to 1
- Always test your game frequently during development
