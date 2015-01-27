---
title: Universe
layout: page
description: A WebGL 2D universe simulation with general-purpose GPU computation.
links:
  - title: Website
    url: https://universe.ns.mg
  - title: Github
    url: https://github.com/project-nsmg/universe
---

Universe is a project aimming to simulate a 2D universe. A working prototype can be found at [[1]](http://universe.ns.mg).

Physics
-------

### Light

Assume a star (which in 2D is a circle) that radiates equally in all directions with luminosity L. Flux density would be F = L/A = L/(2\*pi\*r)

### Gravity

As [people have pointed out](http://physics.stackexchange.com/questions/30652/what-is-the-2d-gravity-potential "http://physics.stackexchange.com/questions/30652/what-is-the-2d-gravity-potential"), the gravitational potential in 2D world is different. The potential can be described as F\_g(2D)=-(2GMm/r)\*r\_0. Therefore, the gravitational force is F(2D)=2GMm/r.
