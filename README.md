# Box Calculator

A single-page parametric calculator for designing a modular optical box frame. The app calculates frame dimensions, section counts, component quantities, and simple engineering blueprint views from a small set of design inputs.

The project is intentionally lightweight: it is plain HTML, CSS, and JavaScript with no build step, package manager, framework, or runtime dependency.

## Live GitHub Pages Deployment

GitHub Pages expects a file named `index.html` at the root of the published branch when someone visits the default project URL:

```text
https://<github-user>.github.io/Box_calculator/
```

The original project only had `Box_calculator.html`, so the default Pages URL had no root entry point to serve. The app could only be reached by typing the full case-sensitive file path:

```text
https://<github-user>.github.io/Box_calculator/Box_calculator.html
```

This repository now includes `index.html`, which redirects the default GitHub Pages URL to the calculator page.

## Files

```text
.
|-- index.html            # GitHub Pages entry point
|-- Box_calculator.html   # Main calculator app
`-- README.md             # Project documentation
```

## Running Locally

Because this is a static app, you can open the main file directly in a browser:

```text
Box_calculator.html
```

You can also run a local static server from the repository root:

```bash
python3 -m http.server 8000
```

Then open:

```text
http://localhost:8000/
```

The root URL will redirect to:

```text
http://localhost:8000/Box_calculator.html
```

## Deploying With GitHub Pages

1. Commit `index.html`, `Box_calculator.html`, and `README.md`.
2. Push the repository to GitHub.
3. Open the repository on GitHub.
4. Go to `Settings` -> `Pages`.
5. Under `Build and deployment`, choose `Deploy from a branch`.
6. Select the branch you want to publish, usually `main`.
7. Select the root folder, usually `/ (root)`.
8. Save the settings.
9. Wait for GitHub Pages to finish deploying.

After deployment, the app should be available at:

```text
https://<github-user>.github.io/Box_calculator/
```

If the repository name is changed, replace `Box_calculator` in the URL with the new repository name.

## GitHub Pages Troubleshooting

If the deployed page does not load, check the following:

- Confirm `index.html` exists at the root of the branch selected in `Settings` -> `Pages`.
- Confirm GitHub Pages is publishing from the correct branch and folder.
- Use the exact file casing if opening the app directly. GitHub Pages URLs are case-sensitive, so `Box_calculator.html` and `box_calculator.html` are different paths.
- Wait a few minutes after changing Pages settings or pushing new files. Deployments are not always instant.
- Check the repository's `Actions` tab if GitHub Pages reports a failed deployment.
- Clear the browser cache or open the page in a private window if an old 404 page is cached.

## How The Calculator Works

The app reads user input values from the form and recalculates immediately when any input changes.

Main inputs:

- `Table Hole Spacing (mm)`: distance between table holes.
- `Width X`: the X-axis span, entered either as a direct millimeter value or as a hole count.
- `Depth Y`: the Y-axis span, entered either as a direct millimeter value or as a hole count.
- `Target Total Height (mm)`: full target height, including feet.
- `Max Span Limit (mm)`: maximum allowed module span before the frame is divided into more sections.
- `Feet Height (mm)`: height of each foot.
- `Rail Profile Width (mm)`: rail profile width used for drawing and material dimensions.
- `Rail Profile Length (mm)`: rail profile length/depth used for drawing and material dimensions.
- `Sheet Cap (mm)`: extra overlap added to each edge of side sheets and lid sheets.

Calculated dimensions:

```text
if X is entered in mm:
    width x = x mm value

if X is entered in holes:
    width x = (x hole count - 1) * table hole spacing

if Y is entered in mm:
    depth y = y mm value

if Y is entered in holes:
    depth y = (y hole count - 1) * table hole spacing

post height = target total height - feet height
x sections = ceiling(width x / max span limit)
y sections = ceiling(depth y / max span limit)
module length = width x / x sections
module width = depth y / y sections
x rail cut length = (width x - (x sections + 1) * rail profile length) / x sections
y rail cut length = (depth y - (y sections + 1) * rail profile width) / y sections
front/back side sheet length = x rail cut length - 2 * sheet cap
left/right side sheet length = y rail cut length - 2 * sheet cap
lid sheet length = front/back side sheet length
lid sheet width = left/right side sheet length
```

Hole count means the number of actual holes across the span, not the number of gaps between holes. For example, 49 holes at 25 mm spacing creates a 1200 mm span because there are 48 spacing intervals between the first and last hole.

When an axis is entered directly in millimeters, the axis input is visually greyed and the note below the input shows the nearest table hole count and its resulting table span.

The horizontal rail cut lengths are the clear distances between adjacent vertical posts. Each row or column has one more post than section count, so the calculator subtracts all post footprints along that axis before dividing the remaining clear span.

Sheet cap is treated as an inset allowance, so it is subtracted from both edges of the clear opening.

The bill of materials is generated from those section counts and dimensions. The canvas drawings show:

- Top view grid layout.
- Front view along the X-axis.
- Side view along the Y-axis.
- Rotatable 3D model with hover dimensions for individual parts.

## Browser Support

The app uses standard browser APIs:

- HTML form inputs.
- Inline CSS.
- Plain JavaScript.
- Canvas 2D drawing.

It should run in current desktop and mobile browsers without additional dependencies.

## Development Notes

- There is no build step.
- There are no external assets.
- There are no third-party JavaScript libraries.
- `Box_calculator.html` contains the app UI, styles, calculations, and canvas rendering.
- `index.html` exists only so GitHub Pages has a default root document.
