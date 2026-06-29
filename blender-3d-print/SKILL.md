---
name: blender-3d-print
description: Modelowanie obudów do druku 3D w Blender — dobre praktyki, boolean workflow, bevel, eksport STL, troubleshooting geometrii. Używaj gdy użytkownik modeluje obudowę, tworzy otwory booleanem, ma problemy z geometrią (non-manifold, za duże pliki STL, błąd slicera), eksportuje do druku 3D lub pyta o rozdzielczość cylindrów/sfer w Blender.
---

## Rozdzielczość siatek — kluczowe dla druku 3D

Za duża rozdzielczość × wiele Boolean = eksplozja faces (1M+) → slicer nie wczyta pliku.

| Typ obiektu | Vertices / Segments | Przykład |
|-------------|---------------------|---------|
| Otwór fi < 8mm | **8–12** | LED fi 3mm, M3 fi 3.3mm |
| Otwór fi 8–20mm | **16** | BTN fi 12mm, DC fi 7.5mm |
| Otwór > 20mm / widoczna powierzchnia | **32** | zewnętrzne krzywizny |
| UV Sphere (boss, półkula) | **16 seg / 8 rings** | zawsze wystarczy |

## Kolejność operacji (nie skracaj)

```
1. Boolean Difference (każdy otwór)
2. Merge by Distance  ← po każdym boolenie
3. Non Manifold check ← przed przejściem dalej
4. Bevel Modifier
5. Apply Bevel + Merge by Distance
6. Ctrl+J (scalenie z bossami)
```

Bevel na brudnej geometrii mnoży błędy. Nigdy nie rób Ctrl+J z niezaaplikowanymi modyfikatorami — otwory znikają.

## Workflow — Boolean Difference

```
# Bool Tool (N panel):
Zaznacz target → Shift+klik cutter (active) → N panel → Edit → Difference

# Modifier (bardziej niezawodny przy złożonej geometrii):
Zaznacz target → Modifier Properties → Add → Boolean
Operation: Difference | Object: cutter | Solver: Exact → Apply → usuń cutter
```

Po każdym boolean:
```
Edit Mode → A → Mesh → Clean Up → Merge by Distance
Select → Select All by Trait → Non Manifold
→ jeśli coś zaznaczone: napraw zanim pójdziesz dalej
```

## Kontrola geometrii na etapach

Sprawdź faces w dolnym pasku Edit Mode:
- < 20 000 — ok
- 20 000–50 000 — akceptowalne
- > 100 000 — problem, napraw przed bevel

> ⚠️ 3D Print Toolbox (Check All) może zawiesić Blender przy złożonej geometrii. Używaj ręcznych metod poniżej.

**Ręczna weryfikacja:**
```
Edit Mode → A
Mesh → Clean Up → Merge by Distance
Mesh → Clean Up → Delete Loose
Alt+N → Recalculate Outside
Select → Select All by Trait → Non Manifold
→ jeśli zaznaczone: Mesh → Clean Up → Fill Holes lub usuń ręcznie
```

## Bevel — zaokrąglenie krawędzi

```
Modifier → Add → Generate → Bevel
Amount: 1–2mm
Segments: 3–4
Limit Method: Angle (30–45°) ← tylko ostre krawędzie, nie krawędzie otworów
Profile: 0.5
→ Apply → A → Mesh → Merge by Distance
```

## Eksport STL

Przed eksportem sprawdź rozmiar (cel: < 5MB dla prostej obudowy):
```
Edit Mode → A → patrz: Faces w dolnym pasku
Jeśli > 100 000: Mesh → Clean Up → Limited Dissolve (5°) → drastycznie redukuje bez zmiany kształtu
```

Eksport:
```
Zaznacz obiekt → File → Export → STL
✓ Selection Only
✓ Apply Modifiers  
✓ Apply Unit Scale
Forward: Y Forward | Up: Z Up
```

Docelowy rozmiar STL: < 5MB. > 20MB = za gęsta geometria.

## Troubleshooting — slicer nie wczytuje pliku

**"Loading of a model file failed"** = uszkodzona geometria lub non-manifold.

Naprawa przez terminal (Linux):
```bash
# Szybkie — admesh
sudo apt install admesh
admesh --fill-holes --normal-directions --normal-values plik.stl -b plik_fixed.stl

# Programowe — trimesh
pip install trimesh
python3 -c "
import trimesh
m = trimesh.load('plik.stl')
m.fill_holes()
m.fix_normals()
m.export('plik_fixed.stl')
print('faces:', len(m.faces), '| watertight:', m.is_watertight)
"
```
`is_watertight: True` = plik załaduje się w slicerze.

**Redukcja faces gdy Decimate Modifier nie działa:**
```
Edit Mode → A → Mesh → Clean Up → Limited Dissolve (5°)
lub: Mesh → Clean Up → Decimate Geometry (Ratio: 0.1)
```

**Otwory znikają po Ctrl+J:**
Niezaaplikowane Boolean modyfikatory. Przed Ctrl+J:
```
Zaznacz box → Modifier Properties → każdy Boolean → ▾ → Apply
```

**Pusty trójkąt po Knife tool:**
Brakuje ściany ukośnej (hypotenusa). Fix:
```
Edge Select (2) → zaznacz 4 krawędzie obwodu dziury → F
lub: Ctrl+E → Bridge Edge Loops
```

## Ustawienia sceny (jednorazowo)

```
Scene Properties → Units: Metric | Scale: 0.001 | Length: Millimeters
File → Defaults → Save Startup File
```
