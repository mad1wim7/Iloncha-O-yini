# Iloncha-O'yini
import pygame
import random

# Pygame-ni boshlash
pygame.init()

# O'yin oynasi o'lchamlari
width, height = 640, 480
win = pygame.display.set_mode((width, height))
pygame.display.set_caption("Snake Game")

# Ranglar
white = (255, 255, 255)
yellow = (255, 255, 102)
black = (0, 0, 0)
red = (213, 50, 80)
green = (0, 255, 0)
blue = (50, 153, 213)

# Ilon parametrlari
snake_block = 10
snake_speed = 15

# O'yin uchun so'zlar
font_style = pygame.font.SysFont("bahnschrift", 25)
score_font = pygame.font.SysFont("comicsansms", 35)

# Natijani ko'rsatish
def Your_score(score):
    value = score_font.render("Sizning ballaringiz: " + str(score), True, yellow)
    win.blit(value, [0, 0])

# Ilonni chizish
def our_snake(snake_block, snake_list):
    for x in snake_list:
        pygame.draw.rect(win, green, [x[0], x[1], snake_block, snake_block])

# O'yin tugadi
def message(msg, color):
    mesg = font_style.render(msg, True, color)
    win.blit(mesg, [width / 6, height / 3])

# O'yin asosiy funksiya
def gameLoop():
    game_over = False
    game_close = False

    # Ilon boshlang'ich holati
    x1 = width / 2
    y1 = height / 2

    x1_change = 0
    y1_change = 0

    snake_List = []
    Length_of_snake = 1

    # Ovqat joylashuvi
    foodx = round(random.randrange(0, width - snake_block) / 10.0) * 10.0
    foody = round(random.randrange(0, height - snake_block) / 10.0) * 10.0

    while not game_over:

        while game_close:
            win.fill(blue)
            message("O'yin tugadi! Q - chiqish, C - yana o'ynash", red)
            Your_score(Length_of_snake - 1)
            pygame.display.update()

            for event in pygame.event.get():
                if event.type == pygame.KEYDOWN:
                    if event.key == pygame.K_q:
                        game_over = True
                        game_close = False
                    if event.key == pygame.K_c:
                        gameLoop()

        # O'yin boshqaruvi
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                game_over = True
            if event.type == pygame.KEYDOWN:
                if event.key == pygame.K_LEFT:
                    x1_change = -snake_block
                    y1_change = 0
                elif event.key == pygame.K_RIGHT:
                    x1_change = snake_block
                    y1_change = 0
                elif event.key == pygame.K_UP:
                    y1_change = -snake_block
                    x1_change = 0
                elif event.key == pygame.K_DOWN:
                    y1_change = snake_block
                    x1_change = 0

        # Ilon chegaradigan joyni tekshirish
        if x1 >= width or x1 < 0 or y1 >= height or y1 < 0:
            game_close = True
        x1 += x1_change
        y1 += y1_change
        win.fill(blue)

        # Ovqatni chizish
        pygame.draw.rect(win, yellow, [foodx, foody, snake_block, snake_block])

        # Ilon tanasini yangilash
        snake_Head = []
        snake_Head.append(x1)
        snake_Head.append(y1)
        snake_List.append(snake_Head)
        if len(snake_List) > Length_of_snake:
            del snake_List[0]

        for x in snake_List[:-1]:
            if x == snake_Head:
                game_close = True

        our_snake(snake_block, snake_List)
        Your_score(Length_of_snake - 1)

        pygame.display.update()

        # Ovqatni yeganini tekshirish
        if x1 == foodx and y1 == foody:
            foodx = round(random.randrange(0, width - snake_block) / 10.0) * 10.0
            foody = round(random.randrange(0, height - snake_block) / 10.0) * 10.0
            Length_of_snake += 1

        pygame.time.Clock().tick(snake_speed)

    pygame.quit()
    quit()

gameLoop()



