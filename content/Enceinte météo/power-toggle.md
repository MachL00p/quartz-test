---
id: power-toggle
aliases:
  - "Power toggle"
tags: []
---
# Libraries
- **subprocess:** allows spawning of new processes and to connect their inputs, outputs or error pipes and their return codes. The recommended approach is to use the ```python: subprocess.run()``` command for most use cases.
- **gpiozero:** simple GPIO library

# Code
```python
#!/usr/bin/env python3
"""Arrêt propre de la Pi quand l'interrupteur passe en position OFF.

Câblage : interrupteur entre GPIO3 (broche 5) et GND.
  - fermé  (ON)  -> GPIO3 à la masse
  - ouvert (OFF) -> GPIO3 tiré à 3,3 V par le pull-up -> arrêt
"""
import subprocess
import sys
import time

from gpiozero import Button

PIN = 3               # GPIO3 (broche physique 5)
CONFIRM_DELAY = 1.0   # l'état OFF doit durer 1 s pour être pris en compte
DRY_RUN = "--test" in sys.argv  # affiche au lieu d'éteindre

# pull_up=True : "pressed" = broche à la masse = interrupteur sur ON
switch = Button(PIN, pull_up=True, bounce_time=0.05)


def main():
    print("Surveillance de l'interrupteur (GPIO3)...", flush=True)
    while True:
        # Bloque tant que l'interrupteur est sur ON
        switch.wait_for_release()

        # Confirme que l'état OFF est stable (évite les parasites)
        time.sleep(CONFIRM_DELAY)
        if switch.is_pressed:
            continue  # c'était un faux contact

        if DRY_RUN:
            print("OFF détecté : la Pi s'éteindrait maintenant.", flush=True)
            switch.wait_for_press()
            print("Retour sur ON.", flush=True)
            continue

        print("OFF détecté : arrêt de la Pi.", flush=True)
        subprocess.run(["systemctl", "poweroff"])
        time.sleep(120)  # on attend l'arrêt, sans relancer la commande


if __name__ == "__main__":
    main()
```
