# Love2D (Lua)

## Basic file

```lua
_G.love = require("love")

function love.load()
  -- needs to be global
  _G.number = 0
end

-- runs at 60 Hz
-- dt is delta time
function love.update(dt)
  number = number + 1
end

function love.draw()
  love.graphics.print(number)
end
```

## Keys

```lua
love.keyboard.isDown("a")
```

