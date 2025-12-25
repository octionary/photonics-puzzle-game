# Lightpath - Specification

## Vision
A Shenzhen I/O-inspired puzzle game about designing integrated photonic circuits. Players design optical systems to meet specifications by connecting components and tuning parameters.

## Core Loop
1. Receive contract/challenge with narrative framing
2. Design circuit by placing and connecting components on grid
3. Configure components (static parameters) and instruments (via script)
4. Run simulation, observe results
5. Iterate until spec met
6. Get scored on area/power/loss, unlock next challenges

## Scope (Prototype)
- Browser-based single HTML file with embedded JS/CSS
- Grid-based component placement with SVG rendering
- 6 components: Laser, Waveguide, Directional Coupler, Ring Resonator, Phase Shifter, Photodetector
- Steady-state complex amplitude simulation
- JavaScript instrument API (PyVISA-like)
- 6 puzzles across 2 arcs (Basics + Sensing)
- Basic scoring (component count, area)

## Out of Scope
- Save/load
- Sound/animations beyond basic
- Time-domain simulation
- Leaderboards
