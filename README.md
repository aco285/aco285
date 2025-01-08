import pygame
import sys

# Inisialisasi Pygame
pygame.init()

# Ukuran layar
WIDTH, HEIGHT = 800, 600
screen = pygame.display.set_mode((WIDTH, HEIGHT))
pygame.display.set_caption("Might Action X")

# Warna
WHITE = (255, 255, 255)
BLACK = (0, 0, 0)
RED = (255, 0, 0)
BLUE = (0, 0, 255)

# Kecepatan
PLAYER_SPEED = 5
BULLET_SPEED = 10
ENEMY_SPEED = 2

# Pemain
player = pygame.Rect(400, 500, 50, 50)
player_color = BLUE

# Peluru
bullets = []

# Musuh
enemies = [pygame.Rect(100, 100, 50, 50), pygame.Rect(300, 100, 50, 50)]

# Waktu
clock = pygame.time.Clock()

# Fungsi menggambar def draw():
    screen.fill(WHITE)
    pygame.draw.rect(screen, player_color, player)
    for bullet in bullets:
        pygame.draw.rect(screen, RED, bullet)
    for enemy in enemies:
        pygame.draw.rect(screen, BLACK, enemy)
    pygame.display.flip()

# Game loop
while True:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            pygame.quit()
            sys.exit()

    # Kontrol pemain
    keys = pygame.key.get_pressed()
    if keys[pygame.K_LEFT] and player.left > 0:
        player.move_ip(-PLAYER_SPEED, 0)
    if keys[pygame.K_RIGHT] and player.right < WIDTH:
        player.move_ip(PLAYER_SPEED, 0)
    if keys[pygame.K_SPACE]:
        # Tambahkan peluru
        bullets.append(pygame.Rect(player.centerx - 5, player.top, 10, 20))

    # Pergerakan peluru
    for bullet in bullets[:]:
        bullet.move_ip(0, -BULLET_SPEED)
        if bullet.bottom < 0:
            bullets.remove(bullet)

    # Pergerakan musuh
    for enemy in enemies:
        enemy.move_ip(0, ENEMY_SPEED)
        if enemy.top > HEIGHT:
            enemy.top = -50

    # Deteksi tabrakan
    for bullet in bullets[:]:
        for enemy in enemies[:]:
            if bullet.colliderect(enemy):
                bullets.remove(bullet)
                enemies.remove(enemy)
                break

    # Gambar ulang layar
    draw()

    # Batasi FPS
    clock.tick(60)
