# Snake
import pygame
import random

pygame.init()

screen = pygame.display.set_mode((800, 600))

class Snake:

    def __init__(self):
        self.size = 1
        self.elements = [[100, 100]]
        self.radius = 5
        self.dx = 5  # right
        self.dy = 0
        self.is_add = False

    def draw(self):
        for element in self.elements:
            pygame.draw.circle(screen, (255, 0, 0), element, self.radius)

    def move(self):
        if self.is_add:
            self.size += 1
            self.elements.append([0, 0])
            self.is_add = False

        for i in range(len(self.elements) - 1, 0, -1):
            self.elements[i][0] = self.elements[i - 1][0]
            self.elements[i][1] = self.elements[i - 1][1]

        self.elements[0][0] += self.dx
        self.elements[0][1] += self.dy
class Food:
    def __init__(self):
        self.x = random.randrange(0,800)
        self.y = random.randrange(0,600)
        self.position = [[self.x,self.y]]
        self.radius = 10
    def draw(self):
        for element in self.position:
            pygame.draw.circle(screen, (0, 255, 0), element, 10)
    def new(self):
        self.position[0][0] = random.randint(0,800)
        self.position[0][1] = random.randint(0,600)

snake = Snake()
food = Food()
running = True

d = 5

FPS = 30

clock = pygame.time.Clock()

def distans(x1,y1,x2,y2):
    return ((x2 - x1)**2 + (y2 - y1)**2)**(1/2)
    
while running: 
    mill = clock.tick(FPS) 
    for event in pygame.event.get(): 
        if event.type == pygame.QUIT: 
            running = False 
        if event.type == pygame.KEYDOWN: 
            if event.key == pygame.K_ESCAPE: 
                running = False 
            if event.key == pygame.K_RIGHT: 
                snake.dx = d 
                snake.dy = 0 
            if event.key == pygame.K_LEFT: 
                snake.dx = -d 
                snake.dy = 0 
            if event.key == pygame.K_UP: 
                snake.dx = 0 
                snake.dy = -d 
            if event.key == pygame.K_DOWN: 
                snake.dx = 0 
                snake.dy = d 
            if event.key == pygame.K_1: 
                snake.is_add = True
    eat = distans(snake.elements[0][0], snake.elements[0][1], food.position[0][0], food.position[0][1])
    if eat < 15:
        snake.is_add = True
        food.new()
        FPS = FPS + 3
    if (snake.elements[0][0] + snake.dx) >= 800 or (snake.elements[0][1] + snake.dy) >= 600:
        snake.elements[0][0] = 0
    if (snake.elements[0][0] + snake.dx) <= 0:
        snake.elements[0][0] = 800
    if (snake.elements[0][1] + snake.dy) <= 0:
        snake.elements[0][1] = 600
    snake.move()
    screen.fill((0, 0, 0))
    food.draw()
    snake.draw()
    pygame.display.flip()
