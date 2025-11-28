# Matrix Plotter

## 2D Array Traversal Visualization Tool

## Overview

An interactive Java application that visualizes various 2D array traversal patterns on a grid. Users can observe different matrix traversal algorithms in real-time, including row-major, column-major, diagonal, serpentine, spiral, and other creative fill patterns. This project demonstrates fundamental concepts of nested loops and array indexing through animated visual feedback.

## Author

- **Anmol Dubey**

## Features

| Feature | Description |
|---------|-------------|
| Interactive GUI | Button-driven interface for each traversal pattern |
| Real-Time Animation | Watch cells fill incrementally as traversal progresses |
| Speed Control | Adjustable animation speed via slider |
| Color Customization | Choose drawing and background colors |
| Resizable Grid | Create grids of different dimensions |
| Multiple Patterns | 14+ different traversal algorithms |

## Traversal Patterns

### Basic Traversals

| Pattern | Description | Direction |
|---------|-------------|-----------|
| Row-Major Fill | Standard left-to-right, top-to-bottom | → ↓ |
| Column-Major Fill | Top-to-bottom, left-to-right | ↓ → |
| Reverse Row-Major | Right-to-left, bottom-to-top | ← ↑ |
| Reverse Column-Major | Bottom-to-top, right-to-left | ↑ ← |

### Diagonal Patterns

| Pattern | Description |
|---------|-------------|
| Main Diagonal Line | Upper-left to lower-right diagonal |
| Other Diagonal Line | Upper-right to lower-left diagonal |
| Both Diagonals | X pattern across grid |
| Main Triangle Fill | Fill on and below main diagonal |
| Other Triangle Fill | Fill on and below anti-diagonal |
| Diagonal Fill | Fill using upward diagonal sweeps |

### Advanced Patterns

| Pattern | Description |
|---------|-------------|
| Serpentine Fill | Alternating left-right direction each row |
| Border Lines | Clockwise outline of grid perimeter |
| Spiral Fill | Clockwise spiral from outside to center |
| Random Fill | Random cell selection until complete |

## Visual Examples

### Row-Major Traversal
```
Start → [1] [2] [3] [4]
        [5] [6] [7] [8]
        [9] [10][11][12]
        [13][14][15][16] → End
```

### Column-Major Traversal
```
Start   ↓
   ↓  [1] [5] [9] [13]
   ↓  [2] [6] [10][14]
   ↓  [3] [7] [11][15]
      [4] [8] [12][16]
                    ↓
                   End
```

### Serpentine Traversal
```
Start → [1] [2] [3] [4]
        [8] [7] [6] [5] ←
      → [9] [10][11][12]
        [16][15][14][13] ← End
```

### Spiral Traversal
```
Start → [1] [2] [3] [4]
        [12][13][14][5]
        [11][16][15][6]
        [10][9] [8] [7]
                    ↓
             Center/End
```

### Main Diagonal
```
[●] [ ] [ ] [ ]
[ ] [●] [ ] [ ]
[ ] [ ] [●] [ ]
[ ] [ ] [ ] [●]
```

### Both Diagonals (X Pattern)
```
[●] [ ] [ ] [●]
[ ] [●] [●] [ ]
[ ] [●] [●] [ ]
[●] [ ] [ ] [●]
```

## Implementation Details

### Core Data Structure

```java
private boolean[][] colorArray;
```

A 2D boolean array where:
- `true` = cell is filled (drawing color)
- `false` = cell is empty (background color)

### Key Methods

| Method | Pattern | Loop Structure |
|--------|---------|----------------|
| `onRowMajorFillButtonClick()` | Row-major | `for r` → `for c` |
| `onColMajorFillButtonClick()` | Column-major | `for c` → `for r` |
| `onMainDiagonalLineButtonClick()` | Main diagonal | `if r == c` |
| `onOtherDiagonalLineButtonClick()` | Anti-diagonal | `if r + c == n-1` |
| `onSerpentineFillButtonClick()` | Serpentine | Alternating direction flag |
| `onSpiralFillButtonClick()` | Spiral | Boundary tracking |

### Example Implementation: Row-Major Fill

```java
public void onRowMajorFillButtonClick() {
    clearGrid();
    for (int r = 0; r < colorArray.length; r++) {
        for (int c = 0; c < colorArray[r].length; c++) {
            colorArray[r][c] = true;
            gui.update(colorArray);
        }
    }
}
```

### Example Implementation: Serpentine Fill

```java
public void onSerpentineFillButtonClick() {
    clearGrid();
    boolean leftToRight = true;
    for (int r = 0; r < colorArray.length; r++) {
        if (leftToRight) {
            for (int c = 0; c < colorArray.length; c++) {
                colorArray[r][c] = true;
                gui.update(colorArray);
            }
        } else {
            for (int c = colorArray.length - 1; c >= 0; c--) {
                colorArray[r][c] = true;
                gui.update(colorArray);
            }
        }
        leftToRight = !leftToRight;
    }
}
```

### Example Implementation: Main Diagonal

```java
public void onMainDiagonalLineButtonClick() {
    clearGrid();
    for (int r = 0; r < colorArray.length; r++) {
        for (int c = 0; c < colorArray.length; c++) {
            if (r == c) {
                colorArray[r][c] = true;
                gui.update(colorArray);
            }
        }
    }
}
```

## Architecture

```
┌─────────────────────────────────────────────────────────┐
│                    ArrayPlotter                          │
│  ┌─────────────────────────────────────────────────┐    │
│  │  boolean[][] colorArray                          │    │
│  │  - Stores grid state                            │    │
│  │  - true = filled, false = empty                 │    │
│  └─────────────────────────────────────────────────┘    │
│                         │                                │
│                         ▼                                │
│  ┌─────────────────────────────────────────────────┐    │
│  │  Traversal Methods                               │    │
│  │  - onRowMajorFillButtonClick()                  │    │
│  │  - onColMajorFillButtonClick()                  │    │
│  │  - onSerpentineFillButtonClick()                │    │
│  │  - ... (14+ patterns)                           │    │
│  └─────────────────────────────────────────────────┘    │
│                         │                                │
│                         ▼                                │
│  ┌─────────────────────────────────────────────────┐    │
│  │  gui.update(colorArray)                          │    │
│  │  - Syncs array state to visual grid             │    │
│  │  - Applies animation delay                      │    │
│  └─────────────────────────────────────────────────┘    │
└─────────────────────────────────────────────────────────┘
                          │
                          ▼
┌─────────────────────────────────────────────────────────┐
│                  ArrayPlotterGUI                         │
│  - Renders grid visually                                │
│  - Generates buttons from method names                  │
│  - Handles color selection                              │
│  - Controls animation speed                             │
└─────────────────────────────────────────────────────────┘
```

## File Structure

```
MatrixPlotter Files/
├── README.md                    # This file
├── ArrayPlotter.java            # Main logic and traversal algorithms
├── ArrayPlotterGUI.java         # GUI framework and display
├── MatrixPlotter Files.iml      # IntelliJ project configuration
├── package.bluej                # BlueJ project configuration
└── libs/                        # Required JAR dependencies
    ├── edu.tag.grid.jar
    └── info.gridworld.jar
```

## Usage

### Running the Application

1. Open project in BlueJ or IntelliJ IDEA
2. Compile both Java files
3. Run `ArrayPlotter.main()`

### Using the Interface

1. **Create Grid:** Click "New Grid" and specify dimensions
2. **Select Colors:** Choose drawing and background colors from menus
3. **Run Pattern:** Click any traversal button to watch animation
4. **Adjust Speed:** Use slider to control animation speed
5. **Clear:** Click "Clear Grid" to reset

### Adding Custom Patterns

Create new methods following the naming convention:

```java
public void onMyPatternButtonClick() {
    try {
        clearGrid();
        // Your traversal logic here
        for (int r = 0; r < colorArray.length; r++) {
            for (int c = 0; c < colorArray[r].length; c++) {
                // Set cells and update
                colorArray[r][c] = true;
                gui.update(colorArray);
            }
        }
    } catch (Exception e) {
        System.out.println("Something went wrong! " + e);
        clearGrid();
    }
}
```

The GUI automatically generates a button for any method matching `on*ButtonClick`.

## Concepts Demonstrated

### Array Indexing

```java
colorArray[row][column] = true;
```

Row-major storage means `[0][0]` is top-left, `[rows-1][cols-1]` is bottom-right.

### Nested Loop Patterns

| Pattern | Outer Loop | Inner Loop |
|---------|------------|------------|
| Row-major | Rows (0→n) | Columns (0→n) |
| Column-major | Columns (0→n) | Rows (0→n) |
| Reverse | Rows (n→0) | Columns (n→0) |

### Diagonal Conditions

```java
// Main diagonal: row equals column
if (r == c) { /* on diagonal */ }

// Anti-diagonal: row + column equals size - 1
if (r + c == colorArray.length - 1) { /* on anti-diagonal */ }

// Below main diagonal
if (c <= r) { /* on or below */ }
```

### Boundary Tracking (Spiral)

```java
int top = 0, bottom = n-1, left = 0, right = n-1;
while (top <= bottom && left <= right) {
    // Traverse top row, right column, bottom row, left column
    // Shrink boundaries after each layer
}
```

## Algorithm Complexity

| Pattern | Time Complexity | Space Complexity |
|---------|-----------------|------------------|
| Row/Column Major | O(n²) | O(1) |
| Diagonal Line | O(n²) or O(n) | O(1) |
| Triangle Fill | O(n²) | O(1) |
| Serpentine | O(n²) | O(1) |
| Spiral | O(n²) | O(1) |
| Random Fill | O(n² log n) expected | O(1) |

## Dependencies

- `edu.tag.grid` - Grid framework library
- `info.gridworld` - GridWorld AP CS library

## Educational Context

This project is designed for learning:

- 2D array declaration and initialization
- Nested loop construction
- Array bounds and indexing
- Conditional logic within loops
- Visual algorithm verification
- GUI event handling in Java
