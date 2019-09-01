# Dodge-the-block-game
#It's a simple block dodging game made by using python language.


import pygame
import sys
import random
pygame.init()

WIDTH = 800
HEIGHT = 600

RED = (159,45,11)
BLUE = (229,229,81)#rgb color coding for red is (255,0,0)
BGCOLOR = (57,155,103)
YELLOW = (255,255,0)
player_size = 50
player_pos = [  WIDTH/2,HEIGHT-2*player_size]

enemy_size = 50
enemy_pos = [random.randint(0,WIDTH-enemy_size),0]       #random positions of enemy block from 0 to width
enemy_list = [enemy_pos]
SPEED = 10

screen = pygame.display.set_mode((WIDTH,HEIGHT))

game_over = False

score = 0

clock = pygame.time.Clock()

myFont = pygame.font.SysFont("monospace",35)

def set_level(score,SPEED):
    if score<20:
        SPEED = 5
    elif score<40:
        SPEED = 7
    elif score<80:
        SPEED = 9
    elif score<100:
        SPEED = 10
    elif score<140:
        SPEED = 11
    elif score<180:
        SPEED = 12
    elif score<220:
        SPEED = 14
    elif score<280:
        SPEED = 15
    elif score<320:
        SPEED = 20
    elif score<380:
        SPEED = 23
    else:
       SPEED = 27
  #      SPEED = score/6+1
    return SPEED
def drop_enemies(enemy_list):
    delay = random.random()
    if len(enemy_list) < 10 and delay < 0.1:
        x_pos = random.randint(0,WIDTH-enemy_size)
        y_pos = 0
        enemy_list.append([x_pos,y_pos])

def draw_enemies(enemy_list):
    for enemy_pos in enemy_list:
        pygame.draw.rect(screen, BLUE, (enemy_pos[0], enemy_pos[1], enemy_size, enemy_size))

def update_enemy_positions(enemy_list,score):
    for idx, enemy_pos in enumerate(enemy_list):
# update pos of enemy
     if enemy_pos[1] >= 0 and enemy_pos[1] < HEIGHT:
       enemy_pos[1] += SPEED
#   else:
 #      enemy_pos[0] = random.randint(0,WIDTH-enemy_size)
  #     enemy_pos[1] = 0
# IF THE BLOCK'S OFF THE SCREEN, WE WANNA SET IT TO 0
     else:
       enemy_list.pop(idx)
       score+=1
    return score
def collision_check(enemy_list,player_pos):
    for enemy_pos in enemy_list:
        if detect_collision(enemy_pos, player_pos):
            return True
    return  False


def detect_collision(player_pos, enemy_pos):
    p_x = player_pos[0]
    p_y = player_pos[1]

    e_x = enemy_pos[0]
    e_y = enemy_pos[1]

    if (e_x >= p_x and e_x < (p_x + player_size)) or (p_x >= e_x and p_x < (e_x+enemy_size)):
        if(e_y >= p_y and e_y < (p_y + player_size)) or (p_y >= e_y and p_y < (e_y+enemy_size)):
            return True
    return False



while not game_over:

    for event in pygame.event.get():

        if event.type == pygame.QUIT:
            sys.exit()
        if event.type == pygame.KEYDOWN:

            x = player_pos[0]
            y = player_pos[1]

            if event.key == pygame.K_LEFT:
                x -= player_size   #shifting x cod by 5
            elif event.key == pygame.K_RIGHT:
                x += player_size

            player_pos = [x,y]
    screen.fill(BGCOLOR)



    if detect_collision(player_pos,enemy_pos):
        game_over = True
        break

    drop_enemies(enemy_list)

    score = update_enemy_positions(enemy_list,score)
    SPEED = set_level(score,SPEED)

    text = "Score:" + str(score)
    label = myFont.render(text,1,YELLOW)
    screen.blit(label,(WIDTH-200, HEIGHT-40))

    if collision_check(enemy_list,player_pos):
        game_over = True
    draw_enemies(enemy_list)

    pygame.draw.rect(screen,RED,(player_pos[0],player_pos[1],player_size,player_size))

    clock.tick(30)

    pygame.display.update()
