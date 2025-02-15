import pygame
import random

# 初始化Pygame
pygame.init()

# 颜色常量
BLACK = (0, 0, 0)
WHITE = (255, 255, 255)
RED = (255, 0, 0)
GREEN = (0, 255, 0)

# 窗口设置
WIDTH = 600
HEIGHT = 400
BLOCK_SIZE = 20

# 创建游戏窗口
screen = pygame.display.set_mode((WIDTH, HEIGHT))
pygame.display.set_caption("贪吃蛇")

# 游戏时钟
clock = pygame.time.Clock()

# 蛇的初始设置
snake = [
    [WIDTH//2, HEIGHT//2],
    [WIDTH//2 - BLOCK_SIZE, HEIGHT//2],
    [WIDTH//2 - 2*BLOCK_SIZE, HEIGHT//2]
]
direction = "RIGHT"
new_direction = direction

# 食物生成函数
def generate_food():
    while True:
        x = random.randint(0, (WIDTH-BLOCK_SIZE)//BLOCK_SIZE) * BLOCK_SIZE
        y = random.randint(0, (HEIGHT-BLOCK_SIZE)//BLOCK_SIZE) * BLOCK_SIZE
        food = [x, y]
        if food not in snake:
            return food

food = generate_food()
score = 0

# 游戏主循环
running = True
game_over = False

while running:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False
        elif event.type == pygame.KEYDOWN:
            if event.key == pygame.K_UP and direction != "DOWN":
                new_direction = "UP"
            elif event.key == pygame.K_DOWN and direction != "UP":
                new_direction = "DOWN"
            elif event.key == pygame.K_LEFT and direction != "RIGHT":
                new_direction = "LEFT"
            elif event.key == pygame.K_RIGHT and direction != "LEFT":
                new_direction = "RIGHT"

    # 更新方向
    direction = new_direction

    # 计算新头部位置
    head = snake[0].copy()
    if direction == "UP":
        head[1] -= BLOCK_SIZE
    elif direction == "DOWN":
        head[1] += BLOCK_SIZE
    elif direction == "LEFT":
        head[0] -= BLOCK_SIZE
    elif direction == "RIGHT":
        head[0] += BLOCK_SIZE

    # 碰撞检测
    if head[0] < 0 or head[0] >= WIDTH or head[1] < 0 or head[1] >= HEIGHT:
        game_over = True
    if head in snake:
        game_over = True

    if game_over:
        running = False
        continue

    # 插入新头部
    snake.insert(0, head)

    # 吃食物检测
    if head == food:
        score += 1
        food = generate_food()
    else:
        snake.pop()

    # 绘制界面
    screen.fill(BLACK)
    for segment in snake:
        pygame.draw.rect(screen, GREEN, (segment[0], segment[1], BLOCK_SIZE, BLOCK_SIZE))
    pygame.draw.rect(screen, RED, (food[0], food[1], BLOCK_SIZE, BLOCK_SIZE))

    pygame.display.flip()
    clock.tick(10)  # 控制游戏速度

pygame.quit()
print(f"游戏结束！最终得分：{score}")# -
简单的小游戏
