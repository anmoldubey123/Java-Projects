# Photoshop Lab

## Image Processing and Manipulation in Java

## Overview

A comprehensive image processing application implemented in Java that provides Photoshop-like functionality for manipulating digital images. This project demonstrates fundamental concepts of digital image processing through pixel-level manipulation, including color transformations, filters, mirroring, edge detection, and steganography.

## Author

- **Anmol Dubey**

## Features

| Category | Features |
|----------|----------|
| Color Manipulation | Zero blue, keep only blue, negate, grayscale, tint, posterize, solarize |
| Transformations | Mirror vertical/horizontal, flip, right-to-left mirror |
| Filters | Simple blur, variable radius blur, edge detection |
| Advanced | Chromakey (green screen), steganography encode/decode |
| Utilities | Picture viewer GUI with zoom, pixel inspector |

## Image Processing Operations

### Color Transformations

| Operation | Description | Formula/Logic |
|-----------|-------------|---------------|
| `zeroBlue()` | Remove all blue | `blue = 0` |
| `keepOnlyBlue()` | Remove red and green | `red = 0, green = 0` |
| `negate()` | Invert colors | `color = 255 - color` |
| `grayscale()` | Convert to grayscale | `gray = (R + G + B) / 3` |
| `tint(r, g, b)` | Adjust color intensity | `color = color × coefficient` |
| `posterize(span)` | Reduce color levels | `color = (color / span) × span` |
| `solarize(threshold)` | Film overexposure effect | Invert if `color < threshold` |

### Visual Examples

#### Grayscale Conversion
```
Original RGB (150, 100, 50)
Average = (150 + 100 + 50) / 3 = 100
Result = (100, 100, 100) → Gray pixel
```

#### Negate (Color Inversion)
```
Original: (200, 100, 50)
Negated:  (55, 155, 205)
         (255-200, 255-100, 255-50)
```

#### Posterize Effect
```
Span = 64
Original Red = 200
Posterized = (200 / 64) × 64 = 192
Result: Reduced color palette
```

### Mirror Operations

| Operation | Description | Direction |
|-----------|-------------|-----------|
| `mirrorVertical()` | Mirror left to right | ← \| → |
| `mirrorRightToLeft()` | Mirror right to left | → \| ← |
| `mirrorHorizontal()` | Mirror top to bottom | ↑ — ↓ |
| `verticalFlip()` | Flip upside down | ↕ swap |

```
mirrorVertical():           mirrorHorizontal():
┌───┬───┐                   ┌───────┐
│ A │ A'│                   │   A   │
│ B │ B'│    ──────►        ├───────┤
│ C │ C'│                   │   A'  │
└───┴───┘                   └───────┘
Left copied                 Top copied
to right                    to bottom
```

### Blur Algorithms

#### Simple Blur
Averages the current pixel with its 4 direct neighbors (up, down, left, right):

```
    [N]
[W] [C] [E]
    [S]

New color = (C + N + S + E + W) / 5
```

#### Variable Radius Blur
Averages all pixels within a square of given radius:

```java
public Picture blur(int radius) {
    // For each pixel, average all pixels in
    // (2×radius) × (2×radius) neighborhood
}
```

### Edge Detection

Detects edges by measuring color distance between adjacent pixels:

```java
public void edgeDetection(int dist) {
    // Compare each pixel to its right neighbor
    // If colorDistance > dist: mark as edge (black)
    // Otherwise: mark as non-edge (white)
}
```

**Color Distance Formula:**
```
distance = √[(R₁-R₂)² + (G₁-G₂)² + (B₁-B₂)²]
```

### Chromakey (Green Screen)

Replaces pixels of a target color with pixels from another image:

```java
public void chromakey(Picture other, Color color, int dist) {
    // For each pixel in this picture:
    // If colorDistance(pixel, color) <= dist:
    //     Replace with corresponding pixel from 'other'
}
```

**Use Case:** Replace blue/green background with custom background image.

### Steganography

Hide secret messages within images by manipulating least significant bits.

#### Encode
```java
public void encode(Picture msg) {
    // Step 1: Make all red values even (clear LSB)
    // Step 2: For black pixels in msg, make red odd (set LSB)
}
```

#### Decode
```java
public Picture decode() {
    // Extract hidden message by checking red LSB
    // Odd red value → black pixel (message)
    // Even red value → white pixel (background)
}
```

**Principle:** The human eye cannot detect single-bit changes in color values.

## Architecture

### Class Diagram

```
┌─────────────────────────────────────────────────────────┐
│                        Picture                          │
├─────────────────────────────────────────────────────────┤
│ - pixels: Pixel[][]                                     │
├─────────────────────────────────────────────────────────┤
│ + Picture(String filename)                              │
│ + Picture(int height, int width)                        │
│ + Picture(Color color, int height, int width)           │
│ + Picture(Picture other)                                │
├─────────────────────────────────────────────────────────┤
│ + getWidth(): int                                       │
│ + getHeight(): int                                      │
│ + getPixel(int x, int y): Pixel                        │
│ + setPixel(int x, int y, Pixel pixel): void            │
│ + getPixels(): Pixel[][]                               │
│ + view(): PictureViewer                                │
├─────────────────────────────────────────────────────────┤
│ // Color Operations                                     │
│ + zeroBlue(): void                                      │
│ + keepOnlyBlue(): void                                  │
│ + negate(): void                                        │
│ + grayscale(): void                                     │
│ + tint(double r, double g, double b): void             │
│ + posterize(int span): void                            │
│ + solarize(int threshold): void                        │
├─────────────────────────────────────────────────────────┤
│ // Transform Operations                                 │
│ + mirrorVertical(): void                               │
│ + mirrorRightToLeft(): void                            │
│ + mirrorHorizontal(): void                             │
│ + verticalFlip(): void                                 │
├─────────────────────────────────────────────────────────┤
│ // Filter Operations                                    │
│ + simpleBlur(): Picture                                │
│ + blur(int radius): Picture                            │
│ + edgeDetection(int dist): void                        │
├─────────────────────────────────────────────────────────┤
│ // Advanced Operations                                  │
│ + chromakey(Picture other, Color c, int dist): void    │
│ + encode(Picture msg): void                            │
│ + decode(): Picture                                    │
└─────────────────────────────────────────────────────────┘
                          │
                          ▼
┌─────────────────────────────────────────────────────────┐
│                         Pixel                           │
├─────────────────────────────────────────────────────────┤
│ - red: int                                              │
│ - green: int                                            │
│ - blue: int                                             │
├─────────────────────────────────────────────────────────┤
│ + Pixel(int red, int green, int blue)                  │
│ + Pixel(Color color)                                   │
│ + getRed(): int                                        │
│ + setRed(int red): void                                │
│ + getGreen(): int                                      │
│ + setGreen(int green): void                            │
│ + getBlue(): int                                       │
│ + setBlue(int blue): void                              │
│ + getColor(): Color                                    │
│ + setColor(int r, int g, int b): void                  │
│ + setColor(Color color): void                          │
│ + colorDistance(Color testColor): double               │
│ + equals(Object other): boolean                        │
│ + toString(): String                                   │
└─────────────────────────────────────────────────────────┘
```

### Data Structure

Images are represented as 2D arrays of Pixel objects:

```java
private Pixel[][] pixels;  // pixels[row][col] or pixels[y][x]
```

**Coordinate System:**
- `pixels[0][0]` = top-left corner
- `pixels[height-1][width-1]` = bottom-right corner
- Row index = y coordinate
- Column index = x coordinate

## File Structure

```
Photoshop Lab/
├── README.md                    # This file
├── Photoshop Lab.iml           # IntelliJ project config
├── src/
│   ├── Picture.java            # Main image class with operations
│   ├── Pixel.java              # RGB pixel representation
│   ├── PictureViewer.java      # GUI for viewing images
│   └── PictureTester.java      # Test driver
├── images/                      # Input images directory
│   ├── beach.jpg
│   ├── swan.jpg
│   ├── temple.jpg
│   ├── blue-mark.jpg
│   ├── moon-surface.jpg
│   └── msg.jpg
└── doc/                         # Javadoc documentation
    ├── index.html
    ├── Picture.html
    ├── Pixel.html
    └── stylesheet.css
```

## Usage

### Running the Application

```java
public class PictureTester {
    public static void main(String[] args) {
        // Load an image
        Picture beach = new Picture("beach.jpg");
        beach.view();  // Display original
        
        // Apply transformations
        beach.grayscale();
        beach.view();  // Display result
    }
}
```

### Example Operations

#### Basic Color Manipulation
```java
Picture pic = new Picture("beach.jpg");
pic.view();           // Original

pic.zeroBlue();       // Remove blue channel
pic.view();

pic.negate();         // Invert colors
pic.view();

pic.grayscale();      // Convert to grayscale
pic.view();
```

#### Mirror and Transform
```java
Picture pic = new Picture("temple.jpg");
pic.mirrorVertical();  // Create symmetry
pic.view();

pic.verticalFlip();    // Flip upside down
pic.view();
```

#### Edge Detection
```java
Picture swan = new Picture("swan.jpg");
swan.view();                // Original
swan.edgeDetection(10);     // Detect edges
swan.view();                // Edge map
```

#### Chromakey (Green Screen)
```java
Picture person = new Picture("blue-mark.jpg");
Picture background = new Picture("moon-surface.jpg");

person.view();
background.view();

// Replace blue background with moon surface
person.chromakey(background, new Color(10, 40, 75), 60);
person.view();
```

#### Steganography
```java
// Hide a message
Picture carrier = new Picture("beach.jpg");
Picture message = new Picture("msg.jpg");

carrier.encode(message);
carrier.view();  // Looks unchanged

// Extract hidden message
Picture hidden = carrier.decode();
hidden.view();   // Reveals the secret message
```

## GUI Features

The `PictureViewer` provides:

| Feature | Description |
|---------|-------------|
| Image Display | Renders the picture in a scrollable window |
| Pixel Inspector | Click to see RGB values at any location |
| Zoom | Multiple zoom levels (100% to 1200%) |
| Crosshair | Visual indicator of selected pixel |

```
┌─────────────────────────────────────┐
│ Row: 150, Col: 200                  │
│ Red = 128, Green = 64, Blue = 192   │
├─────────────────────────────────────┤
│                                     │
│         [Image Display]             │
│              +                      │
│         (crosshair at               │
│          selected pixel)            │
│                                     │
└─────────────────────────────────────┘
```

## Implementation Details

### Pixel Access Patterns

```java
// Row-major traversal (standard)
for (int row = 0; row < pixels.length; row++) {
    for (int col = 0; col < pixels[0].length; col++) {
        Pixel p = pixels[row][col];
        // Process pixel
    }
}

// Enhanced for-loop (when index not needed)
for (Pixel[] rowArray : pixels) {
    for (Pixel pixel : rowArray) {
        // Process pixel
    }
}
```

### Color Clamping

Values must stay within valid range [0, 255]:

```java
// Example from tint()
if ((int)(pixel.getRed() * red) > 255)
    pixel.setRed(255);
else
    pixel.setRed((int)(pixel.getRed() * red));
```

### Creating New Pictures

Some operations return new pictures rather than modifying in-place:

```java
public Picture blur(int radius) {
    Picture result = new Picture(pixels.length, pixels[0].length);
    // ... compute blur into result
    return result;
}
```

## Concepts Demonstrated

| Concept | Application |
|---------|-------------|
| 2D Arrays | Image as pixel grid |
| Nested Loops | Pixel traversal |
| Object-Oriented Design | Picture and Pixel classes |
| Color Theory | RGB color model |
| Image Processing | Filters, transforms |
| Bit Manipulation | Steganography LSB |
| GUI Programming | Swing-based viewer |
| File I/O | Image loading via ImageIO |

## Algorithm Complexity

| Operation | Time Complexity | Space Complexity |
|-----------|-----------------|------------------|
| Color transforms | O(n) | O(1) |
| Mirror operations | O(n) | O(1) |
| Simple blur | O(n) | O(n) |
| Blur with radius r | O(n × r²) | O(n) |
| Edge detection | O(n) | O(1) |
| Chromakey | O(n) | O(1) |
| Encode/Decode | O(n) | O(n) for decode |

Where n = width × height (total pixels).

## Dependencies

- Java SE 8 or higher
- `javax.imageio` for image file handling
- `java.awt` for Color and graphics
- `javax.swing` for GUI components

## Credits

- **GUI Development:** Ben Wyatt (Liberty student, 2017)
- **Visual Tweaks:** Mr. Bunn
- **Algorithm Implementation:** Anmol Dubey
