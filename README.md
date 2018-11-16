# c-talk

A simple language, loosely in the HyperTalk family, designed for simplicity of implementation and embedding in C.

At this point, it is a largely imaginary language, but the below example should give an idea of what I would like the language to feel like.

Example breakout clone:

### Setup

```applescript
set lives to 3
  
-- create paddle in the middle/bottom of the screen
set paddle to new object (width: 70, height: 30, color: green)
set paddle.x to screen.width / 2 - paddle.width / 2
set paddle.y to screen.height - paddle.height
set paddle.speed to 10

-- create 30 bredricks scattered around the top half of the screen
set num_bricks_per_screen_w = screen.width / brick.width
set num_bricks_per_screen_h = screen.height / brick.height
set bricks to new collection (max size: 30)
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
if is_pressed 'left' then:
  set paddle.x to paddle.x - paddle.speed
if is_pressed 'right' then:
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
    end_game

-- bounce the ball off the paddle
if collide ball (with: paddle) then:
  bounce ball (off: paddle)
  
-- bounce the ball off bricks
for_each brick in bricks:
  if collide ball (with: brick) then:
    bounce ball (off: brick)
    destroy brick
    if (count bricks) = 0 then:
      end_game
```
