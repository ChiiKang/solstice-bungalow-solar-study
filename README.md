# Solstice — Bungalow Solar Study

An interactive sales concept built from the supplied bungalow photograph. Assumed front bearing: 228° SW; example location: Kuala Lumpur. Geometry is illustrative, not surveyed.

**[Launch the live demo](https://solstice-bungalow-study.dashshund.chatgpt.site)** · No installation needed.

## Screenshots

### Detailed bungalow and solar dashboard

![Detailed 3D bungalow with solar panels and output dashboard](docs/preview.jpg)

### Morning shading scenario

![Morning shading at 08:50 showing shaded panels and reduced solar output](docs/shading.jpg)

This what-if example uses 13 m neighbouring roof-edge height and 3 m side clearance. It demonstrates obstruction losses; these dimensions are not measurements of the photographed property. Screenshots use the software compatibility renderer; browsers with WebGL use accelerated rendering.

## What you can explore

- Orbit the detailed bungalow, inspect individual panels, and orient the camera with the compass.
- Scrub time to see sunlight, shading and estimated power change together.
- Compare three panel layouts and edit placement directly on the roof.
- Change dimensions, orientation, surrounding obstructions and equipment assumptions.
- Review daily energy, shading losses and a printable proposal.

## Run

Use Node.js 20 or newer and npm. This is a standalone JavaScript + Three.js application built with Vite. No API keys or backend are needed.

```sh
npm ci
npm run dev
npm run build
```

## Demo walkthrough

1. Orbit the house; click a module to inspect irradiance, DC power, obstruction and roof direction.
2. Scrub the local-time slider or play the daylight animation. Switch between appearance, instantaneous shading and daily direct-sun hours.
3. Compare A and B (18 modules each), then C (24). All values are recomputed from the same scenario.
4. Open Assumptions. For an obvious shading example, set neighbour roof-edge height to 13 m and side clearance to 3 m. At approximately 08:50, layout B has visible morning obstruction shade. These are what-if values, not claims about the real property.
5. Edit design to move, add, rotate or remove panels. Roof-edge, gable and overlap checks reject unsuitable placements. Undo restores the previous geometry and layout.
6. Click compass directions to move the camera. N resets to a north-up overhead view. Camera movement never changes the house bearing.
7. Open Proposal for a printable snapshot and an optional installer-supplied quote.

## Calculation model

NOAA-style solar-position approximation; clear-sky irradiance; direct-light obstruction at nine sample points per panel; isotropic diffuse and reflected irradiance; temperature adjustment; 6% fixed DC losses; 96% inverter conversion with AC clipping. Daily energy integrates 15-minute samples. Roof heat-map samples use 30-minute spacing.

The system treats modules independently and omits electrical string mismatch, bypass diodes, diffuse sky obstruction, weather data, annual forecasting, and financial return. Roof and module geometry, trees, surrounding houses, location, module rating, inverter size, and temperature are editable. Invalid modules are excluded from output.

Three.js supplies geometry, raycasting, camera controls and accelerated shadows. A software depth-buffer renderer provides orbitable geometry and panel heat layers when WebGL is unavailable; that compatibility view uses a simplified directional shadow map. Shading calculations are identical in both modes.

The app is client-only. Changes are session-local and reset on reload; print a proposal to retain a scenario. No server, API key, or personal data collection is required. WebMCP tools are feature detected when the browser supports them.

## Verification

Checked layout count/fit (18/18/24), zero production at night, unobstructed bounds, reduced energy with taller neighbours, invalidation after roof resizing, and production build. Browser UI checks cover panel inspection, assumptions/recalculation, time controls, shade layer, comparison, panel removal/undo, compass and proposal. The QA browser uses the software compatibility view because WebGL is disabled there.

## Architectural reconstruction

The revised model reconstructs the visible recessed upper terrace/car porch, projecting front bay, gable and tiled apron, crossing rear hip, framed windows, coloured terrace door, masonry balustrades, porch columns, gutters, downpipes, roof-mounted water heater, paving, twin-leaf gate, boundary walls, side passage and trees. Tiles and ridge caps are geometry with colour variation. Detail camera provides a closer view. The rear elevation and dimensions remain inferred from a single reference photograph; this is not a measured digital twin. PV placement excludes the crossing roofs and water-heater footprint, and shading uses their obstruction geometry.

## Source map

- `src/architecture.js`: detailed bungalow, tiled roofs, balcony, gate, landscape and surroundings.
- `src/model.js`: roof surfaces, module geometry, placement constraints and layouts.
- `src/solar.js`: solar position, irradiance, obstruction sampling and energy estimates.
- `src/main.js`: interactive scene, dashboard and controls.
- `src/compatibility.js`, `src/software-shadow.js`: software rendering fallback.
- `index.html`, `style.css`: interface structure and styling.

## Hosting elsewhere

Run `npm run build` and serve the generated `dist` directory from a static web host. The default asset paths expect deployment at the domain root. For GitHub Pages under a repository subdirectory, use a matching Vite base path and make the reference-image and favicon URLs relative before building.
