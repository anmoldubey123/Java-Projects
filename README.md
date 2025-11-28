## Java Projects Collection

A comprehensive collection of programming projects spanning image processing, algorithms, data structures, visualization tools, and text analysis. These projects demonstrate fundamental computer science concepts through practical implementations.

## Author

- **Anmol Dubey**

## Repository Overview

This repository contains educational programming projects developed in Java, covering topics from basic algorithms to advanced image manipulation and pathfinding.

## Projects

| Project | Description | Key Concepts |
|---------|-------------|--------------|
| [Photoshop Lab](#photoshop-lab) | Image processing and manipulation | Pixel manipulation, filters, steganography |
| [Sieve of Eratosthenes](#sieve-of-eratosthenes) | Prime number generation | Classical algorithms, optimization |
| [Mountain Climber](#mountain-climber) | Topographic pathfinding | Greedy algorithms, 2D arrays, visualization |
| [Word Cloud](#word-cloud) | Text frequency analysis | File I/O, data structures, sorting |

---

## Project Details

### Photoshop Lab

**Location:** `Photoshop Lab/`

A comprehensive image processing application implementing Photoshop-like functionality through pixel-level manipulation.

**Features:**
- Color transformations (grayscale, negate, tint, posterize, solarize)
- Mirror and flip operations
- Blur filters (simple and variable radius)
- Edge detection
- Chromakey (green screen)
- Steganography (encode/decode hidden messages)

**Key Classes:**
- `Picture.java` - Main image class with processing operations
- `Pixel.java` - RGB pixel representation
- `PictureViewer.java` - GUI for viewing images

```
Photoshop Lab/
├── src/
│   ├── Picture.java
│   ├── Pixel.java
│   ├── PictureViewer.java
│   └── PictureTester.java
├── images/
└── doc/
```

---

### Sieve of Eratosthenes

**Location:** `SieveEratosthenes/`

Implementation of the ancient algorithm for efficiently finding all prime numbers up to a specified limit.

**Algorithm:**
1. Create boolean array marking all numbers as potentially prime
2. Starting from 2, mark all multiples as composite
3. Move to next unmarked number and repeat
4. Remaining unmarked numbers are prime

**Complexity:** O(n log log n) time, O(n) space

```
SieveEratosthenes/
└── src/
    └── SieveSolver.java
```

---

### Mountain Climber

**Location:** `mountainClimberPractical/`

Visualizes topographic elevation data and finds optimal hiking paths using a greedy algorithm. Renders grayscale terrain maps and calculates paths of least elevation change from west to east.

**Features:**
- Parse elevation data from text files
- Render grayscale terrain visualization
- Greedy pathfinding algorithm
- Optimal starting row detection
- Interactive DrawingPanel GUI

**Key Classes:**
- `MapDataDrawer.java` - Algorithm implementation
- `DrawingPanel.java` - Graphics library
- `Runner.java` - Test driver

**Sample Data:** Colorado elevation data (480×480 grid)

```
mountainClimberPractical/
└── 01 MountainClimber starter code and data/
    ├── MapDataDrawer.java
    ├── DrawingPanel.java
    ├── Runner.java
    └── Colorado_480x480.txt
```

---

### Word Cloud

**Location:** `wordCloud/`

Analyzes text documents to identify word frequency patterns for word cloud generation. Parses input files, counts occurrences, and identifies most frequent words.

**Features:**
- Text file parsing and tokenization
- Word frequency counting
- Top hits identification
- Statistics reporting

**Key Classes:**
- `WordCloud.java` - Analysis engine
- `Word.java` - Word data class
- `Runner.java` - Entry point

**Sample Data:** Martin Luther King Jr.'s "I Have a Dream" speech

```
wordCloud/
├── src/
│   ├── WordCloud.java
│   ├── Word.java
│   └── Runner.java
└── dream.txt
```

---

## Concepts Demonstrated

### Data Structures
- 2D Arrays (image pixels, elevation grids)
- ArrayList (word collections)
- Boolean arrays (prime sieve)

### Algorithms
- Greedy pathfinding
- Sieve of Eratosthenes
- Linear search and sorting
- Image convolution (blur, edge detection)

### Programming Techniques
- File I/O and parsing
- Object-oriented design
- GUI programming with Swing
- Bit manipulation (steganography)

### Mathematical Concepts
- Color theory (RGB model)
- Grayscale mapping
- Euclidean distance (color distance)
- Prime number theory

---

## Quick Start

### Prerequisites
- Java SE 8 or higher
- IDE (IntelliJ IDEA, BlueJ, or Eclipse recommended)

### Running a Project

```bash
# Navigate to project source directory
cd "Photoshop Lab/src"

# Compile
javac *.java

# Run
java PictureTester
```

### Project-Specific Instructions

| Project | Main Class | Command |
|---------|-----------|---------|
| Photoshop Lab | `PictureTester` | `java PictureTester` |
| Sieve | `SieveSolver` | `java SieveSolver` |
| Mountain Climber | `Runner` | `java Runner` |
| Word Cloud | `Runner` | `java Runner` |

---

## Repository Structure

```
/
├── README.md                          # This file
│
├── Photoshop Lab/
│   ├── README.md
│   ├── src/
│   ├── images/
│   └── doc/
│
├── SieveEratosthenes/
│   ├── README.md
│   └── src/
│
├── mountainClimberPractical/
│   ├── README.md
│   └── 01 MountainClimber starter code and data/
│
└── wordCloud/
    ├── README.md
    ├── src/
    └── dream.txt
```

---

## Learning Outcomes

Each project reinforces specific computer science fundamentals:

| Project | Primary Learning Objectives |
|---------|----------------------------|
| Photoshop Lab | 2D array traversal, pixel manipulation, image formats |
| Sieve of Eratosthenes | Algorithm optimization, space-time tradeoffs |
| Mountain Climber | Greedy algorithms, file parsing, visualization |
| Word Cloud | Collections, sorting, text processing |

---

## Future Enhancements

Potential improvements across projects:

- **Photoshop Lab:** Add more filters, layer support, undo/redo
- **Sieve:** Segmented sieve for larger ranges, parallel processing
- **Mountain Climber:** Dynamic programming for optimal path, 3D visualization
- **Word Cloud:** Stop word filtering, stemming, actual visual output

---

## Dependencies

All projects use standard Java libraries:
- `java.awt` - Graphics and color
- `java.io` - File I/O
- `java.util` - Collections and utilities
- `javax.swing` - GUI components
- `javax.imageio` - Image file handling

No external dependencies required.

---

## License

Educational use - Programming coursework and personal learning projects.

---

## Acknowledgments

- **DrawingPanel:** Marty Stepp & Stuart Reges, University of Washington
- **Elevation Data:** USGS National Elevation Dataset
- **Sample Text:** Martin Luther King Jr., "I Have a Dream" (1963)

---

*This portfolio demonstrates progressive skill development in Java programming, from basic algorithms to complex image processing and data visualization.*
