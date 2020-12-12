# sangeetham

Representing carnatic music to support both notations, views, swarams, sahithyams, kanakku etc.

## Scope

### In Scope 

* Representing all aspects of (Indian) music notation
* Representing multiple parallel tracks.
* Representing swarams, konnakkol, sahithyam (including breakages) all in a similar rhythmic way.
* Document models for representing

### Out of Scope

* Formatting of layouts.  The goal is that the raw format when viewed as is should be a good enough view instead of needing any more special processing.  Anything more is an extension (say coloring etc).
* Non music document components.  We expect these "snippets" are embedded in another document format (such as html) via some special root tag so that if a user needs to place blocks of texts, pictures *outside* the musical notation they can use what ever the container allows (css etc).
* The *editing* model that describes how a song might be edited in some visual editor.

## Models

We want a document model for our notations.

A document is essentially a list of sections (pallavi, anupallavi etc) and these sections can have sections recursively.

At the leaf level we have a list of Lines (or cycles).  Each line consists of one or more Roles.

The idea is that a Line is played fully (start to end sequentially) at a time but all roles in a Line are to be played in parallel.

For example the Swaram and Sahithyam for say a Pallavi's first line could constitute the roles for the first line (which itself may span multiple cycles).

```
Document = Section

Section = List<Section | List<Line>>

Line = List<Role>

Role = List<TimeItem>

TimedItem = {
  position : Time
  duration : Time
  type : union {
    Command
    Ref
    Marker
    Symbol
    Note
    Duration
  }
}
```


## Notation

We assume that musical notation is already enclosed in some top level tag (eg <music></music>).   The following sections defines the notation syntax.  Our model is a sort of a stack based state machine.   The notation has a starting context around which modelling works.   All stack push commands begin with \<command> and stack pops are performed in two ways:

\-            =>  Pops the last command
\-<command>   =>  Pops all commands till the matching \<command>

eg:

```
\thalam{thalam data as json} notation continues here until \-
```

This way custom commands can be passed in as extensions for other layers.

At the end of the document the stack is implicitly popped fully.

### Comments

Comments are via the "#" Command, eg:

```
\# Begins a comment and can be \# nested until we have have a matching #\ end comment tag.  
These are multi-line comments.
These need to be balanced so we need another #\ here.
```

```
\## Are single line comments till the end of the line
```

### Thalams

Thalams are the core of rhythmic timing.  Our songs etc follow *some* thalam.   Thalams fall between "|" and "||" tokens.

Thalams are cyclical.   Piece of music keeps starting and ending cycles repeatedly.   There can be multiple parallel tracks for each section of music - eg a swaram track, a sahithyam track, a konakkol track (in more advanced ways multiple swarams for the same track).  Note that we are not describing sangathis here.  Everything here is about a "single" sangathi.  It is very tempting to annotate linkages between sangathis.  But from a purely modelling perspective, not sure if this is worth it.   At least this should not be a notation level concept.   Better to have different layers adding more and more over the previous layer (eg data layer, view layer, editing layer, playback layer etc).

Thalams are set via the \<thalam> command,eg (more on the duration command next):

```
\thalam{name = Adi, pattern = "|,,,|,,|,,||", duration = 4}
|,,,, ,, P D   N S. , N  ,, P M  |  P M G , , , G ,  |  M , P ,  , , P , || 
|M,,, ,, 
\thalam
```

### Durations

While thalams are set to indicate what a cycle looks like, it is pretty simplistic.  It assumes each beat in the thalam is a single note duration.  However there are times when we need to modify the number of notes per beat.  This can be done in 2 ways.  One via the duration flag in the thalam parameter itself.  *or* as a command in line with the notation.

Each by setting the duration in the thalam command to say 4, each beat has 4 note duration.  However to enable say a thisram (3 notes) in the 3rd beat for a particular sangathi we can use the duration command followed by an end tag, eg:

```
| .... notation continues \duration{3} tha ki ta \- ..... ||
```

TODO - Consider a short cut since this could be common enough via parantheses to show "verbatim" duration.
TODO 2 - Consider command that apply "vertically" instead of horizontally.  eg if every 3rd beat is a thisram can commands be made to apply verically so this duration change does not have to happen each time?

### Thaalams and Avarthams

## Notation

### Durations

### Swarams

### Syllables


