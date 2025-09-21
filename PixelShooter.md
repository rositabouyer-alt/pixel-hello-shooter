import tkinter as tk
import random

WIDTH = 500
HEIGHT = 400
PLAYER_SIZE = 30
ENEMY_SIZE = 30
BULLET_SIZE = 5
BULLET_SPEED = 10

root = tk.Tk()
root.title("Mini Shooter Game")
canvas = tk.Canvas(root, width=WIDTH, height=HEIGHT, bg="black")
canvas.pack()

player = canvas.create_rectangle(WIDTH//2-PLAYER_SIZE//2, HEIGHT-PLAYER_SIZE-10, WIDTH//2+PLAYER_SIZE//2, HEIGHT-10, fill="blue")

enemy = canvas.create_rectangle(random.randint(0, WIDTH-ENEMY_SIZE), 50, random.randint(0, WIDTH-ENEMY_SIZE)+ENEMY_SIZE, 50+ENEMY_SIZE, fill="red")

bullets = []

def move_player(event):
    if event.keysym == "Left":
        canvas.move(player, -20, 0)
    elif event.keysym == "Right":
        canvas.move(player, 20, 0)

root.bind("<Left>", move_player)
root.bind("<Right>", move_player)

def shoot(event):
    x1, y1, x2, y2 = canvas.coords(player)
    bullet = canvas.create_rectangle((x1+x2)/2-BULLET_SIZE, y1-BULLET_SIZE*2, (x1+x2)/2+BULLET_SIZE, y1, fill="yellow")
    bullets.append(bullet)

canvas.bind("<space>", shoot)
canvas.focus_set()

def game_loop():
    global enemy
    for bullet in bullets[:]:
        canvas.move(bullet, 0, -BULLET_SPEED)
        bx1, by1, bx2, by2 = canvas.coords(bullet)
        ex1, ey1, ex2, ey2 = canvas.coords(enemy)
        if bx2 > ex1 and bx1 < ex2 and by2 > ey1 and by1 < ey2:
            canvas.delete(bullet)
            bullets.remove(bullet)
            canvas.delete(enemy)
            new_x = random.randint(0, WIDTH-ENEMY_SIZE)
            enemy = canvas.create_rectangle(new_x, 50, new_x+ENEMY_SIZE, 50+ENEMY_SIZE, fill="red")
        elif by2 < 0:
            canvas.delete(bullet)
            bullets.remove(bullet)
    root.after(50, game_loop)

game_loop()
root.mainloop()
