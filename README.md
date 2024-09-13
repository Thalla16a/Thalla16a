pip install pygame
import pygame
import random

# Initialize pygame
pygame.init()

# Screen dimensions
SCREEN_WIDTH = 800
SCREEN_HEIGHT = 600

# Colors
WHITE = (255, 255, 255)
BLACK = (0, 0, 0)
RED = (255, 0, 0)

# Screen setup
screen = pygame.display.set_mode((SCREEN_WIDTH, SCREEN_HEIGHT))
pygame.display.set_caption('Simple Running Game')

# Player class
class Player(pygame.sprite.Sprite):
    def __init__(self):
        super().__init__()
        self.image = pygame.Surface((50, 50))
        self.image.fill(BLACK)
        self.rect = self.image.get_rect()
        self.rect.center = (100, SCREEN_HEIGHT - 75)
        self.speed_y = 0
        self.jump = False

    def update(self):
        keys = pygame.key.get_pressed()
        if keys[pygame.K_SPACE] and not self.jump:
            self.speed_y = -15
            self.jump = True
        
        self.speed_y += 1  # gravity
        self.rect.y += self.speed_y
        
        if self.rect.bottom >= SCREEN_HEIGHT - 25:
            self.rect.bottom = SCREEN_HEIGHT - 25
            self.speed_y = 0
            self.jump = False

# Obstacle class
class Obstacle(pygame.sprite.Sprite):
    def __init__(self):
        super().__init__()
        self.image = pygame.Surface((50, 50))
        self.image.fill(RED)
        self.rect = self.image.get_rect()
        self.rect.x = SCREEN_WIDTH
        self.rect.y = SCREEN_HEIGHT - 75

    def update(self):
        self.rect.x -= 10
        if self.rect.right < 0:
            self.kill()

# Game loop
def game():
    clock = pygame.time.Clock()
    
