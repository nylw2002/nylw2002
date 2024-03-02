 import turtle
from random import randint
import math

# Set screen
win = turtle.Screen()
win.bgcolor('#2d3436')
win.title('simpleGame')

# Frame
win.tracer(1, delay=2)

# Bounds
bound = turtle.Turtle()
bound.penup()
bound.setposition(-270, -270)
bound.color('white')
bound.pensize(5)
bound.pendown()
bound.speed(0)

for i in range(4):
    bound.forward(540)
    bound.left(90)
bound.hideturtle()

# Player
player = turtle.Turtle()
player.shape('triangle')
player.speed(0)
player.color('#00cec9')
player.penup()

# Goals
goals = []

for i in range(5):
    goals.append(turtle.Turtle())
    goals[i].shape('circle')
    goals[i].color('#ff7979')
    goals[i].penup()
    goals[i].speed(0)
    goals[i].setpos(randint(-255, 255), randint(-255, 255))

f = 1

def TurnLeft():
    player.left(30)

def TurnRight():
    player.right(30)

def SpeedUp():
    global f
    if f > 4:
        pass
    else:
        f += 1

def SpeedDown():
    global f
    if f < 2:
        pass
    else:
        f -= 1

def isDis(t1, t2):
    dis = math.sqrt(pow(t1.xcor() - t2.xcor(), 2) + pow(t1.ycor() - t2.ycor(), 2))
    if dis < 15:
        return True
    else:
        return False

# Keyboard
win.listen()
win.onkeypress(TurnLeft, 'Left')
win.onkeypress(TurnRight, 'Right')
win.onkeypress(SpeedUp, 'Up')
win.onkeypress(SpeedDown, 'Down')

# Var
score = 0

while True:
    player.forward(f)

    # Bond condition
    if player.xcor() > 260 or player.xcor() < -260:
        player.right(180)
    if player.ycor() > 260 or player.ycor() < -260:
        player.right(180)

    # Move goals within bounds
    for i in range(5):
        goals[i].forward(2)

        # Goals bounds
        if goals[i].xcor() > 260 or goals[i].xcor() < -260:
            goals[i].right(180)
        if goals[i].ycor() > 260 or goals[i].ycor() < -260:
            goals[i].right(180)

        

    # Distance
    for i in range(5):
        if isDis(player, goals[i]):
            goals[i].setpos(randint(-255, 255), randint(-255, 255))
            goals[i].right(randint(5, 255))

            # Score
            score += 1
            # Update
            bound.undo()
            bound.hideturtle()
            bound.penup()
            bound.setpos(-275, 275)
            showscore = f"score: {score}"

            bound.write(showscore, False, font=('sans-serif', 14, 'normal'))
            # Move
            win.update()
