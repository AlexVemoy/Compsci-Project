# Compsci-Project
from random import randint
import pygame, sys
import os
import time
import json
import math

WIDTH, HEIGHT = 800, 500
screen=pygame.display.set_mode((WIDTH,HEIGHT))
pygame.display.set_caption("Moving Character")
Box_left, Box_right=WIDTH/4 - 150 , WIDTH*0.75 -150
Box_top, Box_bottom=HEIGHT*0.25 , HEIGHT*0.75 -30
clock=pygame.time.Clock()

current_time=0
in_rect_time=0
countdown = 5
last_count=pygame.time.get_ticks()

#define fonts
font30=pygame.font.SysFont('Calibri', 30)
font40=pygame.font.SysFont('Calibri', 40)

TopLeftBox = False
TopRightBox = False
BottomLeftBox = False
BottomRightBox = False



WalkRight = []
WalkRight.append(pygame.image.load(os.path.join('Game','R1.png')))
WalkRight.append(pygame.image.load(os.path.join('Game','R2.png')))
WalkRight.append(pygame.image.load(os.path.join('Game','R3.png')))
WalkRight.append(pygame.image.load(os.path.join('Game','R4.png')))
WalkRight.append(pygame.image.load(os.path.join('Game','R5.png')))
WalkRight.append(pygame.image.load(os.path.join('Game','R6.png')))
WalkRight.append(pygame.image.load(os.path.join('Game','R7.png')))
WalkRight.append(pygame.image.load(os.path.join('Game','R8.png')))
WalkRight.append(pygame.image.load(os.path.join('Game','R9.png')))

WalkLeft=[]
WalkLeft.append(pygame.image.load(os.path.join('Game','L1.png')))
WalkLeft.append(pygame.image.load(os.path.join('Game','L2.png')))
WalkLeft.append(pygame.image.load(os.path.join('Game','L3.png')))
WalkLeft.append(pygame.image.load(os.path.join('Game','L4.png')))
WalkLeft.append(pygame.image.load(os.path.join('Game','L5.png')))
WalkLeft.append(pygame.image.load(os.path.join('Game','L6.png')))
WalkLeft.append(pygame.image.load(os.path.join('Game','L7.png')))
WalkLeft.append(pygame.image.load(os.path.join('Game','L8.png')))
WalkLeft.append(pygame.image.load(os.path.join('Game','L9.png')))

#define function for creating text
def draw_text(text, font, text_col, x, y):
    img = font.render(text, True, text_col)
    screen.blit(img, (x, y))

bg = pygame.image.load(os.path.join('Game','bg.jpg'))
bg_Transformed=pygame.transform.scale(bg,(WIDTH, HEIGHT)) # changes the image size of background to fit the width of the screen.
char = pygame.image.load(os.path.join('Game','standing.png'))

#Character to move
CharacterW, CharacterH= 64, 64
Character_x=WIDTH/2 - CharacterW/2
Character_y=HEIGHT/2 - CharacterH/2
VEL=5
LEFT=False
RIGHT=False
WalkCount=0

#================================================================================================================================

with open('Questions.txt') as f:
    questions=json.load(f)

with open('Options.txt') as O:
    Options=json.load(O)

with open('Answer.txt') as A:
    AnswerList=A.readlines()

NewAnswerList= str(AnswerList)[2: -2]

NewQuestionsList=[]
newquestion=[]
FileQuestions=[0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10]


for i in range (0, 10):
    num=randint(0, 10)
    newquestion=FileQuestions[num]
    while newquestion in NewQuestionsList:
        num=randint(0, 10)
        newquestion=FileQuestions[num]
    NewQuestionsList.append(newquestion)
    print(NewQuestionsList)


def Question(NewQuestionsList):

    #stats for game
    guesses = ""
    CurrentQ = 1
    guess=""
    Guess=0
    Score = 0
    QuestionNumber=0
    RandomQuestion=NewQuestionsList[QuestionNumber]
    Guessed=False

    #QuestionNumber+=1
    print(RandomQuestion)
    #Questions
    print(questions[RandomQuestion])
    Checked=False
    while Checked==False:
        draw_text(str(questions[RandomQuestion])[2: -2], font40, (0, 0, 0), WIDTH/2 - 50, HEIGHT*0.15)
    #Options
        draw_text(str(Options[RandomQuestion][0]), font30, (0, 0, 0), WIDTH/6 , HEIGHT*0.3)
        draw_text(str(Options[RandomQuestion][1]), font30, (0, 0, 0), WIDTH*0.6 - 10, HEIGHT*0.3)
        draw_text(str(Options[RandomQuestion][2]), font30, (0, 0, 0), WIDTH/6 , HEIGHT*0.75)
        draw_text(str(Options[RandomQuestion][3]), font30, (0, 0, 0), WIDTH*0.6 - 10, HEIGHT*0.75)
        pygame.display.update()

        if TopLeftBox == True:
            guess="A"
            guesses.append(guess)
            Guessed= True

        elif TopRightBox == True:
            guess = "B"
            guesses.append(guess)
            Guessed= True

        elif BottomLeftBox == True:
            guess = "C"
            guesses.append(guess)
            Guessed= True

        elif BottomRightBox == True:
            guess = "D"
            guesses.append(guess)
            Guessed= True

        else:
            Guessed = False

        correct_guess = Check_answer(NewAnswerList[RandomQuestion], Guess, Checked)
        print(correct_guess)

        correct_guess = Guess
        CurrentQ+=1
        if correct_guess == 1:
            Score+= 1
        NewQuestionsList.pop(0)
        return NewQuestionsList
        Checked=True

    if Score == 10:
        display_score(correct_guess, guesses)

def Check_answer(answer, Guess, Checked): #unhashtag the parts in here when fixed
    if answer==Guess:
        #draw_text(str("Correct!"), font40, (0, 0, 0), WIDTH/2 - 10, HEIGHT*0.15)
        return 1
    else:
        #draw_text(str("Wrong!"), font40, (0, 0, 0), WIDTH/2 - 10, HEIGHT*0.15)
        return 0
    Checked=True

def display_score(correct_guess, guesses): #unhashtag the parts in here when fixed

    #draw_text(str("Results"), font40, (0, 0, 0), WIDTH/2 - 10, HEIGHT*0.15)

    #draw_text(("Answers: "), font40, (0, 0, 0), WIDTH/2 - 10, HEIGHT*0.15)
    #for i in questions:
        #draw_text(str(questions.get(i)), font40, (0, 0, 0), WIDTH/2 - 10, HEIGHT*0.15)

    #draw_text(("Guesses: "), font40, (0, 0, 0), WIDTH/2 - 10, HEIGHT*0.15)
    #for i in guesses:
        #draw_text(str(i, end=" "), font40, (0, 0, 0), WIDTH/2 - 10, HEIGHT*0.15)

    score = int((correct_guess/len(questions))*100)
    #draw_text(str("Your score is: "+str(score)+"%"), font40, (0, 0, 0), WIDTH/2 - 10, HEIGHT*0.15)

def Play_again():

    if TopLeftBox == True and LockedIn == True:
        response = "YES"

        if response == "YES":
            return True
        else:
            return False

#=================================================================================================

def redrawGameWindow():
    global WalkCount

    #screen.blit(bg_Transformed, (0, 0))
 # the place surface that it will go on, the RGB, the X and Y positions and the dmensions
    screen.fill((252,148,3))
    pygame.draw.rect(screen, (252,44,3), (Box_left, Box_top, 300, 120))
    pygame.draw.rect(screen, (252,44,3), (Box_right, Box_top, 300, 120))
    pygame.draw.rect(screen, (252,44,3), (Box_left, Box_bottom, 300, 120))
    pygame.draw.rect(screen, (252,44,3), (Box_right, Box_bottom, 300, 120))
    #pygame.draw.rect(screen, (0,0,255), (Character_x, Character_y, CharacterW, CharacterH))# the moving character
    #count down display
    if WalkCount+1>=27:
        WalkCount=0
    if LEFT:
        screen.blit(WalkLeft[WalkCount//3],(Character_x, Character_y))
        WalkCount += 1
    elif RIGHT:
        screen.blit(WalkRight[WalkCount//3], (Character_x, Character_y))
        WalkCount += 1
    else:
        screen.blit(char, (Character_x,Character_y))

    #top left box counter
    if TopLeftBox == True:
        draw_text(str(countdown), font40, (0, 0, 0), WIDTH/2 - 10, HEIGHT*0.15)

    #top right box counter
    elif TopRightBox==True:
        draw_text(str(countdown), font40, (0, 0, 0), WIDTH/2 - 10, HEIGHT*0.15)

    #bottom left box counter
    elif BottomLeftBox==True:
        draw_text(str(countdown), font40, (0, 0, 0), WIDTH/2 - 10, HEIGHT*0.15)

    #bottom right box counter
    elif BottomRightBox==True:
        draw_text(str(countdown), font40, (0, 0, 0), WIDTH/2 - 10, HEIGHT*0.15)

    elif LockedIn == True:
        draw_text(str("locked in!"), font40, (0, 0, 0), WIDTH/2 - 10, HEIGHT*0.15)


    pygame.display.update()

while True:
#-------------------------------------------------------------------------------------------------------------------------------------------------------
#Top-left
    current_time=pygame.time.get_ticks()
    clock.tick(60) #game always runs at 60fps so it doesnt run at different speeds on different computers
    if (Character_x + CharacterW > Box_left and Character_x < Box_left + 300) and (Character_y+CharacterH > Box_top and Character_y < Box_top+150):
        in_rect_time=pygame.time.get_ticks()
        #print(in_rect_time)
        count_timer=pygame.time.get_ticks()
        if count_timer-last_count > 1000:
            countdown-=1
            last_count=count_timer

#-------------------------------------------------------------------------------------------------------------------------------------------------------
#top right
    elif (Character_x + CharacterW > Box_right and Character_x < Box_right + 300) and (Character_y+CharacterH > Box_top and Character_y < Box_top+150):
        in_rectone_time=pygame.time.get_ticks()
        #print(in_rect_time)

        count_timer=pygame.time.get_ticks()
        if count_timer-last_count > 1000:
            countdown-=1
            last_count=count_timer
#-------------------------------------------------------------------------------------------------------------------------------------------------------
#bot left
    elif (Character_x + CharacterW> Box_left and Character_x < Box_left + 300) and (Character_y+CharacterH > Box_bottom and Character_y < Box_bottom+150):
        in_rect_time=pygame.time.get_ticks()
        #print(in_rect_time)

        count_timer=pygame.time.get_ticks()
        if count_timer-last_count > 1000:
            countdown-=1
            last_count=count_timer

#-------------------------------------------------------------------------------------------------------------------------------------------------------
#bot right
    elif (Character_x + CharacterW> Box_right and Character_x < Box_right + 300) and (Character_y+CharacterH > Box_bottom and Character_y < Box_bottom+150):
        in_rectone_time=pygame.time.get_ticks()
        #print(in_rect_time)

        count_timer=pygame.time.get_ticks()
        if count_timer-last_count > 1000:
            countdown-=1
            last_count=count_timer
#-------------------------------------------------------------------------------------------------------------------------------------------------------

    else:
        countdown=5

    for event in pygame.event.get():
        if event.type==pygame.QUIT:
            pygame.quit()#this makes the game close if the x is pressed
            sys.exit()
    if (Character_x + CharacterW > Box_left and Character_x < Box_left + 300) and (Character_y+CharacterH > Box_top and Character_y < Box_top+150) and countdown>0:
        TopLeftBox=True

    #top right box counter
    elif (Character_x + CharacterW> Box_right and Character_x < Box_right + 300) and (Character_y+CharacterH > Box_top and Character_y < Box_top+150) and countdown>0:
        TopRightBox=True
    #bottom left box counter
    elif (Character_x + CharacterW> Box_left and Character_x < Box_left + 300) and (Character_y+CharacterH > Box_bottom and Character_y < Box_bottom+150) and countdown>0:
        BottomLeftBox=True

    #bottom right box counter
    elif (Character_x + CharacterW> Box_right and Character_x < Box_right + 300) and (Character_y+CharacterH > Box_bottom and Character_y < Box_bottom+150) and countdown>0:
        BottomRightBox=True

    elif countdown <= -1:
        LockedIn = True

    else:
        TopLeftBox=False
        TopRightBox=False
        BottomLeftBox=False
        BottomRightBox=False
        LockedIn=False
        Neutral=True


    keys=pygame.key.get_pressed()

    #Movement and Boundaries
    if keys[pygame.K_a] and Character_x > VEL:
        Character_x -= VEL
        LEFT=True
        RIGHT=False
    elif keys[pygame.K_d] and Character_x < WIDTH-CharacterW:
        Character_x += VEL
        LEFT=False
        RIGHT=True
    else:
        LEFT=False
        RIGHT=False
        WalkCount=0
    if keys[pygame.K_w] and Character_y > VEL:
        Character_y -= VEL
        LEFT=False
        RIGHT=False
        WalkCount=0
    if keys[pygame.K_s] and Character_y < HEIGHT-CharacterH:
        Character_y += VEL
        LEFT=False
        RIGHT=False
        WalkCount=0

    NewQuestionsList=Question(NewQuestionsList)
    while Play_again():
        NewQuestionsList=Question(NewQuestionsList)

    redrawGameWindow()


#for the countdown, go to pygame space invaders beginner tutorial in python-part 9

#https://www.youtube.com/watch?v=-ykeT6kk4bk - api tutorial
#Pygame Tutorial #3 - Character Animation and s

#if i decide to make a menu- youtube.com/watch?v=0RryiSjpJn0 or instead "Pygame menu system tutorial Part 1
