from ast import FloorDiv
from operator import index
import turtle
import time
import random

delay = 0.1

score = 0
high_score = 0

# set up screen
wn = turtle.Screen()
wn.title("snake game")
wn.bgcolor("green")
wn.setup(width=400,height=400)
wn.tracer(0) #turns of scrren update


# snake head

head= turtle.Turtle()
head.speed(0)
head.shape("square")
head.color("red")
head.penup()
head.goto(0,0)
head.direction = "stop"

# food

food = turtle.Turtle()
food.speed(0)
food.shape("circle")
food.color("white")
food.penup()
food.goto(0,200)

segements = []

# pen
pen = turtle.Turtle()
pen.speed(0)
pen.shape("square")
pen.penup()
pen.hideturtle()
pen.goto(0,310)
pen.write("score: 0  high score: 0", align="center", font=("Courier",24,"normal"))




# functions
def move():
    if head.direction == "up":
        y = head.ycor()
        head.sety(y + 20)

    if head.direction == "down":
        y = head.ycor()
        head.sety(y - 20)

    if head.direction == "right":
        x = head.xcor()
        head.setx(x + 20)
    
    if head.direction == "left":
        x = head.xcor()
        head.setx(x - 20)

def go_up():
    if head.direction != "down":
        head.direction = "up"

def go_down():
    if head.direction != "up":
        head.direction = "down"

def go_left():
    if head.direction != "right":
        head.direction = "left"

def go_right():
    if head.direction != "left":
        head.direction = "right"

# keyboard binding

wn.listen()
wn.onkeypress(go_up, "w")
wn.onkeypress(go_down, "z")
wn.onkeypress(go_left, "a")
wn.onkeypress(go_right, "s")



# main gamelooop
while True:
    wn.update()

# collison with border
    if head.xcor() > 290  or head.xcor() < -290 or head.ycor() > 290 or head.ycor() < -290:
        time.sleep(1)
        head.goto(0,0)
        head.direction = "stop"

# hide segment
        for segment in segements:
            segment.goto(1000,1000)

# clear segemnt
        segements.clear()

#reset score
        score = 0
        
        pen.clear()
        pen.write("Score: {} High score {}".format(score,high_score),align="center",font=("Courior",24,"normal"))
        
        # reset delay
        
# collison
    if head.distance(food) < 20:
        x = random.randint(-290,290)
        y = random.randint(-290,290)
        food.goto(x,y)

        # add segment
        new_segment = turtle.Turtle()
        new_segment.speed(0)
        new_segment.shape("square")
        new_segment.color("white")
        new_segment.penup()
        segements.append(new_segment)

        

        # increase score
        score += 10

        if score > high_score:
            high_score = score
        
        pen.clear()
        pen.write("Score: {} High score {}".format(score,high_score),align="center",font=("Courior",24,"normal"))

    for index in range(len(segements)-1,0,-1):
        x = segements[index-1].xcor()
        y = segements[index-1].ycor()
        segements[index].goto(x,y)

    if len(segements) > 0:
        x = head.xcor()
        y = head.ycor()
        segements[0].goto(x,y)


    move()
    # check for body collison
    for segment in segements:
        if segment.distance(head) < 20:
            time.sleep(1)
            head.goto(0,0)
            head.direction = "stop"

            for segment in segements:
                segment.goto(1000,1000)
                #reset score
                score = 0
        
                pen.clear()
                pen.write("Score: {} High score {}".format(score,high_score),align="center",font=("Courior",24,"normal"))

                


# clear segemnt
            segements.clear()

            
    time.sleep(delay)
    



wn.mainloop()

