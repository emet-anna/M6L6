Space Invader Project - Part 2
Let's continue with our capstone project of a space invader game to implement all the learnings of Pygame.

Overview
Activities
Outcome
Agenda
Overview
 

Topics introduced
 

Space Invader Game
Activity explanation
Topics in Details -
Space Invader Game
The Space Invaders project is a straightforward Python programme. This is a straightforward spacecraft shooting game. In this game, the player must move their shooting gun with the arrow keys and shoot it with the spacebar. For easier gameplay, there is a basic and clear GUI. The player must manoeuvre the gun and aim at the spaceship using the arrow keys. The game has a lot of potentials. So, let's get started with our journey of creating this amazing game.

Space Invader Game
 

Part 4: Game Loop - Detailed Explanations

 

1. Setting Up the Game Loop

 

# Game loop
running = True
while running:
    screen.fill((0, 0, 0))
    screen.blit(background, (0, 0))
 

running = True: Initializes a variable to keep the game loop running.
while running:: Starts the main game loop.
screen.fill((0, 0, 0)): Fills the screen with black color each frame.
screen.blit(background, (0, 0)): Draws the background image on the screen.


 

2. Event Handling

 

for event in pygame.event.get():
    if event.type == pygame.QUIT:
        running = False
    if event.type == pygame.KEYDOWN:
        if event.key == pygame.K_LEFT:
            playerX_change = -5
        if event.key == pygame.K_RIGHT:
            playerX_change = 5
        if event.key == pygame.K_SPACE and bullet_state == "ready":
            bulletX = playerX
            fire_bullet(bulletX, bulletY)
    if event.type == pygame.KEYUP and event.key in [pygame.K_LEFT, pygame.K_RIGHT]:
        playerX_change = 0
for event in pygame.event.get():: Processes all events in the event queue.
if event.type == pygame.QUIT:: Checks if the window's close button is clicked.
if event.type == pygame.KEYDOWN:: Detects if any key is pressed down.
if event.key == pygame.K_LEFT:: Checks if the left arrow key is pressed.
playerX_change = -5: Moves the player left.
if event.key == pygame.K_RIGHT:: Checks if the right arrow key is pressed.
playerX_change = 5: Moves the player right.
if event.key == pygame.K_SPACE and bullet_state == "ready":: Fires a bullet if the spacebar is pressed and the bullet is ready.
if event.type == pygame.KEYUP:: Checks if a key is released.
playerX_change = 0: Stops the player's movement when the key is released.


 

3. Player Movement

 

# Player Movement
playerX += playerX_change
playerX = max(0, min(playerX, SCREEN_WIDTH - 64))  # 64 is the size of the player
 

playerX += playerX_change: Updates the player's X-coordinate based on movement.
max(0, min(playerX, SCREEN_WIDTH - 64)): Ensures the player stays within the screen's horizontal boundaries.
 

4. Enemy Movement and Game Over Check

 

# Enemy Movement
for i in range(num_of_enemies):
    if enemyY[i] > 340:    # Game Over Condition
        for j in range(num_of_enemies):
            enemyY[j] = 2000
        game_over_text()
        break

    enemyX[i] += enemyX_change[i]
    if enemyX[i] <= 0 or enemyX[i] >= SCREEN_WIDTH - 64:
        enemyX_change[i] *= -1
        enemyY[i] += enemyY_change[i]
 

for i in range(num_of_enemies):: Iterates through each enemy.
if enemyY[i] > 340:: Checks if any enemy has crossed a specific Y-coordinate.
enemyY[j] = 2000: Moves all enemies off-screen in case of game over.
game_over_text(): Displays the game over message.
enemyX[i] += enemyX_change[i]: Updates each enemy's horizontal position.
if enemyX[i] <= 0 or enemyX[i] >= SCREEN_WIDTH - 64:: Checks if enemies hit the screen's edge.
enemyX_change[i] *= -1: Reverses the enemy's direction upon hitting an edge.
enemyY[i] += enemyY_change[i]: Moves the enemy down after changing direction.


 

5. Collision Check and Score Update
 

# Collision Check
if isCollision(enemyX[i], enemyY[i], bulletX, bulletY):
    bulletY = PLAYER_START_Y
    bullet_state = "ready"
    score_value += 1
    enemyX[i] = random.randint(0, SCREEN_WIDTH - 64)
    enemyY[i] = random.randint(ENEMY_START_Y_MIN, ENEMY_START_Y_MAX)

enemy(enemyX[i], enemyY[i], i)
if isCollision(enemyX[i], enemyY[i], bulletX, bulletY):: Checks for collisions between bullets and enemies.
bulletY = PLAYER_START_Y: Resets the bullet's position.
bullet_state = "ready": Makes the bullet ready to fire again.
score_value += 1: Increments the score upon hitting an enemy.
random.randint(0, SCREEN_WIDTH - 64): Randomly repositions the hit enemy horizontally.
random.randint(ENEMY_START_Y_MIN, ENEMY_START_Y_MAX): Randomly repositions the hit enemy vertically.
 

6. Bullet Movement

 

# Bullet Movement
if bulletY <= 0:
    bulletY = PLAYER_START_Y
    bullet_state = "ready"
elif bullet_state == "fire":
    fire_bullet(bulletX, bulletY)
    bulletY -= bulletY_change
if bulletY <= 0:: Checks if the bullet has moved off the top of the screen.
bulletY = PLAYER_START_Y: Resets the bullet to the player's Y-coordinate.
bullet_state = "ready": Makes the bullet ready to fire again.
elif bullet_state == "fire":: Checks if the bullet is in the "fire" state.
fire_bullet(bulletX, bulletY): Draws the bullet on the screen.
bulletY -= bulletY_change: Moves the bullet upwards.


 

7. Drawing Player, Score, and Updating Display

 

player(playerX, playerY)
show_score(textX, textY)
pygame.display.update()
player(playerX, playerY): Draws the player at its current position.
show_score(textX, textY): Displays the current score.
pygame.display.update(): Updates the entire screen with new changes.
 

 

 

Activity 1
Title
Space Invader Game
Short description:
Write a program to create a Space Invader Game using Pygame Library in Python.
Solution
Python Code: 

import math
import random
import pygame

# Constants
SCREEN_WIDTH = 800
SCREEN_HEIGHT = 500
PLAYER_START_X = 370
PLAYER_START_Y = 380
ENEMY_START_Y_MIN = 50
ENEMY_START_Y_MAX = 150
ENEMY_SPEED_X = 4
ENEMY_SPEED_Y = 40
BULLET_SPEED_Y = 10
COLLISION_DISTANCE = 27

# Initialize Pygame
pygame.init()

# Create the screen
screen = pygame.display.set_mode((SCREEN_WIDTH, SCREEN_HEIGHT))

# Background
background = pygame.image.load('background.png')

# Caption and Icon
pygame.display.set_caption("Space Invader")
icon = pygame.image.load('ufo.png')
pygame.display.set_icon(icon)

# Player
playerImg = pygame.image.load('player.png')
playerX = PLAYER_START_X
playerY = PLAYER_START_Y
playerX_change = 0

# Enemy
enemyImg = []
enemyX = []
enemyY = []
enemyX_change = []
enemyY_change = []
num_of_enemies = 6

for _i in range(num_of_enemies):
    enemyImg.append(pygame.image.load('enemy.png'))
    enemyX.append(random.randint(0, SCREEN_WIDTH - 64))  # 64 is the size of the enemy
    enemyY.append(random.randint(ENEMY_START_Y_MIN, ENEMY_START_Y_MAX))
    enemyX_change.append(ENEMY_SPEED_X)
    enemyY_change.append(ENEMY_SPEED_Y)

# Bullet
bulletImg = pygame.image.load('bullet.png')
bulletX = 0
bulletY = PLAYER_START_Y
bulletX_change = 0
bulletY_change = BULLET_SPEED_Y
bullet_state = "ready"

# Score
score_value = 0
font = pygame.font.Font('freesansbold.ttf', 32)
textX = 10
textY = 10

# Game Over Text
over_font = pygame.font.Font('freesansbold.ttf', 64)

def show_score(x, y):
    # Display the current score on the screen.
    score = font.render("Score : " + str(score_value), True, (255, 255, 255))
    screen.blit(score, (x, y))

def game_over_text():
    # Display the game over text
    over_text = over_font.render("GAME OVER", True, (255, 255, 255))
    screen.blit(over_text, (200, 250))

def player(x, y):
    # Draw the player on the screen
    screen.blit(playerImg, (x, y))

def enemy(x, y, i):
    # Draw an enemy on the screen
    screen.blit(enemyImg[i], (x, y))

def fire_bullet(x, y):
    # Fire a bullet from the player's position
    global bullet_state
    bullet_state = "fire"
    screen.blit(bulletImg, (x + 16, y + 10))

def isCollision(enemyX, enemyY, bulletX, bulletY):
    # Check if there is a collision between the enemy and a bullet
    distance = math.sqrt((enemyX - bulletX) ** 2 + (enemyY - bulletY) ** 2)
    return distance < COLLISION_DISTANCE

#Game loop
running = True
while running:
    screen.fill((0, 0, 0))
    screen.blit(background, (0, 0))

    for event in pygame.event.get():
      if event.type == pygame.QUIT:
          running = False
      if event.type == pygame.KEYDOWN:
          if event.key == pygame.K_LEFT:
              playerX_change = -5
          if event.key == pygame.K_RIGHT:
              playerX_change = 5
          if event.key == pygame.K_SPACE and bullet_state == "ready":
              bulletX = playerX
              fire_bullet(bulletX, bulletY)
      if event.type == pygame.KEYUP and event.key in [pygame.K_LEFT, pygame.K_RIGHT]:
          playerX_change = 0

    # Player Movement
    playerX += playerX_change
    playerX = max(0, min(playerX, SCREEN_WIDTH - 64))  # 64 is the size of the player

    # Enemy Movement
    for i in range(num_of_enemies):
        if enemyY[i] > 340:  # Game Over Condition
            for j in range(num_of_enemies):
                enemyY[j] = 2000
            game_over_text()
            break

        enemyX[i] += enemyX_change[i]
        if enemyX[i] <= 0 or enemyX[i] >= SCREEN_WIDTH - 64:
            enemyX_change[i] *= -1
            enemyY[i] += enemyY_change[i]

        # Collision Check
        if isCollision(enemyX[i], enemyY[i], bulletX, bulletY):
            bulletY = PLAYER_START_Y
            bullet_state = "ready"
            score_value += 1
            enemyX[i] = random.randint(0, SCREEN_WIDTH - 64)
            enemyY[i] = random.randint(ENEMY_START_Y_MIN, ENEMY_START_Y_MAX)

        enemy(enemyX[i], enemyY[i], i)

    # Bullet Movement
    if bulletY <= 0:
        bulletY = PLAYER_START_Y
        bullet_state = "ready"
    elif bullet_state == "fire":
        fire_bullet(bulletX, bulletY)
        bulletY -= bulletY_change

    player(playerX, playerY)
    show_score(textX, textY)
    pygame.display.update()
 

Activity screenshots


Outcome

Learning Outcomes of Space Invader Project - Part 2:

• Learned to implement advanced game mechanics such as collision detection and sprite movement.
• Gained skills in handling player and enemy interactions in the game.
• Understood how to create a continuous game loop for a smooth gameplay experience.
• Applied these concepts to further develop the Space Invader game, enhancing interactivity and functionality.

Agenda
 

Complete Code: Download Now

 

Click the Copy button to copy the activity code.
 

 

Thumb Rule: Go through the teacher agenda away before starting the class 

Always appreciate and motivate the students during the session.
Keep the sessions jolly; try to add as many possible ways to keep the student engaged other than the activity.
Never make the session a mugging parrot, explain the concepts included, and guide the student to do the activity.
For different activities, follow these instructions to complete them-
Finally, end the class with the sample questions -
Did you enjoy today’s lesson?
Are you excited about the next lesson?
Do you have any doubts?
Do you want to say something about the lesson?
Before starting the class, please make sure you go through the pointers given below and  follow all of these in the entire session to have a wonderful student experience

1] Greetings

2] Discuss previous class ACP

3] Teach class material+ do class activity

4] Eye exercise

5] Summarize the goal of the class

6] Today's ACP

7] What student will be learning in the next class

 

Assignment: Level Up this game
Write a Python program to add background image and sound to the previously created game.

Open project link
→
Goal

This activity aims to help you understand the concepts of Pygame and implement them.

Getting started
 

In the “Main.py” file, you write the code for your project. Make sure you upload the file you have to work on (link mentioned above).

Procedure:

Go to repl.it
Click on +New Repl


From the drop-down menu, select Python, and then give a name to your repl(always try to provide the name related to the project only).
                    



 

By default, the main.py file will be created. However, you can start writing your code here.
Instructions
 

Take your parent’s help wherever necessary.
Make a sequence of steps to achieve the goal. 
Feel free to use different code blocks to make the project more interesting.
Note down the doubts that you might have faced during the project.
Finally, close any pop-up/dialogue window that appears during the project.
How to submit?
 

After completing, click on the Share button in the top right corner.


A window will pop up. Click on the copy button.


 

Paste this link on the student dashboard—Press Ctrl+V for Windows and Command + V in Mac.


How was this lesson?
Your feedback helps us make lessons better.

Share your feedback
