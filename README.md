import pygame
import sys

# Initialize Pygame
pygame.init()

# Set up the game window
screen_width = 800
screen_height = 600
screen = pygame.display.set_mode((screen_width, screen_height))
pygame.display.set_caption("Simple Mario Game")

# Load Mario image
mario_image = pygame.image.load("mario.png")
mario_rect = mario_image.get_rect()
mario_rect.topleft = (100, 100)

# Set up clock for controlling the frame rate
clock = pygame.time.Clock()

# Set up variables for movement
move_left = False
move_right = False
gravity = 1
vertical_speed = 0
jump = False

# Main game loop
while True:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            pygame.quit()
            sys.exit()
        elif event.type == pygame.KEYDOWN:
            if event.key == pygame.K_LEFT:
                move_left = True
            elif event.key == pygame.K_RIGHT:
                move_right = True
            elif event.key == pygame.K_SPACE and not jump:
                jump = True
                vertical_speed = -15  # Jumping velocity

        elif event.type == pygame.KEYUP:
            if event.key == pygame.K_LEFT:
                move_left = False
            elif event.key == pygame.K_RIGHT:
                move_right = False

    # Update Mario's position
    if move_left:
        mario_rect.x -= 5
    if move_right:
        mario_rect.x += 5

    # Apply gravity and update vertical position
    vertical_speed += gravity
    mario_rect.y += vertical_speed

    # Check for collisions with the ground (bottom of the window)
    if mario_rect.bottom > screen_height:
        mario_rect.bottom = screen_height
        vertical_speed = 0
        jump = False

    # Draw the background
    screen.fill((135, 206, 250))  # Sky blue color

    # Draw Mario
    screen.blit(mario_image, mario_rect.topleft)

    # Update the display
    pygame.display.flip()

    # Cap the frame rate
    clock.tick(30)
