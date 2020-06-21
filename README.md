# Tone Transposer Library

## Introduction

This is a library that takes a series of tones and transposes them into all 12 musical key signatures. 

It is designed to help musicians analyze tone patterns on a piano.


## Transposing

To use this library, first create a Transpose object. This takes in the name of a root note and a list of tones. This is an example
of a C major triad:

```
t = Transpose("C",["C","E","G"])
```

Next, you can either print a quick summary to the output window of all transpositions:
```
t.print_positions()
```

Or you can return an iterable object, and cycle through and print them:

```
for p in t.get_all_positions():
    print(p.root_tone.get_note_name(),p.to_string())
```

This is an example of the output:
```
C | C, E, G
Db | Db, F, Ab
D | D, Gb, A
Eb | Eb, G, Bb
E | E, Ab, B
F | F, A, C
Gb | Gb, Bb, Db
G | G, B, D
Ab | Ab, C, Eb
A | A, Db, E
Bb | Bb, D, F
B | B, Eb, Gb
```

## Notation

You can change the notation that is presented by passing a NoteNameType object value to the 'to_string()' method. 
Here are some values that can be passed: NoteNameType.Flat, NoteNameType.Sharp, NoteNameType.Enharmonic. Every method in the library
defaults to 'Enharmonic' (i.e showing both both the sharp and flat) when no value is passed through.

```
print(p.root_tone.get_note_name(NoteNameType.Flat),p.to_string(NoteNameType.Flat))
```

## Interval numbers

This demonstrates how to print the underlying intervals for a chord:

```
t = Transpose("C",["C","Eb","G","Bb"])
t.print_intervals()
```

```
['R', 'b3_#2', '5', 'b7_#6']
```

You can also interate through these intervals using the 'get_intervals()' method:

```
t = Transpose("C",["C","Eb","G","Bb"])
for p in t.get_intervals(NoteNameType.Flat):
    print(p)
```

```
R
b3
5
b7
```

## Grouping Transpositions

The library allows you to group the different transpositions by their shape on a piano keyboard.

```
t = Transpose("C",["C","Eb","G","Bb"])
t.print_key_pattern(NoteNameType.Flat)
```
```
wwww | 3 | D,E,A
wbwb | 2 | C,F
bwbw | 2 | Db,Gb
bbbb | 1 | Eb
wbww | 1 | G
bwbb | 1 | Ab
bbwb | 1 | Bb
wwbw | 1 | B
```

The first column indicates the pattern of white and black notes of each transposition. The second column indicates the number of
transpositions that have that shape on the piano. The third column indicates which musical keys have that shape.

You can also iterate through the summary like so:

```
for s in t.get_key_pattern_summary(NoteNameType):
    print(s.key_pattern, s.count, s.root_notes)
```

By default, the summary is sorted descending by the count of shapes. However, you can change the sort order and also
column that is sorted. This demonstrates the arguments you need to pass through to change the order:

```
for s in t.get_key_pattern_summary(NoteNameType.Flat,SortKeys.KeyPattern,Direction.Ascending):
    print(s.key_pattern, s.count, s.root_notes)
```
```
bbbb 1 Eb
bbwb 1 Bb
bwbb 1 Ab
bwbw 2 Db,Gb
wbwb 2 C,F
wbww 1 G
wwbw 1 B
wwww 3 D,E,A
```

Direction can only be 'Ascending' and 'Descending', however the columns for sorting can be any of the following:

```
SortKeys.KeyPattern, SortKeys.Count, SortKeys.WhiteCount, SortKeys.BlackCount
```



