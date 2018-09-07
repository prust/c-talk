# red-planet.c
Red Planet game engine, implemented in C using SDL, scriptable via cTalk

Example breakout clone:

### Setup

```applescript
set lives to 3
  
-- create paddle in the middle/bottom of the screen
set paddle to new object (width: 70, height: 30, color: green)
set paddle.x to screen.width / 2 - paddle.width / 2
set paddle.y to screen.height - paddle.height
set paddle.speed to 10

-- create 30 bricks scattered around the top half of the screen
set num_bricks_per_screen_w = screen.width / brick.width
set num_bricks_per_screen_h = screen.height / brick.height
set bricks to new collection (size: 30)
repeat 30x:
  set brick to new object (width: 20, height: 10, color: blue)
  set brick.x to random (max: num_bricks_per_screen_w) * brick.width
  set brick.y to random (max: num_bricks_per_screen_h / 2) * brick.height
  put brick in bricks

-- create a ball dropping at 45 degree angle
set ball to new object (width: 5, height: 5, color: green, speed: 3)
set ball.delta_x to random (max: 2)
if ball.delta_x = 0 then:
  set ball.delta_x to -1
set ball.delta_y to 1
```

### Update

```applescript
if is pressed (key: 'left') then:
  set paddle.x to paddle.x - paddle.speed
if is pressed (key: 'right') then:
  set paddle.x to paddle.x + paddle.speed

-- if ball is outside the screen, move it in & bounce the direction (or lose a life)
if ball.x < 0 then:
  set ball.x to 0
  set ball.delta_x to -ball.delta_x
if ball.x > screen.width then:
  set ball.x to screen.width
  set ball.delta_x to -ball.delta_x
if ball.y < 0 then:
  set ball.y to 0
  set ball.delta_y to -ball.delta_y
if ball.y > screen.height then:
  set ball.y to 0
  set lives to lives - 1
  if lives = 0 then:
    game over

-- bounce the ball off the paddle
if collide (object: ball, with: paddle) then:
  bounce (object: ball, off: paddle)
  
-- bounce the ball off bricks
for each brick in bricks:
  if collide (object: ball, with: brick) then:
    bounce (object: ball, off: brick)
    destroy (object: brick)
    if count (collection: bricks) = 0 then:
      game over
```
