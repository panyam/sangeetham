# sangeetham

Representing carnatic music to support both notations, views, swarams, sahithyams, kanakku etc.

## Scope

### In Scope 

* Representing all aspects of (Indian) music notation
* Representing multiple parallel tracks.
* Representing swarams, konnakkol, sahithyam (including breakages) all in a similar rhythmic way.

### Out of Scope

* Formatting of layouts.  The goal is that the raw format when viewed as is should be a good enough view instead of needing any more special processing.  Anything more is an extension (say coloring etc).
* Non music document components.  We expect these "snippets" are embedded in another document format (such as html) via some special root tag so that if a user needs to place blocks of texts, pictures *outside* the musical notation they can use what ever the container allows (css etc).
* The *editing* model that describes how a song might be edited in some visual editor.

## Notation

We assume that musical notation is already enclosed in some top level tag (eg <music></music>).   The following sections defines the notation syntax.  Our model is a sort of a stack based state machine.   The notation has a starting context around which modelling works.   All stack push commands begin with \<command> and stack pops are performed in two ways:

\-            =>  Pops the last command
\-<command>   =>  Pops all commands till the matching \<command>

eg:

\thalam{thalam data} notation continues \-

This way custom commands can be passed in as extensions for other layers.

At the end of the document the stack is implicitly popped fully.

### Comments

Comments are via the "#" Command, eg:

\# Begins a comment and can be \# nested until we have have a matching #\ end comment tag.  
These are multi-line comments.
These need to be balanced so we need another #\ here.

\## Are single line comments till the end of the line

### Thalams

Thalams are the core of rhythmic timing.  Our songs etc follow *some* thalam.   Thalams fall between "|" and "||" tokens.

Thalams are cyclical.   Piece of music keeps starting and ending cycles repeatedly.   There can be multiple parallel tracks for each section of music - eg a swaram track, a sahithyam track, a konakkol track (in more advanced ways multiple swarams for the same track).  Note that we are not describing sangathis here.  Everything here is about a "single" sangathi.  It is very tempting to annotate linkages between sangathis.  But from a purely modelling perspective, not sure if this is worth it.   At least this should not be a notation level concept.   Better to have different layers adding more and more over the previous layer (eg data layer, view layer, editing layer, playback layer etc).

Thalams are set via the \<thalam> command,eg (more on the duration command next):

\thalam{name = Adi, pattern = "|,,,|,,|,,||"}
\duration{4} \# More on this later #\
|,,,, ,, P D   N S. , N  ,, P M  |  P M G , , , G ,  |  M , P ,  , , P , || 
|M,,, ,, 
\thalam

### Durations




### Thaalams and Avarthams

## Notation

### Durations

### Swarams

### Syllables
