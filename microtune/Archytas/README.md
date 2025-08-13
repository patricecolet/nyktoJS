## Archytas

### Principe
Système basé sur des échelles linéaires (progressions arithmétiques) reproduites à l’octave et compatibles avec une écriture sur portée traditionnelle.

### Gammes
- B6: 6 notes par octave
- B7: 7 notes par octave

### Polyphonie et routage MIDI
- 1 entrée MIDI → 16 sorties MIDI (canaux 1 à 16) pour la polyphonie.
- Chaque note entrante est allouée à un canal voix; le Pitch Bend est envoyé par canal.
- Sliders: « Premier canal de voix » et « Dernier canal de voix » pour définir la plage de canaux de sortie (p.ex. CH1–CH8).

### Résolution et conversion Pitch Bend
- Résolution: 4096 unités = 1 demi‑ton.
- Référence d’entrée: 14 bits, 0..16383 avec centre à 8192.
- Convention Archytas: 8192 = centre, 16383 = +1 « ton » microtonal, 0 = −1 « ton » microtonal.

Définitions par gamme:
- B7: pas microtonal (en demi‑tons) \( s_{B7} = 12/7 \), soit en unités \( U_{B7} = 4096\times 12/7 \).
- B6: pas microtonal (en demi‑tons) \( s_{B6} = 12/6 = 2 \), soit en unités \( U_{B6} = 4096\times 2 = 8192 \).

Entrée Pitch Bend normalisée:
- \( pb \in [0,16383] \), \( u = (pb - 8192)/8192 \in [-1,1] \).
- Décalage en « tons » Archytas: \( d = u \) (±1 au maximum).

Conversion vers un offset de hauteur:
- En demi‑tons: \( \Delta_{semi}(note) = \Delta_{base,semi}(note) + u\cdot s_{B\times} \)
- En unités 4096: \( \Delta_{units}(note) = \Delta_{base,units}(note) + u\cdot U_{B\times} \)

Envoi du Pitch Bend (14 bits) avec une plage d’instrument \( R \) (en demi‑tons):
- \( bend14 = \mathrm{clamp}\big(8192 + (\Delta_{semi}/R)\cdot 8192,\ 0,\ 16383\big) \)

Où \( \Delta_{base} \) provient de la table de correspondance (réaccordage de la classe de note de l’entrée vers le degré de la gamme et son offset).

### Contraintes liées au Pitch Bend et gestion des Note Off
- Le Pitch Bend est défini par canal MIDI: une seule valeur possible à un instant donné sur un canal.
- La polyphonie microtonale nécessite la répartition des notes sur plusieurs canaux (jusqu’à 16), chacun recevant son propre Pitch Bend.
- Entrée traitée comme monophonique: à chaque nouvelle Note On, la note en cours est éteinte immédiatement (Note Off envoyée), et la Note Off d’origine correspondante sera filtrée lorsqu’elle arrivera pour éviter un double arrêt.

### Table de correspondance B7 (par octave)
Format: entrée mod( note_midi, 12 ) → ( sortie mod( note_midi, 12 ), valeur_pitchbend )

- 0 → (0, 0)
- (1, 2) → (2, 2539)
- (3, 4) → (4, 2867)
- (5, 6) → (6, 1475)
- 7 → (7, 6717)
- (8, 9) → (9, 2703)
- (10, 11) → (10, 5898)

Remarques:
- Les paires d’entrée indiquent que les deux degrés partagent la même sortie et le même offset de Pitch Bend.
- Les valeurs de Pitch Bend ci‑dessus sont données dans l’unité 4096 = 1 demi‑ton.
