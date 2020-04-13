# Snake
import pygame
import random

pygame.init()
pygame.font.init()
font = pygame.font.SysFont('Times new roman', 32)
screen = pygame.display.set_mode((800, 600))

class Snake:

    def __init__(self):
        self.size = 1
        self.elements = [[100, 100]]
        self.radius = 7
        self.dx = 5  
        self.dy = 0
        self.is_add = False
        self.Game_Over = True

    def draw(self):
        for element in self.elements:
            pygame.draw.circle(screen, (255, 0, 0), element, self.radius)

    def move(self):
        if self.Game_Over:
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
            pygame.draw.circle(screen, (255, 255, 255), element, 10)
    def new(self):
        self.position[0][0] = random.randint(0,800)
        self.position[0][1] = random.randint(0,600)

snake = Snake()
food = Food()
running = True

d = 5

FPS = 30

clock = pygame.time.Clock()
score = 0

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
            if event.key == pygame.K_RIGHT and snake.dx!=-d: 
                snake.dx = d 
                snake.dy = 0 
            if event.key == pygame.K_LEFT and snake.dx!=d: 
                snake.dx = -d 
                snake.dy = 0 
            if event.key == pygame.K_UP and snake.dy!=d: 
                snake.dx = 0 
                snake.dy = -d 
            if event.key == pygame.K_DOWN and snake.dy!=-d: 
                snake.dx = 0 
                snake.dy = d 
            if event.key == pygame.K_1: 
                snake.is_add = True
    eat = distans(snake.elements[0][0], snake.elements[0][1], food.position[0][0], food.position[0][1])
    if eat < 17:
        snake.is_add = True
        food.new()
        score = score + 1
        FPS = FPS + 3
            
    if (snake.elements[0][0] + snake.dx) >= 800:
        snake.elements[0][0] = 0
    if (snake.elements[0][1] + snake.dy) >= 600:
        snake.elements[0][1] = 0
    if (snake.elements[0][0] + snake.dx) <= 0:
        snake.elements[0][0] = 800
    if (snake.elements[0][1] + snake.dy) <= 0:
        snake.elements[0][1] = 600
    snake.move()
    screen.fill((0, 0, 0))
    food.draw()
    snake.draw()
    for i in range(len(snake.elements) - 1, 0, -1):
        if snake.elements[i][0] == snake.elements[0][0] + snake.dx and snake.elements[i][1] == snake.elements[0][1] + snake.dy:
            snake.Game_Over = False
            text1 = font.render('G A M E _ O V E R', True, (255, 255, 0))
            screen.blit(text1, (250, 300))
    text2 = font.render('S C O R E : ' + str(score), True, (255, 255, 0))
    screen.blit(text2, (575 ,20))
    pygame.display.flip()
