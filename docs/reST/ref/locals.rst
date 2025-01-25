.. include:: common.txt

:mod:`pygame.locals`
====================

.. module:: pygame.locals
   :synopsis: pygame constants

| :sl:`pygame constants`

This module contains various constants used by pygame. Its contents are
automatically placed in the pygame module namespace. However, an application
can use ``pygame.locals`` to include only the pygame constants with a ``from
pygame.locals import *``.

Detailed descriptions of the various constants can be found throughout the
pygame documentation. Here are the locations of some of them.

   - The :mod:`pygame.display` module contains flags like ``FULLSCREEN`` used
     by :func:`pygame.display.set_mode`.
   - The :mod:`pygame.event` module contains the various event types.
   - The :mod:`pygame.key` module lists the keyboard constants and modifiers
     (``K_``\* and ``MOD_``\*) relating to the ``key`` and ``mod`` attributes of
     the ``KEYDOWN`` and ``KEYUP`` events.
   - The :mod:`pygame.time` module defines ``TIMER_RESOLUTION``.

.. ## pygame.locals #pygame.init()

import pygame
from pygame.locals import *
from OpenGL.GL import *
from OpenGL.GLUT import *
from OpenGL.GLU import *
import math

# Initialize Pygame
pygame.init()

# Set up display
display = (800, 600)
pygame.display.set_mode(display, DOUBLEBUF | OPENGL)
gluPerspective(45, (display[0] / display[1]), 0.1, 50.0)
glTranslatef(0.0, 0.0, -5)

# Cube vertices and edges
vertices = [
    (1, -1, -1),
    (1, 1, -1),
    (-1, 1, -1),
    (-1, -1, -1),
    (1, -1, 1),
    (1, 1, 1),
    (-1, 1, 1),
    (-1, -1, 1)
]

edges = [
    (0, 1),
    (1, 2),
    (2, 3),
    (3, 0),
    (4, 5),
    (5, 6),
    (6, 7),
    (7, 4),
    (0, 4),
    (1, 5),
    (2, 6),
    (3, 7)
]

# Function to draw the cube
def draw_cube():
    glBegin(GL_LINES)
    for edge in edges:
        for vertex in edge:
            glVertex3fv(vertices[vertex])
    glEnd()

# Function to move the cube
def move_cube(keys):
    if keys[K_LEFT]:
        glTranslatef(0.1, 0, 0)
    if keys[K_RIGHT]:
        glTranslatef(-0.1, 0, 0)
    if keys[K_UP]:
        glTranslatef(0, 0, 0.1)
    if keys[K_DOWN]:
        glTranslatef(0, 0, -0.1)

# Game loop
def game_loop():
    running = True
    while running:
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                running = False

        # Clear screen and reset matrix
        glClear(GL_COLOR_BUFFER_BIT | GL_DEPTH_BUFFER_BIT)
        glPushMatrix()

        # Rotate the cube (for demo purposes)
        glRotatef(1, 3, 1, 1)

        # Get keyboard input to move the cube
        keys = pygame.key.get_pressed()
        move_cube(keys)

        # Draw the cube
        draw_cube()

        # Pop the matrix to return to the original position
        glPopMatrix()

        # Swap buffers to update the screen
        pygame.display.flip()
        pygame.time.wait(10)

    pygame.quit()

# Run the game loop
game_loop()


