---
title: Drawing and Graphics
order: 5
---
# Chapter 5: Drawing and Graphics

## Understanding the Graphics System

Love2D's graphics system is built on OpenGL and provides a simple yet powerful API for 2D rendering. Everything drawn appears on a coordinate system where (0,0) is the top-left corner, X increases to the right, and Y increases downward.

### Coordinate System

```lua
function love.draw()
    -- Draw coordinate reference points
    love.graphics.setColor(1, 0, 0) -- Red
    love.graphics.circle("fill", 0, 0, 5)     -- Top-left (0,0)
    
    love.graphics.setColor(0, 1, 0) -- Green
    love.graphics.circle("fill", 800, 0, 5)   -- Top-right (800,0)
    
    love.graphics.setColor(0, 0, 1) -- Blue
    love.graphics.circle("fill", 0, 600, 5)   -- Bottom-left (0,600)
    
    love.graphics.setColor(1, 1, 0) -- Yellow
    love.graphics.circle("fill", 800, 600, 5) -- Bottom-right (800,600)
    
    love.graphics.setColor(1, 1, 1)
    love.graphics.print("(0,0)", 10, 10)
    love.graphics.print("(800,0)", 750, 10)
    love.graphics.print("(0,600)", 10, 580)
    love.graphics.print("(800,600)", 730, 580)
end
```

## Basic Shapes

### Drawing Circles

```lua
function love.draw()
    -- Filled circle
    love.graphics.setColor(1, 0, 0) -- Red
    love.graphics.circle("fill", 150, 150, 50)
    
    -- Circle outline
    love.graphics.setColor(0, 1, 0) -- Green
    love.graphics.circle("line", 300, 150, 50)
    
    -- Circle with custom line width
    love.graphics.setColor(0, 0, 1) -- Blue
    love.graphics.setLineWidth(5)
    love.graphics.circle("line", 450, 150, 50)
    love.graphics.setLineWidth(1) -- Reset line width
    
    -- Semi-circles and arcs
    love.graphics.setColor(1, 0, 1) -- Magenta
    love.graphics.arc("fill", 150, 300, 50, 0, math.pi) -- Half circle
    
    love.graphics.setColor(1, 1, 0) -- Yellow
    love.graphics.arc("line", 300, 300, 50, 0, math.pi * 1.5) -- 3/4 circle
end
```

### Drawing Rectangles

```lua
function love.draw()
    -- Filled rectangle
    love.graphics.setColor(0.8, 0.3, 0.3)
    love.graphics.rectangle("fill", 50, 50, 100, 60)
    
    -- Rectangle outline
    love.graphics.setColor(0.3, 0.8, 0.3)
    love.graphics.rectangle("line", 200, 50, 100, 60)
    
    -- Rounded rectangle
    love.graphics.setColor(0.3, 0.3, 0.8)
    love.graphics.rectangle("fill", 350, 50, 100, 60, 10, 10)
    
    -- Rectangle with different corner radii
    love.graphics.setColor(0.8, 0.8, 0.3)
    love.graphics.rectangle("fill", 500, 50, 100, 60, 15, 5)
end
```

### Drawing Lines and Polygons

```lua
function love.draw()
    -- Simple line
    love.graphics.setColor(1, 1, 1)
    love.graphics.line(50, 200, 200, 250)
    
    -- Connected lines
    love.graphics.setColor(1, 0, 0)
    love.graphics.setLineWidth(3)
    love.graphics.line(250, 200, 350, 220, 400, 180, 450, 240)
    
    -- Triangle
    love.graphics.setColor(0, 1, 1)
    love.graphics.polygon("fill", 100, 350, 150, 300, 200, 350)
    
    -- Complex polygon
    love.graphics.setColor(1, 0.5, 0)
    love.graphics.polygon("line", 
        300, 350,  -- Point 1
        350, 320,  -- Point 2
        400, 330,  -- Point 3
        420, 380,  -- Point 4
        380, 400,  -- Point 5
        320, 390   -- Point 6
    )
    
    love.graphics.setLineWidth(1) -- Reset
end
```

## Colors and Transparency

### Color Management

```lua
function love.draw()
    -- RGB colors (values from 0 to 1)
    love.graphics.setColor(1, 0, 0)     -- Pure red
    love.graphics.rectangle("fill", 50, 50, 50, 50)
    
    love.graphics.setColor(0, 1, 0)     -- Pure green
    love.graphics.rectangle("fill", 120, 50, 50, 50)
    
    love.graphics.setColor(0, 0, 1)     -- Pure blue
    love.graphics.rectangle("fill", 190, 50, 50, 50)
    
    -- RGBA colors (with alpha/transparency)
    love.graphics.setColor(1, 0, 0, 0.5)  -- Semi-transparent red
    love.graphics.rectangle("fill", 50, 120, 50, 50)
    
    love.graphics.setColor(0, 1, 0, 0.3)  -- More transparent green
    love.graphics.rectangle("fill", 120, 120, 50, 50)
    
    -- Color mixing through transparency
    love.graphics.setColor(1, 1, 0, 0.7)  -- Semi-transparent yellow
    love.graphics.rectangle("fill", 85, 85, 100, 100)
    
    -- Reset to opaque white
    love.graphics.setColor(1, 1, 1, 1)
end
```

### Color Palettes and Themes

```lua
local colorPalette = {
    background = {0.1, 0.1, 0.2},
    primary = {0.2, 0.7, 0.9},
    secondary = {0.9, 0.4, 0.2},
    accent = {0.9, 0.9, 0.3},
    text = {0.9, 0.9, 0.9},
    danger = {0.9, 0.2, 0.2}
}

function setColor(colorName, alpha)
    local color = colorPalette[colorName]
    if color then
        love.graphics.setColor(color[1], color[2], color[3], alpha or 1)
    else
        love.graphics.setColor(1, 1, 1, alpha or 1) -- Default to white
    end
end

function love.draw()
    -- Set background
    setColor("background")
    love.graphics.rectangle("fill", 0, 0, love.graphics.getWidth(), love.graphics.getHeight())
    
    -- Draw UI elements with consistent colors
    setColor("primary")
    love.graphics.rectangle("fill", 50, 50, 200, 40)
    
    setColor("text")
    love.graphics.print("Primary Button", 60, 60)
    
    setColor("secondary")
    love.graphics.rectangle("fill", 50, 110, 200, 40)
    
    setColor("text")
    love.graphics.print("Secondary Button", 60, 120)
    
    setColor("danger", 0.8) -- Semi-transparent danger color
    love.graphics.rectangle("fill", 50, 170, 200, 40)
    
    setColor("text")
    love.graphics.print("Danger Zone", 60, 180)
end
```

## Working with Images

### Loading and Drawing Images

```lua
local images = {}

function love.load()
    -- Load images (make sure these files exist in your project)
    images.player = love.graphics.newImage("assets/images/player.png")
    images.background = love.graphics.newImage("assets/images/background.png")
    images.coin = love.graphics.newImage("assets/images/coin.png")
    
    -- Set image filter for pixel art (optional)
    love.graphics.setDefaultFilter("nearest", "nearest")
end

function love.draw()
    -- Draw background (stretched to fit screen)
    love.graphics.draw(images.background, 0, 0, 0, 
        love.graphics.getWidth() / images.background:getWidth(),
        love.graphics.getHeight() / images.background:getHeight())
    
    -- Draw player at original size
    love.graphics.draw(images.player, 100, 100)
    
    -- Draw player scaled 2x
    love.graphics.draw(images.player, 200, 100, 0, 2, 2)
    
    -- Draw player rotated and scaled
    local rotation = love.timer.getTime() -- Rotate based on time
    love.graphics.draw(images.player, 350, 150, rotation, 1.5, 1.5,
        images.player:getWidth()/2, images.player:getHeight()/2) -- Center origin
    
    -- Draw coins in a row with transparency
    love.graphics.setColor(1, 1, 1, 0.8)
    for i = 1, 5 do
        love.graphics.draw(images.coin, i * 60, 300)
    end
    
    love.graphics.setColor(1, 1, 1, 1) -- Reset alpha
end
```

### Image Manipulation

```lua
local playerImage
local playerQuad

function love.load()
    -- Load sprite sheet
    playerImage = love.graphics.newImage("assets/images/player_spritesheet.png")
    
    -- Create a quad to display part of the image
    -- Quad parameters: x, y, width, height, image_width, image_height
    playerQuad = love.graphics.newQuad(0, 0, 32, 32, 
        playerImage:getWidth(), playerImage:getHeight())
end

function love.update(dt)
    -- Animate through sprite sheet frames
    local frameTime = 0.2
    local currentTime = love.timer.getTime()
    local frame = math.floor(currentTime / frameTime) % 4 -- 4 frames
    
    playerQuad:setViewport(frame * 32, 0, 32, 32)
end

function love.draw()
    -- Draw the current frame
    love.graphics.draw(playerImage, playerQuad, 400, 300, 0, 3, 3)
    
    -- Draw image information
    love.graphics.print("Image size: " .. playerImage:getWidth() .. "x" .. playerImage:getHeight(), 10, 10)
    love.graphics.print("Quad animating through sprite frames", 10, 30)
end
```

## Text Rendering

### Basic Text

```lua
function love.draw()
    -- Default font
    love.graphics.print("Default font text", 10, 10)
    
    -- Text with different sizes using default font
    love.graphics.print("Normal size", 10, 40)
    love.graphics.print("Large text", 10, 70, 0, 2, 2) -- 2x scale
    love.graphics.print("Rotated text", 200, 100, math.pi/4) -- 45 degrees
    
    -- Colored text
    love.graphics.setColor(1, 0, 0)
    love.graphics.print("Red text", 10, 120)
    
    love.graphics.setColor(0, 1, 0)
    love.graphics.print("Green text", 10, 140)
    
    love.graphics.setColor(1, 1, 1) -- Reset to white
end
```

### Custom Fonts

```lua
local fonts = {}

function love.load()
    -- Load custom fonts
    fonts.small = love.graphics.newFont(12)
    fonts.medium = love.graphics.newFont(18)
    fonts.large = love.graphics.newFont(32)
    
    -- Load font from file (if you have a .ttf file)
    -- fonts.custom = love.graphics.newFont("assets/fonts/custom.ttf", 24)
end

function love.draw()
    -- Small font
    love.graphics.setFont(fonts.small)
    love.graphics.print("Small font (12px)", 10, 10)
    
    -- Medium font
    love.graphics.setFont(fonts.medium)
    love.graphics.print("Medium font (18px)", 10, 40)
    
    -- Large font
    love.graphics.setFont(fonts.large)
    love.graphics.print("Large font (32px)", 10, 80)
    
    -- Text with word wrapping
    love.graphics.setFont(fonts.medium)
    local text = "This is a long line of text that will be wrapped to fit within the specified width."
    love.graphics.printf(text, 10, 150, 300, "left") -- Wrap at 300 pixels
    
    -- Centered text
    love.graphics.printf("Centered Text", 0, 300, love.graphics.getWidth(), "center")
    
    -- Right-aligned text
    love.graphics.printf("Right-aligned", 0, 330, love.graphics.getWidth() - 20, "right")
end
```

### Text Effects

```lua
local time = 0

function love.update(dt)
    time = time + dt
end

function love.draw()
    -- Rainbow text
    local text = "RAINBOW TEXT"
    for i = 1, string.len(text) do
        local char = string.sub(text, i, i)
        local hue = (time + i * 0.1) % 1
        local r, g, b = hslToRgb(hue, 1, 0.5)
        love.graphics.setColor(r, g, b)
        love.graphics.print(char, 50 + i * 20, 50)
    end
    
    -- Waving text
    love.graphics.setColor(1, 1, 1)
    local waveText = "WAVE EFFECT"
    for i = 1, string.len(waveText) do
        local char = string.sub(waveText, i, i)
        local wave = math.sin(time * 3 + i * 0.5) * 10
        love.graphics.print(char, 50 + i * 20, 150 + wave)
    end
    
    -- Pulsing text
    local pulse = math.abs(math.sin(time * 2))
    local scale = 1 + pulse * 0.5
    love.graphics.setColor(1, pulse, pulse)
    love.graphics.print("PULSING TEXT", 200, 250, 0, scale, scale)
    
    love.graphics.setColor(1, 1, 1) -- Reset
end

-- Helper function to convert HSL to RGB
function hslToRgb(h, s, l)
    local r, g, b
    if s == 0 then
        r, g, b = l, l, l
    else
        local function hue2rgb(p, q, t)
            if t < 0 then t = t + 1 end
            if t > 1 then t = t - 1 end
            if t < 1/6 then return p + (q - p) * 6 * t end
            if t < 1/2 then return q end
            if t < 2/3 then return p + (q - p) * (2/3 - t) * 6 end
            return p
        end
        
        local q = l < 0.5 and l * (1 + s) or l + s - l * s
        local p = 2 * l - q
        r = hue2rgb(p, q, h + 1/3)
        g = hue2rgb(p, q, h)
        b = hue2rgb(p, q, h - 1/3)
    end
    return r, g, b
end
```

## Graphics State Management

### Transformations

```lua
function love.draw()
    -- Save the current transformation
    love.graphics.push()
    
    -- Translate (move the origin)
    love.graphics.translate(200, 200)
    love.graphics.setColor(1, 0, 0)
    love.graphics.rectangle("fill", 0, 0, 50, 50) -- Drawn at (200, 200)
    
    -- Rotate around the new origin
    love.graphics.rotate(math.pi / 4) -- 45 degrees
    love.graphics.setColor(0, 1, 0)
    love.graphics.rectangle("fill", 60, 0, 50, 50) -- Green rotated rectangle
    
    -- Scale
    love.graphics.scale(1.5, 1.5)
    love.graphics.setColor(0, 0, 1)
    love.graphics.rectangle("fill", 80, 0, 30, 30) -- Blue scaled rectangle
    
    -- Restore the previous transformation
    love.graphics.pop()
    
    -- This rectangle is drawn in the original coordinate system
    love.graphics.setColor(1, 1, 0)
    love.graphics.rectangle("fill", 400, 100, 50, 50)
    
    -- Multiple transformations example
    love.graphics.push()
    love.graphics.translate(400, 300)
    love.graphics.rotate(love.timer.getTime()) -- Rotate based on time
    love.graphics.setColor(1, 0, 1)
    love.graphics.rectangle("fill", -25, -25, 50, 50) -- Centered on origin
    love.graphics.pop()
end
```

### Blend Modes

```lua
function love.draw()
    -- Default blend mode (alpha)
    love.graphics.setColor(1, 0, 0, 0.7)
    love.graphics.circle("fill", 150, 150, 60)
    
    love.graphics.setColor(0, 1, 0, 0.7)
    love.graphics.circle("fill", 200, 150, 60)
    
    -- Additive blending (colors add together)
    love.graphics.setBlendMode("add")
    love.graphics.setColor(0, 0, 1, 0.7)
    love.graphics.circle("fill", 175, 200, 60)
    
    love.graphics.setColor(1, 1, 0, 0.7)
    love.graphics.circle("fill", 225, 200, 60)
    
    -- Multiply blending (colors multiply together)
    love.graphics.setBlendMode("multiply")
    love.graphics.setColor(1, 0.5, 0.5)
    love.graphics.circle("fill", 350, 150, 80)
    
    love.graphics.setColor(0.5, 1, 0.5)
    love.graphics.circle("fill", 400, 150, 80)
    
    -- Screen blending
    love.graphics.setBlendMode("screen")
    love.graphics.setColor(0.8, 0.2, 0.8)
    love.graphics.circle("fill", 500, 150, 60)
    
    love.graphics.setColor(0.8, 0.8, 0.2)
    love.graphics.circle("fill", 550, 150, 60)
    
    -- Reset to default
    love.graphics.setBlendMode("alpha")
    love.graphics.setColor(1, 1, 1)
    
    -- Labels
    love.graphics.print("Alpha", 120, 250)
    love.graphics.print("Add", 350, 250)
    love.graphics.print("Multiply", 320, 100)
    love.graphics.print("Screen", 500, 250)
end
```

## Drawing Optimization

### Batching Draw Calls

```lua
local sprites = {}
local particles = {}

function love.load()
    sprites.particle = love.graphics.newImage("assets/images/particle.png")
    
    -- Create many particles
    for i = 1, 1000 do
        table.insert(particles, {
            x = math.random(0, 800),
            y = math.random(0, 600),
            vx = math.random(-50, 50),
            vy = math.random(-50, 50),
            life = math.random(1, 3),
            maxLife = math.random(1, 3)
        })
    end
end

function love.update(dt)
    for _, particle in ipairs(particles) do
        particle.x = particle.x + particle.vx * dt
        particle.y = particle.y + particle.vy * dt
        particle.life = particle.life - dt
        
        if particle.life <= 0 then
            particle.life = particle.maxLife
            particle.x = math.random(0, 800)
            particle.y = math.random(0, 600)
        end
    end
end

function love.draw()
    -- Batch similar draw operations
    love.graphics.setColor(1, 1, 1, 1)
    
    -- Draw all particles with the same sprite in one go
    for _, particle in ipairs(particles) do
        local alpha = particle.life / particle.maxLife
        love.graphics.setColor(1, 1, 1, alpha)
        love.graphics.draw(sprites.particle, particle.x, particle.y)
    end
    
    love.graphics.setColor(1, 1, 1, 1)
    love.graphics.print("Particles: " .. #particles, 10, 10)
    love.graphics.print("FPS: " .. love.timer.getFPS(), 10, 30)
end
```

### Using SpriteBatch for Performance

```lua
local spriteBatch
local particleImage

function love.load()
    particleImage = love.graphics.newImage("assets/images/particle.png")
    
    -- Create a SpriteBatch for efficient drawing of many instances
    spriteBatch = love.graphics.newSpriteBatch(particleImage, 1000)
end

function love.update(dt)
    -- Clear the sprite batch
    spriteBatch:clear()
    
    -- Add sprites to the batch
    for i = 1, 500 do
        local x = 400 + math.cos(love.timer.getTime() + i * 0.1) * (100 + i * 0.5)
        local y = 300 + math.sin(love.timer.getTime() + i * 0.1) * (100 + i * 0.5)
        local rotation = love.timer.getTime() + i * 0.2
        local scale = 0.5 + math.sin(love.timer.getTime() * 2 + i * 0.05) * 0.3
        
        spriteBatch:add(x, y, rotation, scale, scale)
    end
end

function love.draw()
    -- Draw entire batch in one call
    love.graphics.draw(spriteBatch)
    
    love.graphics.print("Drawing 500 sprites efficiently with SpriteBatch", 10, 10)
    love.graphics.print("FPS: " .. love.timer.getFPS(), 10, 30)
end
```

## Advanced Graphics Techniques

### Creating Procedural Graphics

```lua
function love.draw()
    -- Procedural star field
    love.graphics.setColor(1, 1, 1)
    math.randomseed(42) -- Fixed seed for consistent stars
    for i = 1, 200 do
        local x = math.random(0, 800)
        local y = math.random(0, 600)
        local brightness = math.random(0.3, 1)
        love.graphics.setColor(brightness, brightness, brightness)
        love.graphics.circle("fill", x, y, math.random(1, 2))
    end
    
    -- Procedural grid pattern
    love.graphics.setColor(0.2, 0.2, 0.3, 0.5)
    for x = 0, 800, 40 do
        love.graphics.line(x, 0, x, 600)
    end
    for y = 0, 600, 40 do
        love.graphics.line(0, y, 800, y)
    end
    
    -- Procedural wave
    love.graphics.setColor(0, 0.8, 1)
    love.graphics.setLineWidth(3)
    local points = {}
    for x = 0, 800, 5 do
        local y = 300 + math.sin(x * 0.02 + love.timer.getTime() * 2) * 50
        table.insert(points, x)
        table.insert(points, y)
    end
    love.graphics.line(points)
    love.graphics.setLineWidth(1)
    
    love.graphics.setColor(1, 1, 1)
end
```

### Drawing Performance Tips

```lua
-- Tips demonstrated in code:

function love.draw()
    -- 1. Minimize color changes
    love.graphics.setColor(1, 0, 0)
    -- Draw all red objects here
    love.graphics.rectangle("fill", 50, 50, 50, 50)
    love.graphics.circle("fill", 150, 75, 25)
    
    -- 2. Batch similar operations
    love.graphics.setColor(0, 1, 0)
    -- Draw all green objects here
    love.graphics.rectangle("fill", 250, 50, 50, 50)
    love.graphics.circle("fill", 350, 75, 25)
    
    -- 3. Use appropriate drawing functions
    -- For filled shapes, use "fill" mode
    -- For outlines, use "line" mode
    
    -- 4. Avoid unnecessary transformations
    -- Group transformed objects together
    love.graphics.push()
    love.graphics.translate(400, 300)
    love.graphics.rotate(love.timer.getTime())
    
    love.graphics.setColor(1, 1, 0)
    love.graphics.rectangle("fill", -25, -25, 50, 50)
    love.graphics.circle("fill", 0, -50, 15)
    
    love.graphics.pop()
    
    love.graphics.setColor(1, 1, 1)
end

-- Performance monitoring
function love.update(dt)
    -- Monitor frame time
    if dt > 1/30 then -- If frame took longer than ~33ms
        print("Frame lag detected: " .. dt .. "s")
    end
end
```

## Common Graphics Pitfalls

### Avoiding Common Mistakes

```lua
function love.draw()
    -- MISTAKE: Not resetting graphics state
    -- love.graphics.setColor(1, 0, 0)
    -- love.graphics.rectangle("fill", 50, 50, 50, 50)
    -- -- Text will be red too!
    -- love.graphics.print("This text is red!", 10, 10)
    
    -- CORRECT: Always reset state
    love.graphics.setColor(1, 0, 0)
    love.graphics.rectangle("fill", 50, 50, 50, 50)
    love.graphics.setColor(1, 1, 1) -- Reset to white
    love.graphics.print("This text is white", 10, 10)
    
    -- MISTAKE: Forgetting coordinate system
    -- Drawing at (0, 0) might be off-screen or hard to see
    
    -- CORRECT: Consider your coordinate system
    local centerX = love.graphics.getWidth() / 2
    local centerY = love.graphics.getHeight() / 2
    love.graphics.circle("fill", centerX, centerY, 30)
    
    -- MISTAKE: Not handling window resize
    -- Hard-coded positions break when window size changes
    
    -- CORRECT: Use relative positioning
    local screenWidth = love.graphics.getWidth()
    local screenHeight = love.graphics.getHeight()
    love.graphics.rectangle("fill", screenWidth - 60, 10, 50, 30) -- Top-right corner
end
```

## Next Steps

You now have a solid foundation in Love2D's graphics system. In the next chapter, we'll explore input handling in detail, including keyboard, mouse, and gamepad input, along with creating responsive controls for your games.

---

**Chapter Summary:**
- Love2D uses a coordinate system with (0,0) at top-left
- Basic shapes include circles, rectangles, lines, and polygons
- Colors use RGB values from 0 to 1, with optional alpha channel
- Images can be loaded, scaled, rotated, and animated
- Text rendering supports custom fonts and formatting
- Graphics transformations allow complex positioning and effects
- Blend modes create interesting visual effects
- Performance optimization techniques improve frame rates
- SpriteBatch enables efficient drawing of many similar objects
- Proper graphics state management prevents common issues
