# snake-game

#Import the Modules
import random
import turtle as t

t.bgcolor('yellow')
t.title('Snake Game')

# a Turtle for the Hungry Snake
snake = t.Turtle()
snake.shape('square')
snake.color('red')
snake.speed(0)
snake.penup()
snake.hideturtle()

#Food for the Snake
food = t.Turtle()
food.color('green')
food.shape('square')
food.speed(0)
food.penup()
food.hideturtle()

#Text on the Screen
welcome_text = t.Turtle()
welcome_text.write('Press SPACE to Start', align='center', font=('Helvetica', 20, 'bold'))
welcome_text.hideturtle()
score_text = t.Turtle()
score_text.hideturtle()

#Game Over
def game_over():
    snake.color('yellow')
    food.color('yellow')
    t.penup()
    t.hideturtle()
    t.write('GAME OVER!', align='center', font=('Helvetica', 40, 'bold'))

#The Boundary
def boundary():
    left_wall = -t.window_width() / 2
    right_wall = t.window_width() / 2
    top_wall = t.window_height() / 2
    bottom_wall = -t.window_height() / 2

    (x, y) = snake.pos()
    boundary = (x<=left_wall or x>=right_wall or y<=bottom_wall or y>=top_wall)
    return boundary
#Display the Score
def display_score(current_score):
    score_text.clear()
    score_text.penup()
    x = (t.window_width() / 2) - 50
    y = (t.window_height() / 2) - 50
    score_text.setpos(x, y)
    score_text.write(str(current_score), align='right',
    font=('Helvetica', 30, 'bold'))

#Place Food
def place_food():
    # Hide Turtle
    food.ht()
    food.setx(random.randint(-150, 150))
    food.sety(random.randint(-150, 150))
    # Show Turtle
    food.st()    

#Start the Game
def start_game():
    score = 0
    # Clear the Starting Screen
    welcome_text.clear()

    snake_speed = 1
    snake_length = 2
    snake.shapesize(1, snake_length, 1)
    snake.showturtle()
    display_score(score)
    place_food()
    while True:
        snake.forward(snake_speed)
        # The snake eats the food when it is less than 30 pixels away
        if snake.distance(food) < 30:
            place_food()
            snake_length += 0.5
            snake.shapesize(1, snake_length, 1)
            snake_speed += 0.4
            score += 2
            display_score(score)
        if boundary():
            game_over()
            break

#Controller Functions
def go_left():
    if snake.heading() == 90 or snake.heading() == 270:
        # The head of the snake is being set at a 180 degree angle.
        snake.setheading(180)

def go_right():
    if snake.heading() == 90 or snake.heading() == 270:
        snake.setheading(0)

def go_up():
    if snake.heading() == 0 or snake.heading() == 180:
        snake.setheading(90)

def go_down():
    if snake.heading() == 0 or snake.heading() == 180:
        snake.setheading(270)

#Listening the Key Presses
t.onkey(start_game, 'space')
t.onkey(go_up, 'Up')
t.onkey(go_right, 'Right')
t.onkey(go_down, 'Down')
t.onkey(go_left, 'Left')
t.listen()
t.mainloop()
