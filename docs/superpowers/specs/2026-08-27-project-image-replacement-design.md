# Project image replacement design

## Goal

Replace the five low-resolution project illustrations on the homepage with the exact originals supplied in `ruizhe_project_images_hd.zip`. Keep all layout, copy, links, and project ordering unchanged.

## Asset mapping

- `01_PACE.png` -> `assets/projects/pace.png`
- `02_MaskSource.png` -> `assets/projects/masksource.png`
- `03_BoneVibAuth.png` -> `assets/projects/bonevibauth.png`
- `04_Voice_Defense.png` -> `assets/projects/voice-defense.png`
- `05_ARC_Solver.png` -> `assets/projects/arc-solver.png`

## Integration

Copy the PNG files without recompression. Update both thumbnail `src` values and full-image `href` values in `index.html` to point to the new PNG assets. Remove obsolete low-resolution project images and incomplete encoded-image build data so that the repository has a single source of truth for each illustration.

## Verification

Confirm that every PNG is valid, has the expected 4:3 dimensions, and is referenced exactly twice in `index.html`. Check that no project image reference points to the obsolete SVG or WebP assets and that all referenced local files exist.
