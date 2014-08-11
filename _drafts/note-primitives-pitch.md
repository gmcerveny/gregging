# Practical Primitives for representing notes : Pitch

One of smallest units in which we can deal with music is the note. A note tells us both pitch and duration, and when later placed in a sequence or score, timing.  Computers have delt with these as numerical values, but when analyzing and conversing about music, this does little good.  A few abstractions can come in handy.

Rightfully so, iOS has no built in abstraction for representing musical data.  So here are a few C representations I'm using to make this easier. FN: using C is crucial for the Core Audio render thread.

## Pitch

Pitch can be easily represented by the western set of notes.  This largely looks like this:

```
C, C# or Db, D, D# or Eb, F, F# or Gb, G, G# or Ab, A, A# or Bb, B
```

To create primitive values for this a simple variable constant makes it easy.  And note names themselves are ambiguous (a note may live in any of 8 or so octaves depending on the instrument/voice).

```
int C = 60,
  C# = Db = 61,
  D = 62,
  D# = Eb = 63,
  E = 64
  F = 65,
  F# = Gb = 66,
  G = 67,
  G# = Ab = 68,
  A = 69,
  A# = Bb = 70,
  B = 71
```

## Keys and Scales

Keys are a set of related notes, with scales being the sequence. There are different modes for keys, and scales that have different orders up and down, but for the purpose of explanation, let's leave this for now.

A C major scale looks like this:

```
C D E F G A B C
```

By looking at our note values, or the semitones on piano, we can see we do not have an even distribution between the notes.

```
60, 62, 64, 65, 67, 69, 71, 72
 0,  2,  2,  1,  2,  2,  2,  1
```

### Degrees and Position

In traditional computer programming the first element in an array is referenced by the index 0.  But when conversing about music, we usually refer to the first degree or root of a key as 1.  

```
static ints[] notes = {C, D, E, F, G, A, B, C};

static int note(int degree) {
  return notes[degree - 1];
}

static pitch degree(note a) {
  // return pitch for degree
}
```

### Octaves

We also may refer to notes out side of scales, but those repeat in units of 7.

```
(degree % 7 - 1);
```

### Intervals

Another usefull division is the interval.  An interval is the relative distance between two notes in degrees.

So, in C Major, from C-G is 5, but so is D-A.

```
static int interval(pitch a, pitch b) {
  return degree(b) - degree(a);
}

static pitch pitch(pitch a, interval int ) {
  int origin = degree(a);
  return note(a + interval);
}
```