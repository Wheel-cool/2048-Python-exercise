import numpy as np
import random
import pygame
import sys

#初始化游戏，建立两个矩阵，record1记录上一步，record2实时变化。
#random随机刷新新的格子
def New_Game():
    global record1, record2
    Gameover = False
    record1 = np.array(
        [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0]).reshape(4, 4)
    record2 = np.array(
        [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0]).reshape(4, 4)

    if random.randint(0, 10) in range(9):
        rand = 1
    else:
        rand = 2
    First_cell = 2 ** rand
    First_cell_location_raw, First_cell_location_clo = random.randint(0, 3), random.randint(0, 3)
    record2[First_cell_location_clo, First_cell_location_raw] = First_cell

#每次刷新一个格子
def Flash():
    if not np.array_equal(record2, record1):
        try:
            if random.randint(0, 10) in range(9):
                rand = 1
            else:
                rand = 2
            Next_cell1 = 2 ** rand
            N = random.choice(np.where(record2.reshape(1, 16) == 0)[1])
            Next_cell1_location_raw = N // 4
            Next_cell1_location_clo = N
            while Next_cell1_location_clo > 3:
                Next_cell1_location_clo = Next_cell1_location_clo - 4
            record2[Next_cell1_location_raw][Next_cell1_location_clo] = Next_cell1
        except:
            pass

#如果16个格子被占满，此时如果一个格子向四周拓展均没有相同的则判断此时游戏结束
def Gameover():
    global gameover
    gameover = 0
    _ = 0
    if len(record2[record2 == 0]) == 0:
        for raw in range(4):
            for clo in range(4):
                if raw == 0:
                    if record2[raw][clo] == record2[raw + 1][clo]:
                        _ = 1
                        break
                elif raw == 3:
                    if record2[raw][clo] == record2[raw - 1][clo]:
                        _ = 1
                        break
                else:
                    if record2[raw][clo] == record2[raw + 1][clo] or record2[raw][clo] == record2[raw - 1][clo]:
                        _ = 1
                        break
                if clo == 0:
                    if record2[raw][clo] == record2[raw][clo + 1]:
                        _ = 1
                        break
                elif clo == 3:
                    if record2[raw][clo] == record2[raw][clo - 1]:
                        _ = 1
                        break
                else:
                    if record2[raw][clo] == record2[raw][clo + 1] or record2[raw][clo] == record2[raw][clo - 1]:
                        _ = 1
                        break
            if _ == 1:
                break
        else:
            gameover = 1

#以下四个函数为向指定方向移动的变化，包括相同格子的合并，不同格子的移动
def point_up():
    for clo in range(4):
        count = 0
        for raw in range(4):
            if record2[raw][clo] != 0:
                record2[count][clo] = record2[raw][clo]
                if count != raw:
                    record2[raw][clo] = 0
                count += 1
        for raw in range(3):
            if record2[raw][clo] == record2[raw + 1][clo] and record2[raw][clo] != 0:
                record2[raw][clo] = record2[raw][clo] * 2
                if raw == 2:
                    lenth = 0
                elif raw == 1:
                    lenth = 1
                else:
                    lenth = 2
                for i in range(lenth):
                    record2[raw + i + 1][clo] = record2[raw + i + 2][clo]
                record2[3][clo] = 0
                break

def point_left():
    for raw in range(4):
        count = 0
        for clo in range(4):
            if record2[raw][clo] != 0:
                record2[raw][count] = record2[raw][clo]
                if count != clo:
                    record2[raw][clo] = 0
                count += 1
        for clo in range(3):
            if record2[raw][clo] == record2[raw][clo + 1] and record2[raw][clo] != 0:
                record2[raw][clo] = record2[raw][clo] * 2
                if clo == 2:
                    lenth = 0
                elif clo == 1:
                    lenth = 1
                else:
                    lenth = 2
                for i in range(lenth):
                    record2[raw][clo + i + 1] = record2[raw][clo + i + 2]
                record2[raw][3] = 0
                break

def point_down():
    for clo in range(4):
        count = 0
        for raw in list(range(4))[::-1]:
            if record2[raw][clo] != 0:
                record2[3 - count][clo] = record2[raw][clo]
                if 3 - count != raw:
                    record2[raw][clo] = 0
                count += 1
        for raw in list(range(1, 4))[::-1]:
            if record2[raw][clo] == record2[raw - 1][clo] and record2[raw][clo] != 0:
                record2[raw][clo] = record2[raw][clo] * 2
                if raw == 1:
                    lenth = 0
                elif raw == 2:
                    lenth = 1
                else:
                    lenth = 2
                for i in range(lenth):
                    record2[raw - i - 1][clo] = record2[raw - i - 2][clo]
                record2[0][clo] = 0
                break

def point_right():
    for raw in range(4):
        count = 0
        for clo in list(range(4))[::-1]:
            if record2[raw][clo] != 0:
                record2[raw][3 - count] = record2[raw][clo]
                if 3 - count != clo:
                    record2[raw][clo] = 0
                count += 1
        for clo in list(range(1, 4))[::-1]:
            if record2[raw][clo] == record2[raw][clo - 1] and record2[raw][clo] != 0:
                record2[raw][clo] = record2[raw][clo] * 2
                if clo == 1:
                    lenth = 0
                elif clo == 2:
                    lenth = 1
                else:
                    lenth = 2
                for i in range(lenth):
                    record2[raw][clo - i - 1] = record2[raw][clo - i - 2]
                record2[raw][0] = 0
                break

#利用pygame设计游戏画面
def gameframe():
    global map_font ,screen
    pygame.init()
    icon1 = pygame.image.load('2048.bmp')
    size = width, height = 500, 600
    screen = pygame.display.set_mode(size)
    pygame.display.set_caption('2048小游戏')
    screen.fill((238 ,220 ,130))
    map_font = pygame.font.Font('msyh.ttc', 40)

    map_font_start = pygame.font.Font('msyh.ttc', 20)

    font_surf1 = map_font_start.render('2048 小游戏', True, (105 ,112 ,225))
    font_rect1 = font_surf1.get_rect()
    font_rect1.center = (100,20)

    font_surf2 = map_font_start.render('***祝您游戏愉快***', True, (150, 0, 221))
    font_rect2 = font_surf2.get_rect()
    font_rect2.center = (100,60)

    font_surf3 = map_font_start.render('白云苍狗 制作', True, (152, 52, 12))
    font_rect3 = font_surf3.get_rect()
    font_rect3.center = (100,100)

    font_surf4 = map_font_start.render('历史最高:{}'.format(eval(maxscord)), True, (0, 0, 0))
    font_rect4 = font_surf3.get_rect()
    font_rect4.center = (395,50)

    screen.blit(font_surf1, font_rect1)
    screen.blit(font_surf2, font_rect2)
    screen.blit(font_surf3, font_rect3)
    screen.blit(font_surf4, font_rect4)

    # fps = 30
    # flock = pygame.time.Clock()

#动态刷新16个格子的值
def show_config():
    color = np.array(['(255 ,246 ,143)','(255 ,226 ,133)','(255 ,216 ,163)','(255 ,216 ,143)','(255 ,236 ,123)','(255 ,246 ,123)','(255 ,255 ,123)','(255 ,255 ,143)','(245 ,246 ,133)','(255 ,216 ,173)','(255 ,226 ,173)','(255 ,226 ,143)','(255 ,246 ,173)','(255 ,246 ,103)',"(255 ,206 ,143)",'(245 ,226 ,163)']).reshape(4,4)

    for i in range(4):
        for j in range(4):
            if record2[i][j] == 0:
                font_surf = map_font.render('', True, (0, 0, 0))
            else:
                font_surf = map_font.render(str(record2[i][j]), True, (0, 0, 0))
            font_rect = font_surf.get_rect()
            font_rect.center = (80 + 110 * j, 190 + 110 * i)
            block = pygame.Surface((100, 100))
            block.fill(eval(color[i][j]))
            screen.blit(block, (30 + 110 * j, 140 + 110 * i))
            screen.blit(font_surf, font_rect)

    map_font_scord = pygame.font.Font('msyh.ttc', 20)
    font_surf4 = map_font_scord.render('总分:{:<}分'.format(np.sum(record2)), True, (0, 0, 0))
    font_rect4 = font_surf4.get_rect()
    font_rect4.center = (400,95)
    block_scord = pygame.Surface((150, 35))
    block_scord.fill((238 ,220 ,130))
    screen.blit(block_scord, (350, 80))
    screen.blit(font_surf4, font_rect4)

#pygame 主循环，键盘事件的监测
def main():
    global record1 ,record2,maxscord
    with open('最好成绩.txt', 'r') as f:
        maxscord = f.read()
    gameframe()
    New_Game()
    path1 = '古筝.wav'
    path2 = 'River Fflows In You.wav'
    flag = path2
    sound = 0.5
    while True:
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                sys.exit()
            elif pygame.mixer.music.get_busy()==False:
                if flag == path1:
                    flag = path2
                else:
                    flag = path1
                pygame.mixer.music.load(flag)
                pygame.mixer.music.play()
            elif event.type == pygame.KEYUP:
                error = 0
                if event.key == pygame.K_LEFT:
                    record1 = record2 * 1
                    point_left()
                elif event.key == pygame.K_RIGHT:
                    record1 = record2 * 1
                    point_right()
                elif event.key == pygame.K_UP:
                    record1 = record2 * 1
                    point_up()
                elif event.key == pygame.K_DOWN:
                    record1 = record2 * 1
                    point_down()
                elif event.key == pygame.K_BACKSPACE:
                    record2 = record1
                elif event.key == pygame.K_u:
                    sound = sound + 0.05
                    pygame.mixer.music.set_volume(sound)
                    error = 1
                elif event.key == pygame.K_d:
                    sound = sound - 0.05
                    pygame.mixer.music.set_volume(sound)
                    error = 1
                else:
                    error = 1
                if not np.array_equal(record2,record1) and error == 0:
                    Flash()

        show_config()
        #flock.tick(fps)
        Gameover()
        if gameover == 1:
            block_over = pygame.Surface((400,150))
            block_over.fill((238 ,220 ,130))
            map_font_over = pygame.font.Font('msyh.ttc', 60)
            font_surf = map_font_over.render('GAME OVER', True, (0, 0, 0))
            font_rect = font_surf.get_rect()
            font_rect.center = (250, 350)
            screen.blit(block_over, (50,275))
            screen.blit(font_surf, font_rect)

            with open('最好成绩.txt', 'w') as f:
                f.write(str(np.sum(record2)))
            if np.sum(record2) > eval(maxscord):
                with open('最好成绩.txt', 'w') as f:
                        f.write(str(np.sum(record2)))
            pygame.display.update()
        pygame.display.update()


if __name__ == "__main__":
    main()
