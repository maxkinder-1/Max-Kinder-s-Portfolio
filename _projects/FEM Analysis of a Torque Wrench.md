---
layout: project
title: "FEM Wrench Design"
subtitle: "Part of Mechanics of Engineering Materials"
date: 2025-12-10
image: /assets/images/wec_rendering.PNG
tags: [ANSYS, Design]
---

### Summary
The final project in the MAE 3270: Mechanics of Materials course was to identify and analyze a wrench design and material so that it could meet a series of safety factor constraints, including yield strength, fracture toughness, and cyclic loading. At first this analysis was performed using beam analysis and once an approximate solution was found it was tested and refined using FEM analysis in ANSYS Static Structural. 

### Identifying Design Range
Safety factor 
- $$X_0$$ >= 4 for yield or brittle failure, depending on the material.
- $$X_K$$ >= 2 for crack growth with an assumed 0.04 in crach length.
- $$X_S$$ >= 1.5 for fatigue stress at 1,000,000 cycles.

```matlab
M = 600; % max torque (in-lbf)
L = 16; % length from drive to where load applied (inches)
h = 0.50; % width
b = 0.80; % thickness
c = 1.0; % distance from center of drive to center of strain gauge
E = 11.13E6; % Young's modulus (psi)
nu = 0.33; % Poisson's ratio
su = 143.5E3; % tensile strength use yield or ultimate depending on material (psi)
KIC = 81.9E3; % fracture toughness (psi sqrt(in))
sfatigue = 90.e3; % fatigue strength from Granta for 10^6 cycles
name = 'Ti-12Mo'; % material name

%Stress and deflection analysis
%Calculate load point diflection
load = M/L %calculate applied load (lbf)
I = ((h^3)*b)/12 %calculate rectangluar inertia
lpd = (load*(L^3))/(3*E*I) %calculate load point diflection

%Calculate max normal stress
mns = M*(h/2)/I %calculate max normal stress

%Calculate safety factor for strength
sfs = su/mns %calculate safety factor for strength

%Calculate safety factor for crack growth
Ki = 1.12*mns*sqrt(pi*0.04) %calculate stress intensity factor
sfcg = KIC/Ki %calculate safety factor for crack growth

%Calculate safety factor for fatigue
ans = (mns-mns)/2 %calculate mean stress per loading
ampns = mns-ans %calculate amplitude of stress per loading
ss = sqrt((ans+ampns)*ampns)
sff = sfatigue/ss

%Calculate strain at guage
gM = load*(L-c) %calculate the moment at the guage
gmns = gM*(h/2)/I %calculate the normal stress at guage
strain = (gmns/E) %calculate strain at guage

%Calculate output of the guage
output = 1.013*strain*1000 %calculate the output of the strain guage (Guage factor was calculated from given values)

%Check if the material passes the requirements
if output >= 1
   disp("passes")
else
   disp("fails")
end
if sfs <= 4
   disp("fails")
else
   disp("passes")
end
if sfcg <= 2
   disp("fails")
else
   disp("passes")
end
if sfs <= 1.5
   disp("fails")
else
end


```
