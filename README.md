# stoneman
# Python based 2-D game
# I have made this game based on a pygame tutorial and I want to enhance the game features.Looking forward for any inputs1





import pygame
import time
import random

pygame.init()

display_width = 800
display_height = 600
black = (0,0,0)
white = (255,255,255)
bright_red=(255,0,0)
bright_green=(0,255,0)
bright_blue =(0,0,255)
red=(200,0,0)
green=(0,200,0)
blue=(0,0,200)
gray=(127,127,127)
teal= (0,170,153)


car_speed = 0
s_width=72
s_height=84

gameDisplay = pygame.display.set_mode((display_width,display_height))
pygame.display.set_caption('Stone Man !!')
clock = pygame.time.Clock()

cavemanImg = pygame.image.load('C:\\Users\\vivek\\Desktop\\stoneman game\\cavman3.png')
stoneImg = pygame.image.load('C:\\Users\\vivek\\Desktop\\stoneman game\\stone.000.png')
background = pygame.image.load('C:\\Users\\vivek\\Desktop\\stoneman game\\gameimage.png')
#gameIcon = pygame.image.load('C:\\Users\\vivek\\Desktop\\stoneman game\\pygameicon.png')

#pygame.display.set_icon(gameIcon)

global Pause 
Pause = False
hit_sound = pygame.mixer.Sound("C:\\Users\\vivek\\Desktop\\stoneman game\\stonehit.wav")


def car(x,y):
    gameDisplay.blit(cavemanImg, (x,y))


def stone(thingx,thingy):
    gameDisplay.blit(stoneImg, (thingx,thingy))
    
    
def text_objects(text, font):
    textSurface = font.render(text, True, teal)
    
    return textSurface, textSurface.get_rect()


def text_button(text, font):
    textSurface = font.render(text, True, black)
    return textSurface, textSurface.get_rect()

"""def text_PLAY(text, font):
    textSurface = font.render(text, True, black)
    return textSurface, textSurface.get_rect()"""

def message(text):
    largeText = pygame.font.Font('freesansbold.ttf',115)
    TextSurf, TextRect = text_objects(text, largeText)
    TextRect.center = ((display_width/2),(display_height/2))
    gameDisplay.blit(TextSurf, TextRect)
    pygame.display.update()
    time.sleep(2)
    game_loop()

    
    
def hit():
    pygame.mixer.music.stop()
    pygame.mixer.Sound.play(hit_sound)
    message('GAME OVER')
    
   

def things(thingx,thingy,thing_w,thing_h):
    stone(thingx,thingy)
    

def score(count):
    font = pygame.font.SysFont(None, 25)
    text = font.render("Dodged: "+str(count), True, black)
    gameDisplay.blit(text,(0,0))
    
    
def button(label,x,y,w,h,ic,ac,action=None):
     mouse=pygame.mouse.get_pos()
     click =pygame.mouse.get_pressed()
     if x+w > mouse[0] > x and y+w > mouse[1] > y:
            pygame.draw.rect(gameDisplay,ac,(x,y,w,h))
            if click[0]==1 and action != None:
                action()
     else:    
            pygame.draw.rect(gameDisplay,ic,(x,y,w,h))
            
     smallText = pygame.font.Font("freesansbold.ttf",15)
     textSurf, textRect = text_button(label, smallText)
     textRect.center = ( (x+(w/2)), (y+(h/2)) )
     gameDisplay.blit(textSurf, textRect)  
     
def quitgame():
    pygame.quit()
    quit()     

def game_intro():

    intro = True

    while intro:
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                pygame.quit()
                quit()
                
        #gameDisplay.fill(white)
        gameDisplay.blit(background,[0,0])
        largeText = pygame.font.Font('freesansbold.ttf',115)
        TextSurf, TextRect = text_objects("STONE-MAN", largeText)
        TextRect.center = ((display_width/2),(display_height/2))
        gameDisplay.blit(TextSurf, TextRect)
        
        button("PLAY",150,450,100,50,gray,teal,game_loop)
        button("QUIT",550,450,100,50,gray,teal,game_loop)
        
        #button("PLAY",150,450,100,50,green,bright_green,game_loop)
        #button("QUIT",550,450,100,50,blue,bright_blue,quitgame)
         
        pygame.display.update()
        clock.tick(15)
        
        if event.type == pygame.KEYDOWN:
            game_loop()
        
        
def unpause():
    global pause
    pause = False  
    pygame.mixer.music.unpause()      
        
def paused():

    global pause
    pause = True
    pygame.mixer.music.pause()
    
    while pause:
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                pygame.quit()
                quit()
                
        #gameDisplay.fill(white)
        #gameDisplay.blit(background,[0,0])
        largeText = pygame.font.Font('freesansbold.ttf',115)
        TextSurf, TextRect = text_objects("PAUSED", largeText)
        TextRect.center = ((display_width/2),(display_height/2))
        gameDisplay.blit(TextSurf, TextRect)
        
        button("CONTINUE",150,450,100,50,gray,teal,unpause)
        button("QUIT",550,450,100,50,gray,teal,quitgame)
         
        pygame.display.update()
        clock.tick(15)        
   
    
def game_loop():
    x =  (display_width * 0.45)
    y = (display_height * 0.8)
    x_change = 0
    y_change = 0
    
    thingx = random.randrange(0,display_width)
    thingy = -500
    thing_speed = 7
    thing_w = 39
    thing_h = 33
    thingCount = 1
    dodged = 0
    pygame.mixer.music.load('C:\\Users\\vivek\\Desktop\\stoneman game\\music.wav')
    pygame.mixer.music.play(-1)
    global Pause
    
    game_exit = False
    while not game_exit:
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                pygame.quit()
                quit()
                

        
            if event.type == pygame.KEYDOWN:
                if event.key == pygame.K_LEFT:
                    x_change = -5
                if event.key == pygame.K_RIGHT:
                    x_change = 5
                if event.key == pygame.K_UP:
                    y_change = -5
                if event.key == pygame.K_DOWN:
                    y_change = 5
                if event.key == pygame.K_p:
                    
                    paused()
            if event.type == pygame.KEYUP:
                if event.key == pygame.K_LEFT or event.key == pygame.K_RIGHT:
                    x_change = 0
                if event.key == pygame.K_UP or event.key == pygame.K_DOWN:
                    y_change = 0
                    
        
  
        x += x_change
        y += y_change
         
        gameDisplay.fill(white)
        #gameDisplay.blit(background,[0,0])
        
        things(thingx, thingy, thing_w, thing_h)
        thingy += thing_speed
        car(x,y)
        
        score(dodged)
    
        if x > display_width-s_width or x < 0:
            hit()  
            
        if y > display_height-s_height or y < 0:
            hit()
            
        if thingy > display_height:
            thingy = 0 - thing_h
            thingx = random.randrange(0,display_width-thing_w) 
            dodged += 1
            thing_speed += 1
            
            
        if y < thingy + thing_h:
            if x > thingx and x < thingx + thing_w or  x + s_width > thingx and x + s_width < thingx + thing_w:
                hit()
                game_intro()
            
        pygame.display.update()
        clock.tick(60)

game_intro()
game_loop()
pygame.quit()
quit()
