import pygame as pg
import time
import os
import random
from modules.sqlite import sqlite as db

pg.init()
pg.mixer.init()

class Background(pg.sprite.Sprite):
    def __init__(self, image_file, location):
        pg.sprite.Sprite.__init__(self)  #call Sprite initializer
        self.image = pg.image.load(image_file)
        self.rect = self.image.get_rect()
        self.rect.left, self.rect.top = location

class game():
    def __init__(self):
        self.__bullet_speed = 20
        self.__display_caption = 'Dead Gear'
        self.__src_path = os.path.dirname(os.path.abspath(__file__)) + '/src/'
        self.__black = (0,0,0)
        self.__white = (255,255,255)
        self.__start_image = pg.image.load(self.__src_path + 'start.png')
        self.__tarfic_red_image = pg.image.load(self.__src_path + 'trafic_red.png')
        self.__tarfic_green_image = pg.image.load(self.__src_path + 'trafic_green.png')
        self.__tarfic_yellow_image = pg.image.load(self.__src_path + 'trafic_yellow.png')
        self.__speedometer_0 = pg.image.load(self.__src_path + 'speedometer_0.png')
        self.__speedometer_1 = pg.image.load(self.__src_path + 'speedometer_1.png')
        self.__speedometer_2 = pg.image.load(self.__src_path + 'speedometer_2.png')
        self.__speedometer_3 = pg.image.load(self.__src_path + 'speedometer_3.png')
        self.__speedometer_4 = pg.image.load(self.__src_path + 'speedometer_4.png')
        self.__speedometer_5 = pg.image.load(self.__src_path + 'speedometer_5.png')
        self.__car_1 = pg.image.load(self.__src_path + 'car_1.png')
        self.__car_2 = pg.image.load(self.__src_path + 'car_2.png')
        self.__car_3 = pg.image.load(self.__src_path + 'car_3.png')
        self.__car_4 = pg.image.load(self.__src_path + 'car_4.png')
        self.__car_5 = pg.image.load(self.__src_path + 'car_5.png')
        self.__racecar = pg.image.load(self.__src_path + 'racecar.png')
        self.__police = pg.image.load(self.__src_path + 'police.png')
        self.__tank = pg.image.load(self.__src_path + 'tank.png')
        self.__rocket = pg.image.load(self.__src_path + 'rocket.png')
        self.__exploit = pg.image.load(self.__src_path + 'exploit.png')
        self.__car_image = self.__car_1
        self.__trafic_color = ['red','yellow','green']
        self.__BackGround = Background(self.__src_path + 'road.png', [0,0])
        self.__MENU = Background(self.__src_path + 'menu.png', [0,0])
        self.__CRASH = Background(self.__src_path + 'crash.png', [0,0])
        self.__GARAGE = Background(self.__src_path + 'garage.png' , [0,0])
        self.__WIN = Background(self.__src_path + 'win.png', [0,0])
        self.__choose_car = 1
        self.__show_Car = self.__car_1
        self.__stop = False
        self.__crash = False
        self.__menu_back = True
        self.__done = False
        self.__start = True
        self.__left = None
        self.__event= None
        self.__choose = False
        self.__screen_height = 600
        self.__screen_width = 800
        self.__car_x = (self.__screen_width * 0.442)
        self.__car_y = (self.__screen_height * 0.75)
        self.__change_x = 0
        self.__speed = 0
        self.__screen = pg.display.set_mode((self.__screen_width,self.__screen_height))
        pg.display.set_caption(self.__display_caption)
        self.__clock = pg.time.Clock()
        self.__cars = []
        self.__car_width = 30
        self.__car_height = 40
        self.__menu_key = pg.font.Font(self.__src_path + 'destroy.ttf',40)
        self.__k_start = self.__menu_key.render('START', True, self.__white)
        self.__k_garage = self.__menu_key.render('GARAGE', True, self.__black)
        self.__k_menu = self.__menu_key.render('MENU', True, self.__white)
        self.__k_exit = self.__menu_key.render('EXIT', True, self.__black)
        self.__score = 0
        self.__score_msg = 'SCORE {}'.format(self.__score)
        self.__score_font = pg.font.Font(self.__src_path + 'destroy.ttf',20)
        self.__score_display = self.__score_font.render(self.__score_msg, True, self.__white)
        self.__sound_crash = self.__src_path + 'sound_crash.mp3'
        self.__sound_police = self.__src_path + 'police.mp3'
        self.__sound_car =  self.__src_path + 'sound_car.mp3'
        self.__sound_start = self.__src_path + 'sound_start.mp3'
        self.__sound_meydoon = self.__src_path + 'meydoon.mp3'
        self.__sound_win = self.__src_path + 'win.mp3'
        self.__sound_tier = self.__src_path + 'wheel.mp3'
        self.__sound_door = self.__src_path + 'door.mp3'
        self.__sound_tank = self.__src_path + 'tank.mp3'
        self.__sound_drift = self.__src_path + 'drift.mp3'
        self.__sound_rocket = self.__src_path + 'rocket.mp3'
        self.__sound_exploit = self.__src_path + 'sound_exploit.mp3'
        self.__menu_option = 3
        self.__msg_x = (self.__screen_width * 0.5)
        self.__msg_y = (self.__screen_height * 0.2)
        self.__msg_size = 115
        self.__msg_font = self.__src_path + 'destroy.ttf'
        self.__msg_color = self.__white
        self.__vehicles_sound = None
        self.__police_mode = False
        self.__is_racecar = False
        self.__is_changed = True
        self.__bullets = []
        self.__bullet_width = 100
        self.__bullet_height = 50
        self.__cars_num = 1
        self.__cars_speed = 20
        self.__cars_max_speed = self.__cars_speed
        self.__db = db('game')
        self.__db_car = ''
        self.__car_2_price = 500
        self.__car_3_price = 1000
        self.__car_4_price = 1500
        self.__car_5_price = 2000
        self.__increase_num = [100,300,500,900,1000]
        self.__special_cars = [300,600,800]
        self.__win_score = 3000
        self.__db.createTable('account',id = 'integer',score = 'integer',car_1 = 'boolean',car_2 = 'boolean',car_3 = 'boolean',car_4 = 'boolean',car_5 = 'boolean')
        if len(self.__db.selectAll('account',fetch='all')) == 0:
            self.__db.execute_manuall("INSERT INTO account('id','score','car_1','car_2','car_3','car_4','car_5') VALUES ('1','0',True,False,False,False,False)")
            self.__MONEY = 0
        else :
            self.count_money()
            print(self.__MONEY)

        self.main()

    def count_money(self):
        self.__MONEY = self.__db.selectValueFrom('account','score',id = 1 ,fetch='one')[0]

    def reset(self):
        self.__db_car = ''
        self.__bullets = []
        self.__is_changed = True
        self.__is_racecar = False
        self.__stop = False
        self.__crash = False
        self.__menu_back = True
        self.__choose = False
        self.__done = False
        self.__start = True
        self.__left = None
        self.__event= None
        self.__screen_height = 600
        self.__screen_width = 800
        self.__car_x = (self.__screen_width * 0.442)
        self.__car_y = (self.__screen_height * 0.75)
        self.__change_x = 0
        self.__speed = 0
        self.__screen = pg.display.set_mode((self.__screen_width,self.__screen_height))
        self.__cars = []
        self.__k_start = self.__menu_key.render('START', True, self.__white)
        self.__k_exit = self.__menu_key.render('EXIT', True, self.__black)
        self.__k_menu = self.__menu_key.render('MENU', True, self.__white)
        self.__k_garage = self.__menu_key.render('GARAGE', True, self.__black)
        self.__k_exit = self.__menu_key.render('EXIT', True, self.__black)
        self.__score = 0
        self.__choose_car = 1
        self.__score_msg = 'SCORE {}'.format(self.__score)
        self.__score_font = pg.font.Font(self.__src_path + 'destroy.ttf',20)
        self.__score_display = self.__score_font.render(self.__score_msg, True, self.__white)
        self.__menu_option = 3
        self.__cars_speed = 8
        self.__cars_max_speed = self.__cars_speed
        self.__police_mode = False
        if self.__car_image == self.__tank or self.__car_image == self.__police or self.__car_image == self.__racecar:
            self.__car_image = self.__show_Car

    def sound_effect(self,sound_path,num = 1):
        pg.mixer.init()
        pg.mixer.music.load(sound_path)
        pg.mixer.music.play(num)

    def display_flag(self):
        self.__screen.blit(self.__start_image,((self.__screen_width * 0.235),(self.__screen_height * 0.3)))
    
    def display_car(self,x,y):
        self.__screen.blit(self.__car_image,(x,y))

    def display_flash(self,color):
        if color == 'red':
            return self.__tarfic_red_image
        elif color == 'yellow':
            return self.__tarfic_yellow_image
        elif color == 'green':
            return self.__tarfic_green_image
    
    def display_speedometer(self,gear):
        if gear == 0:
            self.__screen.blit(self.__speedometer_0,((self.__screen_width * 0.02),(self.__screen_height * 0.7)))
        elif gear == 1:
            self.__screen.blit(self.__speedometer_1,((self.__screen_width * 0.02),(self.__screen_height * 0.7)))
        elif gear == 2:
            self.__screen.blit(self.__speedometer_2,((self.__screen_width * 0.02),(self.__screen_height * 0.7)))
        elif gear == 3:
            self.__screen.blit(self.__speedometer_3,((self.__screen_width * 0.02),(self.__screen_height * 0.7)))
        elif gear == 4:
            self.__screen.blit(self.__speedometer_4,((self.__screen_width * 0.02),(self.__screen_height * 0.7)))
        elif gear == 5:
            self.__screen.blit(self.__speedometer_5,((self.__screen_width * 0.02),(self.__screen_height * 0.7)))

    def Speedometer(self):
        max_speed = 8
        if self.__show_Car == self.__car_1:
            max_speed *= 1
        if self.__show_Car == self.__car_2:
            max_speed *= 2 
        if self.__show_Car == self.__car_3:
            max_speed *= 3 
        if self.__show_Car == self.__car_4:
            max_speed *= 4
        if self.__show_Car == self.__car_5:
            max_speed *= 5
        if self.__car_image == self.__police:
            max_speed *= 3
        if self.__car_image == self.__racecar:
            max_speed *= 5
        if self.__car_image == self.__tank:
            max_speed *= 1

        if self.__speed < max_speed and self.__event.key == pg.K_UP:
            self.__speed += 8
        elif self.__speed > 0 and self.__event.key == pg.K_DOWN:
            self.__speed += -8

    def steeringـwheel(self):
        if self.__event.key == pg.K_LEFT:
            self.__left = True
        elif self.__event.key == pg.K_RIGHT:
            self.__left = False
    def engine(self):
        global left,speed,change_x
        if self.__left:
            self.__change_x = -self.__speed
        elif not self.__left:
            self.__change_x = self.__speed
        
    def text_objects(self,text, font , color):
        textSurface = font.render(text, True, color)
        return textSurface, textSurface.get_rect()

    def message_display(self,text):
        largeText = pg.font.Font(self.__msg_font,self.__msg_size)
        TextSurf, TextRect = self.text_objects(text, largeText , self.__msg_color)
        TextRect.center = (self.__msg_x,self.__msg_y)
        self.__screen.blit(TextSurf, TextRect)
        pg.display.update()
        time.sleep(1)
        
    def cars_display(self,num,x,y):
        if num == 0:
            self.__screen.blit(self.__car_1,(x,y))
        elif num == 1:
            self.__screen.blit(self.__car_2,(x,y))
        elif num == 2:
            self.__screen.blit(self.__car_3,(x,y))
        elif num == 3:
            self.__screen.blit(self.__car_4,(x,y))
        elif num == 4:
            self.__screen.blit(self.__car_5,(x,y))
        elif num == 5:
            self.__screen.blit(self.__car_6,(x,y))
        elif num == 6:
            self.__screen.blit(self.__car_7,(x,y))
        elif num == 7:
            self.__screen.blit(self.__exploit,(x,y))

    def game_over(self):
        self.__stop = True
        self.sound_effect(self.__sound_crash)
        while not self.__crash:
            for event in pg.event.get():
                if event.type == pg.QUIT:
                    pg.quit()
                    quit()
                elif event.type == pg.KEYDOWN:
                    if event.key == pg.K_RETURN:
                        if self.__menu_back:
                            self.reset()
                            self.main()

                        else:
                            self.__done = True
                            pg.quit()
                            quit()
                    if event.key == pg.K_UP:
                        self.__menu_back = True
                        self.__k_menu = self.__menu_key.render('MENU', True, self.__white)
                        self.__k_exit = self.__menu_key.render('EXIT', True, self.__black)
                    elif event.key == pg.K_DOWN:
                        self.__menu_back = False
                        self.__k_menu = self.__menu_key.render('MENU', True, self.__black)
                        self.__k_exit = self.__menu_key.render('EXIT', True, self.__white)

            self.__screen.fill(self.__white)
            self.__screen.blit(self.__CRASH.image, self.__CRASH.rect)
            self.__screen.blit(self.__k_menu,(600,50))
            self.__screen.blit(self.__k_exit,(50,500))
            self.__score_point = self.__menu_key.render('SCORE {}'.format(self.__score),True,self.__white)
            self.__screen.blit(self.__score_point,(50,50))
            pg.display.update()
            self.__clock.tick(60)




    def win(self):
        self.__stop = True
        self.sound_effect(self.__sound_win)
        while not self.__crash:
            for event in pg.event.get():
                if event.type == pg.QUIT:
                    pg.quit()
                    quit()
                elif event.type == pg.KEYDOWN:
                    if event.key == pg.K_RETURN:
                        if self.__menu_back:
                            self.reset()
                            self.main()

                        else:
                            self.__done = True
                            pg.quit()
                            quit()
                    if event.key == pg.K_UP:
                        self.__menu_back = True
                        self.__k_menu = self.__menu_key.render('MENU', True, self.__white)
                        self.__k_exit = self.__menu_key.render('EXIT', True, self.__black)
                    elif event.key == pg.K_DOWN:
                        self.__menu_back = False
                        self.__k_menu = self.__menu_key.render('MENU', True, self.__black)
                        self.__k_exit = self.__menu_key.render('EXIT', True, self.__white)

            self.__screen.fill(self.__white)
            self.__screen.blit(self.__WIN.image, self.__WIN.rect)
            self.__screen.blit(self.__k_menu,(600,50))
            self.__screen.blit(self.__k_exit,(50,500))
            self.__score_point = self.__menu_key.render('SCORE {}'.format(self.__score),True,self.__black)
            self.__screen.blit(self.__score_point,(50,50))
            pg.display.update()
            self.__clock.tick(60)


    




    def garage(self):
        self.sound_effect(self.__sound_door)
        available = 0
        msg_trade = ''
        while not self.__choose:
            for event in pg.event.get():    
                if event.type == pg.QUIT:
                        self.__done = True
                        pg.quit()
                        quit()
                elif event.type == pg.KEYDOWN:
                    if event.key == pg.K_LEFT:
                        self.sound_effect(self.__sound_tier)
                        if self.__choose_car < 5 :
                            self.__choose_car += 1
                    elif event.key == pg.K_RIGHT:
                        self.sound_effect(self.__sound_tier)
                        if self.__choose_car > 1 :
                            self.__choose_car -= 1
                    if event.key == pg.K_RIGHT or event.key == pg.K_LEFT:
                        if self.__choose_car == 1:
                            self.__show_Car = self.__car_1
                            self.__db_car = 'car_1'
                        elif self.__choose_car == 2:
                            self.__show_Car = self.__car_2
                            self.__db_car = 'car_2'
                            msg_trade = self.__car_2_price
                        elif self.__choose_car == 3:
                            self.__show_Car = self.__car_3
                            self.__db_car = 'car_3'
                            msg_trade = self.__car_3_price
                        elif self.__choose_car == 4:
                            self.__show_Car = self.__car_4
                            self.__db_car = 'car_4'
                            msg_trade = self.__car_4_price
                        elif self.__choose_car == 5:
                            self.__show_Car = self.__car_5
                            self.__db_car = 'car_5'
                            msg_trade = self.__car_5_price
                        available = self.__db.selectValueFrom('account',"{}".format(self.__db_car),id = 1,fetch='one')[0]
                        if available == 1:
                            msg_trade = ''
                        else:
                            if self.__db_car == 'car_2':
                                msg_trade = self.__car_2_price
                            elif self.__db_car == 'car_3':
                                msg_trade = self.__car_3_price
                            elif self.__db_car == 'car_4':
                                msg_trade = self.__car_4_price
                            elif self.__db_car == 'car_5':
                                msg_trade = self.__car_5_price

                            
                    if event.key == pg.K_RETURN:                   
                        if available == 1 or msg_trade == '':
                            self.__car_image = self.__show_Car
                            self.__choose = True
                            self.reset()
                            self.main()
                        else:
                            print(msg_trade)
                            print(self.__MONEY)
                            if int(self.__MONEY) > msg_trade:
                                self.__MONEY -= msg_trade
                                self.__db.execute_manuall('UPDATE account SET score = score - {} WHERE id = 1'.format(msg_trade))
                                self.__db.execute_manuall('UPDATE account SET {} = True WHERE id = 1'.format(self.__db_car))
                                msg_trade = ''
                            

                                

                                     
            self.__screen.fill(self.__white)
            self.__screen.blit(self.__GARAGE.image, self.__GARAGE.rect)
            self.__screen.blit(self.__show_Car , (self.__screen_width * 0.43 , self.__screen_height * 0.37))
            MONEY = self.__menu_key.render('MONEY ${}'.format(self.__MONEY),True,self.__white)
            if msg_trade != '':
                msg_display = self.__menu_key.render('COST ${}'.format(msg_trade),True,self.__white)
                self.__screen.blit(msg_display,(225,350))
            self.__screen.blit(MONEY,(25,25))
            pg.display.update()
            self.__clock.tick(60)

    
    def ready(self):
        self.__screen.fill(self.__white)
        self.__screen.blit(self.__BackGround.image,self.__BackGround.rect)
        self.sound_effect(self.__sound_start)
        for i in range(1,4):
            self.__screen.blit(self.display_flash(self.__trafic_color[i-1]),((self.__screen_width * 0.04),(self.__screen_height * 0.04)))
            self.display_speedometer(self.__speed / 8)
            self.display_car(self.__car_x,self.__car_y)
            self.message_display('{}'.format(i))
            self.__screen.fill(self.__white)
            self.__screen.blit(self.__BackGround.image, self.__BackGround.rect)
        self.display_speedometer(self.__speed / 8)
        self.display_flag()
        self.display_car(self.__car_x,self.__car_y)
        self.message_display('START')
        time.sleep(1)
        self.sound_effect(self.__sound_car,-1)


    
    def menu(self):
        self.sound_effect(self.__sound_meydoon,-1)
        while not self.__done:
            for event in pg.event.get():
                if event.type == pg.QUIT:
                    self.__done = True
                    pg.quit()
                    quit()
                elif event.type == pg.KEYDOWN:
                    if event.key == pg.K_RETURN:
                        if self.__menu_option == 3:
                            self.__done = True
                            self.ready()
                            self.spwn_cars()
                        if self.__menu_option == 2:
                            self.__done = True
                            self.garage()
                        if self.__menu_option == 1:
                            self.__done = True
                            pg.quit()
                            quit()
                    if event.key == pg.K_UP:
                        if self.__menu_option < 3 :
                            self.__menu_option += 1
                    elif event.key == pg.K_DOWN:
                        if self.__menu_option > 1 :
                            self.__menu_option -= 1
                    if self.__menu_option == 3:
                        self.__k_start = self.__menu_key.render('START', True, self.__white)
                        self.__k_garage = self.__menu_key.render('GARAGE', True, self.__black)
                        self.__k_exit = self.__menu_key.render('EXIT', True, self.__black)
                    if self.__menu_option == 2:
                        self.__k_start = self.__menu_key.render('START', True, self.__black)
                        self.__k_garage = self.__menu_key.render('GARAGE', True, self.__white)
                        self.__k_exit = self.__menu_key.render('EXIT', True, self.__black)
                    if self.__menu_option == 1:
                        self.__k_start = self.__menu_key.render('START', True, self.__black)
                        self.__k_garage = self.__menu_key.render('GARAGE', True, self.__black)
                        self.__k_exit = self.__menu_key.render('EXIT', True, self.__white)
                    
        
            self.__screen.fill(self.__white)
            self.__screen.blit(self.__MENU.image, self.__MENU.rect)
            self.__screen.blit(self.__k_start,(535,330))
            self.__screen.blit(self.__k_garage,(515,410))
            self.__screen.blit(self.__k_exit,(560,490))
            pg.display.update()
            self.__clock.tick(60)

    def bullet_display(self,x,y):
        self.__screen.blit(self.__rocket,(x,y))

    def shoot(self):
            spwn_x = self.__car_x
            spwn_y = self.__car_y
            bullet_speed = random.randrange(10,self.__bullet_speed)
            self.__bullets.append([spwn_x,spwn_y,bullet_speed])


    def spwn_cars(self):
        for i in range(self.__cars_num):
            car_theme = random.randrange(1,5)
            spwn_x = random.randrange(20,self.__screen_width - 20)
            spwn_y = 0 
            cars_speed = random.randrange(1,self.__cars_max_speed)
            self.__cars.append([car_theme,spwn_x,spwn_y,cars_speed])
        
    def main(self):
        self.menu()
        while not self.__stop:
            for self.__event in pg.event.get():
                if self.__event.type == pg.QUIT:
                    self.__stop = True
                elif self.__event.type == pg.KEYDOWN:
                    if self.__car_image == self.__tank:
                        self.sound_effect(self.__sound_tank,-1)

                    if self.__event.key == pg.K_SPACE and self.__car_image == self.__racecar:
                        self.sound_effect(self.__sound_drift)
                        time.sleep(1)
                        self.sound_effect(self.__sound_car,-1)



                    if self.__event.key == pg.K_SPACE and self.__car_image == self.__tank:
                        self.sound_effect(self.__sound_rocket)
                        self.shoot()
                        time.sleep(1)
                        
                        
                        
                    if self.__event.key == pg.K_SPACE and self.__car_image == self.__police:
                        self.__police_mode = not self.__police_mode
                        if self.__police_mode:
                            self.__cars_max_speed = self.__cars_speed / 2 
                            self.sound_effect(self.__sound_police,-1)
                        else:
                            self.__cars_max_speed = self.__cars_speed
                            self.sound_effect(self.__sound_car,-1)

                        
                    self.Speedometer()
                    self.steeringـwheel()
                    self.engine()

            if self.__car_x >= self.__screen_width:
                self.__car_x = 0
            elif self.__car_x <= 0 :
                self.__car_x = self.__screen_width

            if self.__score > self.__increase_num[0]:
                self.__cars_num = 2
            if self.__score > self.__increase_num[1]:
                self.__cars_num = 3
            if self.__score > self.__increase_num[2]:
                self.__cars_num = 4
            if self.__score > self.__increase_num[3]:
                self.__cars_num = 5
            if self.__score > self.__increase_num[4]:
                self.__cars_num = 6    

            if self.__score > self.__special_cars[0]:
                self.__car_image = self.__police
            if self.__score > self.__special_cars[1]:
                self.__car_image = self.__racecar  
                self.__cars_max_speed = self.__cars_speed    
            if self.__is_changed and self.__score > self.__special_cars[1]:      
                self.__is_racecar = True
                self.__is_changed = False
            if self.__score > self.__special_cars[2]:
                self.__car_image = self.__tank
                self.__speed = 8

            self.__screen.fill(self.__white)
            self.__screen.blit(self.__BackGround.image, self.__BackGround.rect)
            self.display_speedometer(self.__speed / 8)
            self.__car_x += self.__change_x

            
            for bullet in self.__bullets:
                bullet[1] -= bullet[2]
                self.bullet_display(bullet[0],bullet[1])
            

            if len(self.__cars) == 0:
                self.__cars = []
                self.spwn_cars()    
            
            for car in self.__cars:
                for bullet in self.__bullets:
                    if (bullet[0]  - self.__bullet_width) <= car[1] <= (bullet[0] + self.__bullet_width) and (bullet[1] - self.__bullet_height) <= car[2] and car[2] <= (bullet[1] + self.__bullet_height):
                        if car[0] != 7:
                            car[0] = 7
                            self.__score += 300
                            self.sound_effect(self.__sound_exploit)

                if car[0] != 7 and (self.__car_x - self.__car_width) <= car[1] and car[1] <= (self.__car_x + self.__car_width) and (self.__car_y - self.__car_height) <= car[2] and  car[2] <= (self.__car_y + self.__car_height):
                    self.__db.execute_manuall("UPDATE account SET score = score + {} WHERE id = 1".format(self.__score))
                    self.__MONEY += self.__score 
                    self.game_over()

                if self.__score > self.__win_score:
                    self.__score = 3000
                    self.__db.execute_manuall("UPDATE account SET score = score + {} WHERE id = 1".format(self.__score))
                    self.__MONEY += self.__score
                    self.win()

                if car[2] >= 450 and car[0] != 7:
                    self.__score += int(self.__speed / 8)
                


                if car[2] >= self.__screen_height:
                    self.__cars.remove(car)
                car[2] += car[3]
                self.cars_display(car[0],car[1],car[2])
            
            if self.__is_racecar :
                self.sound_effect(self.__sound_car,-1)
                self.__is_racecar = False
            
            self.__score_msg = 'SCORE {}'.format(self.__score)
            self.__score_display = self.__score_font.render(self.__score_msg, True, self.__white)
            self.__screen.blit(self.__score_display,(10,10))   
            self.display_car(self.__car_x,self.__car_y)
            pg.display.update()
            self.__clock.tick(60)
    

game()
