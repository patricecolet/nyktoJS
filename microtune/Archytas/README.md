## Archytas — Échelles linéaires B6/B7

### Principe
Système basé sur des échelles linéaires (progressions arithmétiques) reproduites à l’octave et compatibles avec une écriture sur portée traditionnelle. Les intervalles sont construits à partir d’épimores (rapports superpartiels) issus de segments de série harmonique.

- B6: 6 degrés par octave (harmoniques 6→12)
- B7: 7 degrés par octave (harmoniques 7→14)

Réf. énoncé et contexte: Archytas – Échelles Linéaires
- https://www.collectif-archytas.com/echelles-lineaires/

### Règle de pitch bend (convention du projet)
- Plage supposée côté instrument: ±2 demi‑tons
- 8192 = +200 cents (2 demi‑tons)
- 1 cent = 40,96 pas 14‑bit
- Formule: bend14 = 8192 + 40,96 × Δcents (borné 0..16383)
- Encodage: LSB = bend14 & 0x7F, MSB = (bend14 >> 7) & 0x7F

Δcents est la déviation par rapport à l’ancrage ET standard: ET cible = 100 × classe (C=0, C#=100, …, B=1100). Exemple: 8/7 (≈231¢) pour C# (100¢) → Δcents = +131.

### Chromatique juste (à partir de C5)
| Degré | Ratio | Fréq (Hz) | Cents abs | ET cible (¢) | Δcents | bend14 | MSB | LSB |
|---|---|---:|---:|---:|---:|---:|---:|---:|
| C | 1/1 | 523.251 | 0 | 0 | 0 | 8192 | 64 | 0 |
| C# | 8/7 | 598.001 | 231 | 100 | +131 | 13558 | 105 | 118 |
| D | 7/6 | 610.460 | 267 | 200 | +67 | 10936 | 85 | 56 |
| D# | 9/7 | 672.751 | 435 | 300 | +135 | 13722 | 107 | 26 |
| E | 4/3 | 697.668 | 498 | 400 | +98 | 12206 | 95 | 46 |
| F | 10/7 | 747.502 | 617 | 500 | +117 | 12984 | 101 | 56 |
| F# | 3/2 | 784.877 | 702 | 600 | +102 | 12370 | 96 | 82 |
| G | 11/7 | 822.252 | 782 | 700 | +82 | 11551 | 90 | 31 |
| G# | 5/3 | 872.085 | 884 | 800 | +84 | 11633 | 90 | 113 |
| A | 12/7 | 897.002 | 933 | 900 | +33 | 9544 | 74 | 72 |
| A# | 11/6 | 959.294 | 1049 | 1000 | +49 | 10199 | 79 | 87 |
| B | 13/7 | 971.752 | 1072 | 1100 | −28 | 7045 | 55 | 5 |

### Sliders
- Through: bypass total du traitement (vérification A/B)
- Tonalité (tonic): rotation des classes (C=0..B=11) pour appliquer Δcents sur la tonalité voulue (ancrage ET conservé)

### Utilisation
- Insérer le plugin avant l’instrument.
- Régler la plage de pitch bend de l’instrument à ±2 demi‑tons.
- Vérifier au tuner: les fréquences doivent correspondre au tableau ci‑dessus.

### Notes
- Le pitch bend est par canal: en mode simple, on envoie sur un canal fixe.
- Pour une polyphonie microtonale multi‑canaux fiable, il faut une allocation par canal (MPE‑like) et une gestion stricte des Note Off; à réintroduire une fois la base validée.