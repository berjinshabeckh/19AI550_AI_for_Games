# Ex.No: 5  Implementation of Jumping behavior 
### DATE: 06/09/24
### REGISTER NUMBER : 212222240018
### NAME : Berjin Shabeck

### AIM: 
To write a python program to simulate Jumbing behavior.

### Algorithm:
1. Start the program
2. Import the necessary modules
3. Initiate the pygame engine and window
4. Specify the necessary parameter for player height,depth,gravity,jump power. 
5. Create a game loop to simulate the continuous behavior.
6. If Quit button is pressed then quit the pygame window.
7. Move the player left when left button is pressed
8. Move the player right when right button is pressed
9. If space bar is pressed then enable the jump by increasing y axis value.
10. land the player and display the player at every timestep
11.  Stop the program
 
### Program:
```
import pygame

# Initialize Pygame
pygame.init()

sprite_image_filename = r'c:\ASHWIN\creating the game with ai\frames\mario_frame_01.png.gif'

# Screen dimensions
width, height = 1000, 600
screen = pygame.display.set_mode((width, height))
pygame.display.set_caption("Simple Jumping")
sprite=pygame.image.load(sprite_image_filename)

# Colors
black = (0, 0, 0)
white = (255, 255, 0)

# Player properties
player_width = 40
player_height = 300
player_x = 100
player_y = height - player_height 
player_velocity = 5
jump_power = -15
gravity = 1
is_jumping = False

# Initial vertical speed
vertical_speed = 5

# Game loop
running = True
while running:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False

    keys = pygame.key.get_pressed()

    # Horizontal movement
    if keys[pygame.K_LEFT]:
        player_x -= player_velocity
    if keys[pygame.K_RIGHT]:
        player_x += player_velocity

    # Jumping logic
    if not is_jumping:
        if keys[pygame.K_SPACE]:
            is_jumping = True
            vertical_speed = jump_power

    # Apply gravity
    if is_jumping:
        player_y += vertical_speed
        vertical_speed += gravity

        # Check if player lands on the ground
        if player_y >= height - player_height:
            player_y = height - player_height
            is_jumping = False

    # Clear screen
    screen.fill(black)

    # Draw player
    #pygame.draw.rect(screen, white, (player_x, player_y, player_width, player_height))
    screen.blit(sprite,(player_x,player_y))

    # Update display
    pygame.display.flip()

    # Frame rate
    pygame.time.delay(30)

# Quit Pygame
pygame.quit()
```

### Output:
![1](https://github.com/user-attachments/assets/de9c8e11-f732-4556-ba6b-fa8c63481c34)
![Screenshot 2024-08-24 154051](https://github.com/user-attachments/assets/61556090-4e86-412a-861c-82b3b4f4df51)

### Result:
Thus the simple jumping behavior  was implemented.
