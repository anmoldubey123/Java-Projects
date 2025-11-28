# Mountain Climber

## Greedy Pathfinding Through Topographic Elevation Data

## Overview

A Java application that visualizes topographic elevation data and finds optimal hiking paths across mountainous terrain using a greedy algorithm. The program reads elevation data from text files, renders grayscale terrain maps, and calculates the path of least elevation change from west to east.

## Author

- **Anmol Dubey**

## Features

| Feature | Description |
|---------|-------------|
| Elevation Parsing | Load terrain data from text files into 2D arrays |
| Grayscale Rendering | Visualize elevation as shaded terrain map |
| Greedy Pathfinding | Find low-elevation-change paths west to east |
| Optimal Path Detection | Identify best starting row across all possibilities |
| Interactive Display | DrawingPanel GUI with zoom and save capabilities |

## Problem Description

Given a rectangular grid of elevation data representing mountainous terrain, find a path from the western edge to the eastern edge that minimizes total elevation change. This simulates a hiker trying to cross a mountain range while avoiding steep climbs and descents.

### The Greedy Algorithm

At each step, the algorithm chooses the next position that minimizes immediate elevation change:

```
Current Position: (row, col)
Three Choices:
  1. Forward:      (row, col+1)      - move straight east
  2. Forward-Up:   (row-1, col+1)    - move northeast  
  3. Forward-Down: (row+1, col+1)    - move southeast

Decision Rule:
  - Calculate |elevation_change| for each valid option
  - Choose the option with minimum change
  - Ties: prefer straight → up → down
```

### Visual Representation

```
                    WEST → EAST
         col:  0    1    2    3    4    ...
              ┌────┬────┬────┬────┬────┐
    row 0     │2100│2150│2080│2200│2100│
              ├────┼────┼────┼────┼────┤
    row 1  →  │2050│2060│2055│2070│2040│  ← Path travels east
              ├────┼────┼────┼────┼────┤
    row 2     │2200│2180│2190│2150│2120│
              └────┴────┴────┴────┴────┘

From position (1,1) with elevation 2060:
  Forward (1,2):      |2055 - 2060| = 5   ← BEST
  Forward-Up (0,2):   |2080 - 2060| = 20
  Forward-Down (2,2): |2190 - 2060| = 130
```

## Algorithm Details

### Grayscale Mapping

Elevations are mapped to grayscale values (0-255) using linear interpolation:

```
grayscale = (elevation - min) / (max - min) × 255

Where:
  min = minimum elevation in entire grid
  max = maximum elevation in entire grid
  
Example:
  min = 1500 ft, max = 4500 ft, elevation = 3000 ft
  grayscale = (3000 - 1500) / (4500 - 1500) × 255 = 127.5 ≈ 128
```

### Path Selection Rules

```java
// At each column, evaluate three options:
int forward     = |grid[row][col+1] - current|
int forwardUp   = |grid[row-1][col+1] - current|  // if row > 0
int forwardDown = |grid[row+1][col+1] - current|  // if row < rows-1

// Choose minimum, with tie-breaking preference:
// 1. Forward (straight)
// 2. Forward-Up (northeast)  
// 3. Forward-Down (southeast)
```

### Finding the Optimal Path

```java
// Test all possible starting rows
int bestRow = 0;
int bestChange = Integer.MAX_VALUE;

for (int row = 0; row < rows; row++) {
    int change = drawLowestElevPath(g, row);
    if (change < bestChange) {
        bestChange = change;
        bestRow = row;
    }
}
```

## Class Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                      MapDataDrawer                          │
├─────────────────────────────────────────────────────────────┤
│ - grid: int[][]                                             │
├─────────────────────────────────────────────────────────────┤
│ + MapDataDrawer(String fileName, int rows, int cols)        │
│ + findMin(): int                                            │
│ + findMax(): int                                            │
│ + drawMap(Graphics g): void                                 │
│ + drawLowestElevPath(Graphics g, int row): int              │
│ + indexOfLowestElevPath(Graphics g): int                    │
│ + getRows(): int                                            │
│ + getCols(): int                                            │
└─────────────────────────────────────────────────────────────┘
                              │
                              │ uses
                              ▼
┌─────────────────────────────────────────────────────────────┐
│                      DrawingPanel                           │
├─────────────────────────────────────────────────────────────┤
│ + DrawingPanel(int width, int height)                       │
│ + getGraphics(): Graphics2D                                 │
│ + setBackground(Color c): void                              │
│ + zoom(int factor): void                                    │
│ + save(String filename): void                               │
└─────────────────────────────────────────────────────────────┘
                              │
                              │ uses
                              ▼
┌─────────────────────────────────────────────────────────────┐
│                        Runner                               │
├─────────────────────────────────────────────────────────────┤
│ + main(String[] args): void                                 │
│   - Load elevation data                                     │
│   - Create visualization panel                              │
│   - Draw terrain map                                        │
│   - Find and display optimal path                           │
└─────────────────────────────────────────────────────────────┘
```

## Implementation Guide

### Step 1: Constructor - Parse Elevation Data

```java
public MapDataDrawer(String fileName, int rows, int cols) throws IOException {
    grid = new int[rows][cols];
    Scanner scanner = new Scanner(new File(fileName));
    
    for (int r = 0; r < rows; r++) {
        for (int c = 0; c < cols; c++) {
            grid[r][c] = scanner.nextInt();
        }
    }
    scanner.close();
}
```

### Step 2: Find Min/Max Elevations

```java
public int findMin() {
    int min = Integer.MAX_VALUE;
    for (int[] row : grid) {
        for (int elevation : row) {
            if (elevation < min) min = elevation;
        }
    }
    return min;
}

public int findMax() {
    int max = Integer.MIN_VALUE;
    for (int[] row : grid) {
        for (int elevation : row) {
            if (elevation > max) max = elevation;
        }
    }
    return max;
}
```

### Step 3: Draw Grayscale Map

```java
public void drawMap(Graphics g) {
    int min = findMin();
    int max = findMax();
    
    for (int r = 0; r < grid.length; r++) {
        for (int c = 0; c < grid[0].length; c++) {
            int gray = (int) ((grid[r][c] - min) * 255.0 / (max - min));
            g.setColor(new Color(gray, gray, gray));
            g.fillRect(c, r, 1, 1);  // x=col, y=row
        }
    }
}
```

### Step 4: Greedy Path Algorithm

```java
public int drawLowestElevPath(Graphics g, int row) {
    int totalChange = 0;
    int currentRow = row;
    
    for (int col = 0; col < grid[0].length - 1; col++) {
        g.fillRect(col, currentRow, 1, 1);  // draw current position
        
        int current = grid[currentRow][col];
        int forward = Math.abs(grid[currentRow][col + 1] - current);
        
        int forwardUp = Integer.MAX_VALUE;
        if (currentRow > 0) {
            forwardUp = Math.abs(grid[currentRow - 1][col + 1] - current);
        }
        
        int forwardDown = Integer.MAX_VALUE;
        if (currentRow < grid.length - 1) {
            forwardDown = Math.abs(grid[currentRow + 1][col + 1] - current);
        }
        
        // Choose minimum with tie-breaking
        int minChange = Math.min(forward, Math.min(forwardUp, forwardDown));
        
        if (forward == minChange) {
            totalChange += forward;
            // currentRow stays same
        } else if (forwardUp == minChange) {
            totalChange += forwardUp;
            currentRow--;
        } else {
            totalChange += forwardDown;
            currentRow++;
        }
    }
    
    // Draw final position
    g.fillRect(grid[0].length - 1, currentRow, 1, 1);
    
    return totalChange;
}
```

### Step 5: Find Best Starting Row

```java
public int indexOfLowestElevPath(Graphics g) {
    int bestRow = 0;
    int bestChange = Integer.MAX_VALUE;
    
    for (int row = 0; row < grid.length; row++) {
        int change = drawLowestElevPath(g, row);
        if (change < bestChange) {
            bestChange = change;
            bestRow = row;
        }
    }
    
    return bestRow;
}
```

## File Structure

```
mountainClimberPractical/
├── README.md                           # This file
├── 01 MountainClimber starter code and data/
│   ├── MapDataDrawer.java              # Main algorithm implementation
│   ├── DrawingPanel.java               # Graphics library (provided)
│   ├── Runner.java                     # Test driver
│   ├── package.bluej                   # BlueJ project config
│   └── Colorado_480x480.txt            # Elevation data file
└── out/
    └── production/
        └── mountainClimberPractical/   # Compiled classes
```

## Data File Format

The elevation data file contains space-separated integer values representing elevation in feet:

```
Colorado_480x480.txt:
- 480 rows × 480 columns
- Each value: elevation in feet
- Row-major order (left to right, top to bottom)
- Values range approximately 5000-12000 ft

Format:
elevation[0][0] elevation[0][1] ... elevation[0][479]
elevation[1][0] elevation[1][1] ... elevation[1][479]
...
elevation[479][0] elevation[479][1] ... elevation[479][479]
```

## Usage

### Running the Application

```java
// In Runner.java main():
MapDataDrawer map = new MapDataDrawer("Colorado_480x480.txt", 480, 480);
DrawingPanel panel = new DrawingPanel(map.getCols(), map.getRows());
Graphics g = panel.getGraphics();

// Draw terrain
map.drawMap(g);

// Draw all paths in red
g.setColor(Color.RED);
int bestRow = map.indexOfLowestElevPath(g);

// Highlight best path in green
g.setColor(Color.GREEN);
int totalChange = map.drawLowestElevPath(g, bestRow);

System.out.println("Best path starts at row: " + bestRow);
System.out.println("Total elevation change: " + totalChange);
```

### Expected Output

```
Min value in map: 5765
Max value in map: 11676
Lowest-Elevation-Change Path starting at row 200 gives total change of: 3245
The Lowest-Elevation-Change Path starts at row: 285 and gives a total change of: 2847
```

## Visualization

```
┌────────────────────────────────────────────────┐
│░░░░░▓▓▓▓▓▓███████▓▓▓▓░░░░░░░░░░░░░░░░░░░░░░░░│  Light = Low elevation
│░░░░▓▓▓████████████▓▓▓░░░░░░░░░░░░░░░░░░░░░░░░│  Dark = High elevation
│░░░▓▓█████████████████▓░░░░░░░░░░░░░░░░░░░░░░░│
│░░▓▓███████████████████▓░░░░░░░░░░░░░░░░░░░░░░│
│░░▓████████████████████▓░░░░░░░░░░░░░░░░░░░░░░│
│░▓█████████████████████▓▓░░░░░░░░░░░░░░░░░░░░░│
│═══════════════════════════════════════════════│  ← Green = Best path
│░▓▓████████████████████▓▓░░░░░░░░░░░░░░░░░░░░░│
│░░▓▓██████████████████▓▓░░░░░░░░░░░░░░░░░░░░░░│
│░░░▓▓████████████████▓▓░░░░░░░░░░░░░░░░░░░░░░░│
└────────────────────────────────────────────────┘
 WEST                                        EAST
```

## Algorithm Complexity

| Operation | Time | Space |
|-----------|------|-------|
| Constructor (parse file) | O(rows × cols) | O(rows × cols) |
| findMin() / findMax() | O(rows × cols) | O(1) |
| drawMap() | O(rows × cols) | O(1) |
| drawLowestElevPath() | O(cols) | O(1) |
| indexOfLowestElevPath() | O(rows × cols) | O(1) |

## Limitations

The greedy algorithm does not guarantee the globally optimal path. It makes locally optimal decisions at each step, which may miss a better overall path that requires temporarily climbing to reach a lower-cost route.

```
Example where greedy fails:

     A ──(10)── B ──(10)── C
     │                      │
    (5)                   (100)
     │                      │
     D ──(5)─── E ──(5)─── F

Greedy from A: A→D→E→F (cost: 15) ✓ Actually optimal here
But in complex terrain, local minima can trap the algorithm
```

## Extensions

Potential improvements:

- **Dynamic Programming:** Find true optimal path using DP
- **A* Search:** Use heuristics for better pathfinding
- **Multiple Paths:** Show top N best paths
- **Bidirectional:** Find paths in both directions
- **3D Visualization:** Render terrain in 3D
- **Animation:** Animate the path-finding process

## Dependencies

- Java SE 8 or higher
- DrawingPanel library (provided)

## Credits

- **DrawingPanel:** Marty Stepp & Stuart Reges, University of Washington
- **Elevation Data:** USGS National Elevation Dataset
