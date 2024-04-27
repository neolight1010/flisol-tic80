# Empty cart

```lua
-- title:   breakout
-- author:  neoligh1010
-- desc:    breakout clone
-- site:    https://github.com/neolight1010
-- license: MIT License
-- version: 0.1
-- script:  lua

function TIC()
end
```

# The basics

```lua
-- title:   breakout
-- author:  neoligh1010
-- desc:    breakout clone
-- site:    https://github.com/neolight1010
-- license: MIT License
-- version: 0.1
-- script:  lua

local radius = 5
local width = 5

function TIC()
 cls(5)
 print("Hello World!")
 
 radius = radius + 1
 circ(20, 20, radius, 7)
 
 -- 240x136
 width = width + 1
 rect(120, 6, width, 5, 3)
end
```

# Drawing the paddle

```lua
-- title:   breakout
-- author:  neoligh1010
-- desc:    breakout clone
-- site:    https://github.com/neolight1010
-- license: MIT License
-- version: 0.1
-- script:  lua

local SCR_HEIGHT = 136
local SCR_WIDTH = 240

function TIC()
 cls(0)
 drawPaddle()
end

function drawPaddle()
	local paddleHeight = 8
	local paddleWidth = 64
	local paddleX = SCR_WIDTH/2 - (paddleWidth/2)
	
	rect(paddleX, SCR_HEIGHT - paddleHeight, paddleWidth, paddleHeight, 2)
end
```

# Moving the paddle

```lua
-- title:   breakout
-- author:  neoligh1010
-- desc:    breakout clone
-- site:    https://github.com/neolight1010
-- license: MIT License
-- version: 0.1
-- script:  lua

local SCR_HEIGHT = 136
local SCR_WIDTH = 240
local RIGHT_BTN = 3
local LEFT_BTN = 2

local mainPaddle = {
 height = 8,
 width = 64,
 x = SCR_WIDTH/2 - (64/2)
}

-- TODO avoid moving past boundaries

function TIC()
 cls(0)
 
 drawPaddle(mainPaddle)
 
 if btn(RIGHT_BTN) then
  movePaddleRight(mainPaddle)
 end
 
 if btn(LEFT_BTN) then
  movePaddleLeft(mainPaddle)
 end
end

function movePaddleRight(paddle)
 paddle.x = paddle.x + 1
end

function movePaddleLeft(paddle)
 paddle.x = paddle.x - 1
end

function drawPaddle(paddle)
	rect(paddle.x, SCR_HEIGHT - paddle.height, paddle.width, paddle.height, 2)
end
```

# Draw ball

```lua
-- title:   breakout
-- author:  neoligh1010
-- desc:    breakout clone
-- site:    https://github.com/neolight1010
-- license: MIT License
-- version: 0.1
-- script:  lua

local SCR_HEIGHT = 136
local SCR_WIDTH = 240
local RIGHT_BTN = 3
local LEFT_BTN = 2

local mainPaddle = {
 height = 8,
 width = 64,
 x = SCR_WIDTH/2 - (64/2),
}

local mainBall = {
 x = SCR_WIDTH/2,
 y = SCR_HEIGHT/2,
}

-- TODO avoid moving past boundaries

function TIC()
 cls(0)
 
 drawPaddle(mainPaddle)
 drawBall(mainBall)
 
 if btn(RIGHT_BTN) then
  movePaddleRight(mainPaddle)
 end
 
 if btn(LEFT_BTN) then
  movePaddleLeft(mainPaddle)
 end
end

function movePaddleRight(paddle)
 paddle.x = paddle.x + 1
end

function movePaddleLeft(paddle)
 paddle.x = paddle.x - 1
end

function drawPaddle(paddle)
	rect(paddle.x, SCR_HEIGHT - paddle.height, paddle.width, paddle.height, 2)
end

-- ball

function drawBall(ball)
 circ(ball.x, ball.y, 5, 5) 
end
```

# Moving the ball (and paddle collision)

```lua
-- title:   breakout
-- author:  neoligh1010
-- desc:    breakout clone
-- site:    https://github.com/neolight1010
-- license: MIT License
-- version: 0.1
-- script:  lua

local SCR_HEIGHT = 136
local SCR_WIDTH = 240
local RIGHT_BTN = 3
local LEFT_BTN = 2

local mainPaddle = {
 height = 8,
 width = 64,
 x = SCR_WIDTH/2 - (64/2),
 y = SCR_HEIGHT - 8,
}

local mainBall = {
 x = SCR_WIDTH/2,
 y = SCR_HEIGHT/2,
 velY = 1,
}

-- TODO avoid moving past boundaries

function TIC()
 cls(0)
 
 updateBall(mainBall, mainPaddle)
 
 drawPaddle(mainPaddle)
 drawBall(mainBall)
 
 if btn(RIGHT_BTN) then
  movePaddleRight(mainPaddle)
 end
 
 if btn(LEFT_BTN) then
  movePaddleLeft(mainPaddle)
 end
end

function movePaddleRight(paddle)
 paddle.x = paddle.x + 1
end

function movePaddleLeft(paddle)
 paddle.x = paddle.x - 1
end

function drawPaddle(paddle)
	rect(paddle.x, paddle.y, paddle.width, paddle.height, 2)
end

-- ball

function drawBall(ball)
 circ(ball.x, ball.y, 5, 5) 
end

function updateBall(ball, paddle)
 ball.x = ball.x + 1
 ball.y = ball.y + ball.velY
 
 local collidedY =
  ball.y >= paddle.y and
  ball.y <= paddle.y + height
 local collidedX =
  ball.x >= paddle.x and
  ball.x <= paddle.x + paddle.width
 
 if collidedY and collidedX then
  ball.velY = ball.velY * -1
 end
end
```

# Wall and ceiling collisions

```lua
-- title:   breakout
-- author:  neoligh1010
-- desc:    breakout clone
-- site:    https://github.com/neolight1010
-- license: MIT License
-- version: 0.1
-- script:  lua

local SCR_HEIGHT = 136
local SCR_WIDTH = 240
local RIGHT_BTN = 3
local LEFT_BTN = 2

local mainPaddle = {
 height = 8,
 width = 64,
 x = SCR_WIDTH/2 - (64/2),
 y = SCR_HEIGHT - 8,
}

local mainBall = {
 x = SCR_WIDTH - 50,
 y = 0,
 velX = -1,
 velY = 1,
}

-- TODO avoid moving past boundaries

function TIC()
 cls(0)
 
 updateBall(mainBall, mainPaddle)
 
 drawPaddle(mainPaddle)
 drawBall(mainBall)
 
 if btn(RIGHT_BTN) then
  movePaddleRight(mainPaddle)
 end
 
 if btn(LEFT_BTN) then
  movePaddleLeft(mainPaddle)
 end
end

function movePaddleRight(paddle)
 paddle.x = paddle.x + 1
end

function movePaddleLeft(paddle)
 paddle.x = paddle.x - 1
end

function drawPaddle(paddle)
	rect(paddle.x, paddle.y, paddle.width, paddle.height, 2)
end

-- ball

function drawBall(ball)
 circ(ball.x, ball.y, 5, 5) 
end

function updateBall(ball, paddle)
 ball.x = ball.x + ball.velX
 ball.y = ball.y + ball.velY
 
 if ballCollidedWithAnyWall(ball) then
  ball.velX = ball.velX * -1
 end
 
 if (
  ballCollidedWithPaddle(ball, paddle) or
  ballCollidedWithCeiling(ball)
 ) then
  ball.velY = ball.velY * -1
 end
end

function ballCollidedWithCeiling(ball)
 return ball.y <= 0
end

function ballCollidedWithAnyWall(ball)
 return
  ballCollidedWithLeftWall(ball) or
  ballCollidedWithRightWall(ball)
end

function ballCollidedWithLeftWall(ball)
 return ball.x <= 0
end

function ballCollidedWithRightWall(ball)
 return ball.x >= SCR_WIDTH
end

function ballCollidedWithPaddle(ball, paddle)
 local collidedY =
  ball.y >= paddle.y and
  ball.y <= paddle.y + height
 local collidedX =
  ball.x >= paddle.x and
  ball.x <= paddle.x + paddle.width
  
 return collidedX and collidedY
end
```

# Drawing bricks

```lua
-- title:   breakout
-- author:  neoligh1010
-- desc:    breakout clone
-- site:    https://github.com/neolight1010
-- license: MIT License
-- version: 0.1
-- script:  lua

local SCR_HEIGHT = 136
local SCR_WIDTH = 240
local RIGHT_BTN = 3
local LEFT_BTN = 2

local mainPaddle = {
 height = 8,
 width = 64,
 x = SCR_WIDTH/2 - (64/2),
 y = SCR_HEIGHT - 8,
}

local mainBall = {
 x = SCR_WIDTH - 50,
 y = 0,
 velX = -1,
 velY = 1,
}

-- TODO autogenerate
local bricks = {
 { x = 0 + 64, y = 32, width = 32, height = 8, color = 9 },
 { x = 42 + 64, y = 32, width = 32, height = 8, color = 9 },
 { x = 84 + 64, y = 32, width = 32, height = 8, color = 9 },
}

-- TODO avoid moving paddle past boundaries

function TIC()
 cls(0)
 
 updateBall(mainBall, mainPaddle)
 
 drawPaddle(mainPaddle)
 drawBall(mainBall)
 
 for _, brick in ipairs(bricks) do
	 drawBrick(brick)
 end
 
 if btn(RIGHT_BTN) then
  movePaddleRight(mainPaddle)
 end
 
 if btn(LEFT_BTN) then
  movePaddleLeft(mainPaddle)
 end
end

function movePaddleRight(paddle)
 paddle.x = paddle.x + 1
end

function movePaddleLeft(paddle)
 paddle.x = paddle.x - 1
end

function drawPaddle(paddle)
	rect(paddle.x, paddle.y, paddle.width, paddle.height, 2)
end

-- ball

function drawBall(ball)
 circ(ball.x, ball.y, 5, 5) 
end

function updateBall(ball, paddle)
 ball.x = ball.x + ball.velX
 ball.y = ball.y + ball.velY
 
 if ballCollidedWithAnyWall(ball) then
  ball.velX = ball.velX * -1
 end
 
 if (
  ballCollidedWithPaddle(ball, paddle) or
  ballCollidedWithCeiling(ball)
 ) then
  ball.velY = ball.velY * -1
 end
end

function ballCollidedWithCeiling(ball)
 return ball.y <= 0
end

function ballCollidedWithAnyWall(ball)
 return
  ballCollidedWithLeftWall(ball) or
  ballCollidedWithRightWall(ball)
end

function ballCollidedWithLeftWall(ball)
 return ball.x <= 0
end

function ballCollidedWithRightWall(ball)
 return ball.x >= SCR_WIDTH
end

function ballCollidedWithPaddle(ball, paddle)
 local collidedY =
  ball.y >= paddle.y and
  ball.y <= paddle.y + height
 local collidedX =
  ball.x >= paddle.x and
  ball.x <= paddle.x + paddle.width
  
 return collidedX and collidedY
end

-- brick

function drawBrick(brick)
 rect(brick.x, brick.y, brick.width, brick.height, brick.color)
end
```

# Block collision

```lua
-- title:   breakout
-- author:  neoligh1010
-- desc:    breakout clone
-- site:    https://github.com/neolight1010
-- license: MIT License
-- version: 0.1
-- script:  lua

local SCR_HEIGHT = 136
local SCR_WIDTH = 240
local RIGHT_BTN = 3
local LEFT_BTN = 2

local mainPaddle = {
 height = 8,
 width = 64,
 x = SCR_WIDTH/2 - (64/2),
 y = SCR_HEIGHT - 8,
}

local mainBall = {
 x = SCR_WIDTH - 50,
 y = 0,
 velX = -1,
 velY = 1,
}

-- TODO autogenerate
local bricks = {
 { x = 0 + 64, y = 32, width = 32, height = 8, color = 9 },
 { x = 42 + 64, y = 32, width = 32, height = 8, color = 9 },
 { x = 84 + 64, y = 32, width = 32, height = 8, color = 9 },
}

-- TODO avoid moving paddle past boundaries

function TIC()
 cls(0)
 
 updateBall(mainBall, mainPaddle, bricks)
 
 drawPaddle(mainPaddle)
 drawBall(mainBall)
 
 for _, brick in ipairs(bricks) do
	 drawBrick(brick)
 end
 
 if btn(RIGHT_BTN) then
  movePaddleRight(mainPaddle)
 end
 
 if btn(LEFT_BTN) then
  movePaddleLeft(mainPaddle)
 end
end

function movePaddleRight(paddle)
 paddle.x = paddle.x + 1
end

function movePaddleLeft(paddle)
 paddle.x = paddle.x - 1
end

function drawPaddle(paddle)
	rect(paddle.x, paddle.y, paddle.width, paddle.height, 2)
end

-- ball

function drawBall(ball)
 circ(ball.x, ball.y, 5, 5) 
end

-- TODO receive multiple bricks
function updateBall(ball, paddle, bricks)
 ball.x = ball.x + ball.velX
 ball.y = ball.y + ball.velY
 
 if ballCollidedWithAnyWall(ball) then
  ball.velX = ball.velX * -1
 end
 
 if (
  ballCollidedWithPaddle(ball, paddle) or
  ballCollidedWithCeiling(ball) or
  ballCollidedWithAnyBrick(ball, bricks)
 ) then
  ball.velY = ball.velY * -1
 end
end

function ballCollidedWithCeiling(ball)
 return ball.y <= 0
end

function ballCollidedWithAnyWall(ball)
 return
  ballCollidedWithLeftWall(ball) or
  ballCollidedWithRightWall(ball)
end

function ballCollidedWithLeftWall(ball)
 return ball.x <= 0
end

function ballCollidedWithRightWall(ball)
 return ball.x >= SCR_WIDTH
end

function ballCollidedWithPaddle(ball, paddle)
 local collidedY =
  ball.y >= paddle.y and
  ball.y <= paddle.y + paddle.height
 local collidedX =
  ball.x >= paddle.x and
  ball.x <= paddle.x + paddle.width
  
 return collidedX and collidedY
end

function ballCollidedWithAnyBrick(ball, bricks)
 for _, brick in ipairs(bricks) do
  if ballCollidedWithBrick(ball, brick) then
   return true
  end
 end
 
 return false
end

function ballCollidedWithBrick(ball, brick)
 local collidedY =
  ball.y >= brick.y and
  ball.y <= brick.y + brick.height
 local collidedX =
  ball.x >= brick.x and
  ball.x <= brick.x + brick.width
  
 return collidedX and collidedY
end

-- brick

function drawBrick(brick)
 rect(brick.x, brick.y, brick.width, brick.height, brick.color)
end
```

# Block removal

```lua
-- title:   breakout
-- author:  neoligh1010
-- desc:    breakout clone
-- site:    https://github.com/neolight1010
-- license: MIT License
-- version: 0.1
-- script:  lua

local SCR_HEIGHT = 136
local SCR_WIDTH = 240
local RIGHT_BTN = 3
local LEFT_BTN = 2

local mainPaddle = {
 height = 8,
 width = 64,
 x = SCR_WIDTH/2 - (64/2),
 y = SCR_HEIGHT - 8,
}

local mainBall = {
 x = SCR_WIDTH - 50,
 y = 0,
 velX = -1,
 velY = 1,
}

-- TODO autogenerate
local bricks = {
 { x = 0 + 64, y = 32, width = 32, height = 8, color = 9 },
 { x = 42 + 64, y = 32, width = 32, height = 8, color = 9 },
 { x = 84 + 64, y = 32, width = 32, height = 8, color = 9 },
}

-- TODO avoid moving paddle past boundaries

function TIC()
 cls(0)
 
 updateBall(mainBall, mainPaddle, bricks)
 
 drawPaddle(mainPaddle)
 drawBall(mainBall)
 
 for _, brick in ipairs(bricks) do
	 drawBrick(brick)
 end
 
 if btn(RIGHT_BTN) then
  movePaddleRight(mainPaddle)
 end
 
 if btn(LEFT_BTN) then
  movePaddleLeft(mainPaddle)
 end
end

function movePaddleRight(paddle)
 paddle.x = paddle.x + 1
end

function movePaddleLeft(paddle)
 paddle.x = paddle.x - 1
end

function drawPaddle(paddle)
	rect(paddle.x, paddle.y, paddle.width, paddle.height, 2)
end

-- ball

function drawBall(ball)
 circ(ball.x, ball.y, 5, 5) 
end

-- TODO receive multiple bricks
function updateBall(ball, paddle, bricks)
 ball.x = ball.x + ball.velX
 ball.y = ball.y + ball.velY
 
 if ballCollidedWithAnyWall(ball) then
  ball.velX = ball.velX * -1
 end
 
 if (
  ballCollidedWithPaddle(ball, paddle) or
  ballCollidedWithCeiling(ball)
 ) then
  ball.velY = ball.velY * -1
 end
 
 local brickColliding = findBrickCollidingWithBall(ball, bricks)
 if brickColliding ~= nil then
  table.remove(bricks, brickColliding)
  ball.velY = ball.velY * -1
 end
end

function ballCollidedWithCeiling(ball)
 return ball.y <= 0
end

function ballCollidedWithAnyWall(ball)
 return
  ballCollidedWithLeftWall(ball) or
  ballCollidedWithRightWall(ball)
end

function ballCollidedWithLeftWall(ball)
 return ball.x <= 0
end

function ballCollidedWithRightWall(ball)
 return ball.x >= SCR_WIDTH
end

function ballCollidedWithPaddle(ball, paddle)
 local collidedY =
  ball.y >= paddle.y and
  ball.y <= paddle.y + paddle.height
 local collidedX =
  ball.x >= paddle.x and
  ball.x <= paddle.x + paddle.width
  
 return collidedX and collidedY
end

function findBrickCollidingWithBall(ball, bricks)
 for index, brick in ipairs(bricks) do
  if ballCollidedWithBrick(ball, brick) then
   return index
  end
 end
 
 return nil
end

function ballCollidedWithBrick(ball, brick)
 local collidedY =
  ball.y >= brick.y and
  ball.y <= brick.y + brick.height
 local collidedX =
  ball.x >= brick.x and
  ball.x <= brick.x + brick.width
  
 return collidedX and collidedY
end

-- brick

function drawBrick(brick)
 rect(brick.x, brick.y, brick.width, brick.height, brick.color)
end
```
