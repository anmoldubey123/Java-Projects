# Word Cloud

## Text Analysis and Word Frequency Visualization

## Overview

A Java application that analyzes text documents to identify word frequency patterns and generate word cloud data. The program parses input text files, counts word occurrences, and identifies the most frequently used words for visualization purposes.

## Author

- **Anmol Dubey**

## Features

| Feature | Description |
|---------|-------------|
| Text Parsing | Load and tokenize text from files |
| Word Counting | Track frequency of each unique word |
| Top Hits | Identify most frequently occurring words |
| Statistics | Report total and unique word counts |

## How It Works

### Word Frequency Analysis

```
Input Text: "I have a dream that one day this nation will rise up"

Processing:
┌─────────┬───────┐
│  Word   │ Count │
├─────────┼───────┤
│ I       │   1   │
│ have    │   1   │
│ a       │   1   │
│ dream   │   1   │
│ that    │   1   │
│ one     │   1   │
│ day     │   1   │
│ this    │   1   │
│ nation  │   1   │
│ will    │   1   │
│ rise    │   1   │
│ up      │   1   │
└─────────┴───────┘

Top Hits: Words sorted by frequency (descending)
```

### Algorithm Flow

```
┌──────────────┐     ┌───────────────┐     ┌──────────────┐
│  Load File   │────▶│  Tokenize &   │────▶│   Count      │
│              │     │  Parse Words  │     │   Frequency  │
└──────────────┘     └───────────────┘     └──────────────┘
                                                   │
                                                   ▼
┌──────────────┐     ┌───────────────┐     ┌──────────────┐
│  Display     │◀────│  Sort by      │◀────│  Find Top    │
│  Results     │     │  Count        │     │  Hits        │
└──────────────┘     └───────────────┘     └──────────────┘
```

## Class Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                         Word                                │
├─────────────────────────────────────────────────────────────┤
│ - word: String                                              │
│ - count: int                                                │
├─────────────────────────────────────────────────────────────┤
│ + Word(String w)                                            │
│ + getWord(): String                                         │
│ + getCount(): int                                           │
│ + increment(): void                                         │
│ + toString(): String                                        │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│                       WordCloud                             │
├─────────────────────────────────────────────────────────────┤
│ - words: ArrayList<Word>                                    │
│ - topHits: ArrayList<Word>                                  │
│ - totalWords: int                                           │
│ - uniqueWords: int                                          │
├─────────────────────────────────────────────────────────────┤
│ + WordCloud(String fileName)                                │
│ - getIndex(String str): int                                 │
│ - load(String fileName): void                               │
│ - findTopHits(): void                                       │
│ + getTopHits(): ArrayList<Word>                             │
│ + printInfo(): void                                         │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│                        Runner                               │
├─────────────────────────────────────────────────────────────┤
│ + main(String[] args): void                                 │
└─────────────────────────────────────────────────────────────┘
```

## Implementation Details

### Word Class

Encapsulates a word and its occurrence count:

```java
public class Word {
    private String word;
    private int count;

    public Word(String w) {
        word = w;
    }

    public void increment() {
        count++;
    }

    public String toString() {
        return word + "\t\t" + count;
    }
}
```

### WordCloud Class

Manages the collection of words and frequency analysis:

```java
public class WordCloud {
    private ArrayList<Word> words;
    private ArrayList<Word> topHits;

    // Search for existing word
    private int getIndex(String str) {
        for (int i = 0; i < words.size(); i++) {
            if (words.get(i).getWord().equals(str)) {
                return i;
            }
        }
        return -1;
    }

    // Load and parse file
    private void load(String fileName) {
        Scanner input = new Scanner(new File(fileName));
        while (input.hasNext()) {
            String word = input.next();
            int index = getIndex(word);
            if (index != -1) {
                words.get(index).increment();
            } else {
                words.add(new Word(word));
            }
        }
        findTopHits();
    }

    // Find most frequent words
    private void findTopHits() {
        // Sort by frequency and select top N
    }
}
```

## File Structure

```
wordCloud/
├── README.md               # This file
├── wordCloud.iml          # IntelliJ project config
├── dream.txt              # Sample input: MLK "I Have a Dream"
└── src/
    ├── Runner.java        # Main entry point
    ├── Word.java          # Word data class
    └── WordCloud.java     # Analysis engine
```

## Usage

### Running the Application

```java
public class Runner {
    public static void main(String[] args) throws FileNotFoundException {
        WordCloud tester = new WordCloud("dream.txt");
    }
}
```

### Sample Output

```
Total # of Words >>> 1667
Total # of unique Words >>> 526

Top Words:
1)  the         110
2)  of          99
3)  to          59
4)  and         54
5)  a           37
6)  will        27
7)  freedom     20
8)  we          17
9)  that        17
10) be          17
...
```

## Sample Input

The project includes Martin Luther King Jr.'s "I Have a Dream" speech (1963) as sample text for analysis. Key themes emerge through word frequency:

| Rank | Word | Count | Significance |
|------|------|-------|--------------|
| 1 | the | 110 | Common article |
| 2 | of | 99 | Common preposition |
| 7 | freedom | 20 | Core theme |
| - | dream | 11 | Title/theme word |
| - | justice | 8 | Key concept |
| - | Negro | 15 | Historical context |

## Word Cloud Visualization Concept

Word frequency maps to visual size in word clouds:

```
                    THE
              freedom    of
         will    WE    dream    to
      justice  NATION  ring   be  and
    Alabama  Mississippi  day  hope  faith
  children  mountain  together  let  great
```

*Higher frequency = larger size*

## Algorithm Complexity

| Operation | Time | Space |
|-----------|------|-------|
| Load file | O(n) | O(n) |
| getIndex() | O(m) | O(1) |
| Overall parsing | O(n × m) | O(m) |
| Sort for top hits | O(m log m) | O(m) |

Where n = total words, m = unique words

## Potential Enhancements

- **Stop Word Filtering:** Remove common words (the, a, of, etc.)
- **Case Normalization:** Treat "The" and "the" as same word
- **Punctuation Handling:** Strip punctuation from words
- **Visual Output:** Generate actual word cloud image
- **Stemming:** Group word variations (dream, dreams, dreaming)
- **N-gram Analysis:** Track common phrases, not just single words

### Enhanced Preprocessing Example

```java
private String normalize(String word) {
    // Remove punctuation
    word = word.replaceAll("[^a-zA-Z]", "");
    // Convert to lowercase
    word = word.toLowerCase();
    return word;
}

private boolean isStopWord(String word) {
    String[] stopWords = {"the", "a", "an", "of", "to", "and", "in", "is"};
    return Arrays.asList(stopWords).contains(word);
}
```

## Dependencies

- Java SE 8 or higher
- Standard Java libraries (java.io, java.util)


*Word clouds provide an intuitive visual representation of text data, highlighting the most prominent themes and concepts through size-based emphasis.*
