# Text Set Operations (BST-based)

A command-line C program that treats the contents of text files as **sets** — of either paragraphs or sentences — and lets you run classic set operations on them: **union**, **intersection**, and **difference**.

Internally, each chunk of text (a paragraph or a sentence, your choice) is cleaned, normalized then stored in a **binary search tree**, which is used to insert, search, and de-duplicate entries.

## Features

- Two input modes (the code can be modified to handle n-number of files)
  - One file: find duplicate paragraphs/sentences within a single file, or run a difference between two paragraphs you pick from it.
  - Two files: run union, intersection, or difference between the two files' contents.
- Two granularity levels: operate on whole paragraphs or individual sentences.
- Automatic text normalization: lowercases, strips punctuation, and collapses whitespace before comparing, so "Hello,  World!" and "hello world" are treated as the same entry.
- Results saved to RESULT.txt after every operation.

## Build

    gcc main.c Operation_Trees.c -o sets

## Run

    ./sets

You'll get a menu:

    1. Choose method     - one file / two files
    2. Choose level      - paragraph / sentence
    3. Load file(s)      - first file, and second file if using two
    4. Run operation     - union / intersection / difference
    5. Exit

## Example

Given a.txt:

    The sky is blue today.

    Rain is expected tomorrow.

and b.txt:

    The sky is blue today.

    Snow is expected this weekend.

Running an **intersection** at sentence level returns:

    the sky is blue today

Running a **difference** (a − b) returns:

    rain is expected tomorrow

## The Way It Works

- Files are read paragraph by paragraph (a paragraph = text between blank lines).
- If sentence-level mode is selected, each paragraph is further split on `. ! ?` into sentences.
- Each part is cleaned (superClean module) and inserted into a BST.
- Set operations traverse the trees:
  - Union: insert everything from both trees.
  - Intersection: the common elements between tree A and tree B.
  - Difference: what is in tree A and not in tree B.
- Duplicate detection (for the one-file mode) tracks elements seen once vs. seen again while walking the file.

## Why a BST?

- Sorted organization: keeps entries ordered automatically, unlike dynamic arrays or linked lists which need explicit sorting.
- Faster operations: offers better average-case speed for searching, inserting, and deleting compared to linear structures.
- Set-logic friendly: simplifies implementing union, intersection, and difference, since each is just a tree traversal plus a lookup.
- Efficient duplicate checking: avoids scanning every element one-by-one, making duplicate detection close to instantaneous on average.

## Notes

This was originally a school assignment (Data Structures, 2024/2025) built to practice BST implementation and file I/O in C. Graded 16.5/20 — understanding the core of the code **line by line** was taken into consideration for the mark.

