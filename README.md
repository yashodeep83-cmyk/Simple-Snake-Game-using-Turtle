# 🐍 Simple Snake Game using Turtle (50 lines)

import turtle
import time
import random

delay = 0.1
score = 0

# Screen setup
win = turtle.Screen()
win.title("🐍 Snake Game")
win.bgcolor("black")
win.setup(width=600, height=600)
win.tracer(0)

# Snake head
head = turtle.Turtle()
head.shape("square")
head.color("lime")
head.penup()
head.goto(0, 0)
head.direction = "stop"

# Food
food = turtle.Turtle()
food.shape("circle")
food.color("red")
food.penup()
food.goto(0, 100)

segments = []

# Movement functions
def go_up():    head.direction = "up"
def go_down():  head.direction = "down"
def go_left():  head.direction = "left"
def go_right(): head.direction = "right"

win.listen()
win.onkeypress(go_up, "w")
win.onkeypress(go_down, "s")
win.onkeypress(go_left, "a")
win.onkeypress(go_right, "d")

# Main game loop
while True:
    win.update()

    # Border collision
    if head.xcor()>290 or head.xcor()<-290 or head.ycor()>290 or head.ycor()<-290:
        time.sleep(1)
        head.goto(0,0)
        head.direction = "stop"
        for s in segments: s.goto(1000,1000)
        segments.clear()
        score = 0

    # Move head
    if head.direction == "up": head.sety(head.ycor()+20)
    if head.direction == "down": head.sety(head.ycor()-20)
    if head.direction == "left": head.setx(head.xcor()-20)
    if head.direction == "right": head.setx(head.xcor()+20)

    # Food collision
    if head.distance(food) < 20:
        x = random.randint(-290, 290)
        y = random.randint(-290, 290)
        food.goto(x, y)
        new_segment = turtle.Turtle()
        new_segment.shape("square")
        new_segment.color("green")
        new_segment.penup()
        segments.append(new_segment)
        score += 10

    # Move segments
    for i in range(len(segments)-1, 0, -1):
        x = segments[i-1].xcor()
        y = segments[i-1].ycor()
        segments[i].goto(x, y)
    if len(segments)>0:
        segments[0].goto(head.xcor(), head.ycor())

    # Self-collision
    for s in segments:
        if s.distance(head) < 20:
            time.sleep(1)
            head.goto(0,0)
            head.direction = "stop"
            for seg in segments: seg.goto(1000,1000)
            segments.clear()
            score = 0

    time.sleep(delay)

win.mainloop()
