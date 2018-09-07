# red-planet.c
Red Planet game engine, implemented in C using SDL, scriptable via cTalk

Example breakout clone:

### Setup

```applescript
set player_lives to 3
  
-- create paddle in the middle/bottom of the screen
set paddle to new object (width: 70, height: 30, color: green)
set paddle.x to screen.width / 2 - paddle.width / 2
set paddle.y to screen.height - paddle.height
set paddle.speed to 10

-- create 30 bricks scattered around the top half of the screen
set num_bricks_per_screen_w = screen.width / brick.width
set num_bricks_per_screen_h = screen.height / brick.height
repeat 30x:
  set brick to new object (width: 20, height: 10, color: blue)
  set brick.x to random(num_bricks_per_screen_w) * brick.width
  set brick.y to random(num_bricks_per_screen_h / 2) * brick.height

-- create a ball dropping at 45 degree angle
set ball to new object (width: 5, height: 5, color: green, speed: 3)
set ball.delta_x to random(2)
if ball.delta_x = 0 then:
  set ball.delta_x to -1
set ball.delta_y to 1
```

### Update

```applescript
if keyIsPressed('left') then:
  set paddle.x to paddle.x - paddle.speed
if keyIsPressed('right') then:
  set paddle.x to paddle.x + paddle.speed

-- alternatively we could put walls/ceiling
-- otherwise we need to know which dimension to bounce the ball...
if ball.isOutside(screen) then:
  ball.moveInside(screen)


```
