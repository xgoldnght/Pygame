# Pygame
# Что такое Pygame
Pygame (рус. Пайгейм) — набор модулей (библиотек) языка программирования Python, предназначенный для написания компьютерных игр и мультимедиа-приложений. Pygame базируется на мультимедийной библиотеке SDL.

Изначально Pygame был написан Питом Шиннерсом (Pete Shinners). Начиная примерно с 2004/2005 года поддерживается и развивается сообществом свободного программного обеспечения.

Одна из библиотек предоставляющих доступ к API SDL (существуют и другие). В то же время дает возможность написания более высокоуровневого кода.

Pygame-приложения могут работать под Android на телефонах и планшетах с использованием подмножества Pygame для Android (pgs4a). На этой платформе поддерживаются звук, вибрация, клавиатура, акселерометр.

# Мини-Игра Arkanoid Где главной задачой является уничтожение всех монстров шариком.
```python
import pygame
import time
pygame.init()
back = (200, 255, 255) #цвет фона (background)
mw = pygame.display.set_mode((500, 500)) #окно программы (main window)
mw.fill(back)
clock = pygame.time.Clock()
# переменные - координаты платф.
platform_x=200
platform_y=300
# флаги движения платформы
move_right = False
move_left = False
# переменные отвечающие за направление мяча
dx = 5
dy = 5
# флаг окончанния игры
game_over=False
#класс из пред. проекта
class Area():
    def __init__(self, x=0, y=0, width=10, height=10, color=None):
        self.rect = pygame.Rect(x, y, width, height) #прямоугольник
        self.fill_color = back
    def color(self, new_color):
        self.fill_color = new_color
    def fill(self):
        pygame.draw.rect(mw, self.fill_color, self.rect)
    def collidepoint(self, x, y):
        return self.rect.collidepoint(x, y)
    def colliderect(self, rect):
        return self.rect.colliderect(rect)
class Label(Area):
    def set_text(self, text, fsize=12, text_color=(0, 0, 0)):
        self.image = pygame.font.SysFont('verdana', fsize).render(text, True, text_color)
    def draw(self, shift_x=0, shift_y=0):
        self.fill()
        mw.blit(self.image, (self.rect.x + shift_x, self.rect.y + shift_y))
class Picture(Area):
    def __init__(self, filename, x=0, y=0, width=10, height=10):
        Area.__init__(self, x=x, y=y, width=width, height=height, color=None)
        self.image = pygame.image.load(filename)
    def draw(self):
        mw.blit(self.image, (self.rect.x, self.rect.y))
#создание мяча и платф
ball = Picture('ball.png', 160, 200, 50, 50)
platform = Picture('platform.png', platform_x, platform_y, 100, 30)
# создание вражин
start_x = 5  # координаты создания первого монстра
start_y = 5
count = 9  # количество монстров в верхнем ряду
monsters = []  # список с монстрами
for j in range(3):  # цикл по столцам
    y = start_y + (55 * j)  # координаты монстра в каждом след. столбце будет смещена на 55 пикселей по y
    x = start_x + (27.5 * j)  # и 27.5 по x
    for i in range(count):  # цикл по рядам(строкам) создает в строке количество монстров. равное count
        d = Picture('enemy.png', x, y, 50, 50)  # создаем монстра
        monsters.append(d)  # добавляем в список
        x = x + 55  # увеличение координату следующего монстра
    count = count - 1  # для следующего ряда уменьшаем кол-во монстров
while not game_over:
        ball.fill()
        platform.fill()
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                game_over=True
            if event.type == pygame.KEYDOWN:
                if event.key == pygame.K_RIGHT: #если нажата клавиша
                    move_right = True #поднимаем флаг
                if event.key == pygame.K_LEFT: #если нажата клавиша
                    move_left = True #поднимаем флаг
            elif event.type == pygame.KEYUP:
                if event.key == pygame.K_RIGHT: #если нажата клавиша
                    move_right = False #поднимаем флаг
                if event.key == pygame.K_LEFT: #если нажата клавиша
                    move_left = False #поднимаем флаг
        if move_right: #флаг движения вправо
            platform.rect.x += 50
        if move_left: #флаг движения влево
            platform.rect.x -= 50
        # придаем постоянное ускорение мячу по х и у
        ball.rect.x += dx
        ball.rect.y += dy
        # если мяч достигает границ экрана, меняем направление его движения
        if ball.rect.y < 0:
            dy *= -1
        if ball.rect.x > 450 or ball.rect.x < 0:
            dx *= -1
        #если мяч коснулся платформы, меняем направление движения
        if ball.rect.colliderect(platform.rect):
            dy *= -1
        if ball.rect.y > 350:
            time_text = Label(150,150,50,50,back)
            time_text.set_text('YOU LOSE',60, (255,0,0))
            time_text.draw(10, 10)
            game_over = True
        if len(monsters) == 0:
            time_text = Label(150,150,50,50,back)
            time_text.set_text('YOU WIN',60, (0,200,0))
            time_text.draw(10, 10)
            game_over = True
        #отрисовываем всех монстров из списка
        for m in monsters:
            m.draw()
            if m.rect.colliderect(ball.rect):
                monsters.remove(m)
                m.fill()
                dy *= -1
        platform.draw()
        ball.draw()
        pygame.display.update()
        clock.tick(40)
```
### Результат
![Меню]()
![Меню]()
![Меню]()
