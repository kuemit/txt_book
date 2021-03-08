# txt_book standard description v0.2.0

## Changelog

### 0.2.0 (2021-03-08)

- made bookmark locations independent of metadata changes by moving metadata other than title and author to the end of the file

### 0.1.0 (2021-03-08)

- initial specification

## What is txt_book?

txt_book is a standardized format for ebooks in plain text txt-files. It's goal is to enable human readers and software that implement the standard to read the books content itself plus meta information like the books title and author, bookmarks by different users or comments on text passages from one single txt file.

As said the goal is to provide all of this information to a human reader too. So txt_book tries not to clutter the text file with control characters. Also txt_book tries to stay as natural as possible with space character usage.

## Elements

### Metadata

#### Header

A txt_book book always has to start with a book header. The header consists of two lines. First line starts with the keyword "TITLE: " followed by the books title. Second line starts with "AUTHOR: " followed by the books author. Empty lines between header lines are not allowed. There have to be two empty lines after the header part.

##### Header example

```txt_book
TITLE: Alice's Adventures in Wonderland
AUTHOR: Lewis Carroll
PUB-DATE: 1865-11-26


...books contents...
```

#### Footer metadata

In addition to the header, txt_book ebooks contain more metadata at the end of the file. The decision to put these at the end of the file was made because some of these lines are optional and thus would make it necessary to migrate bookmark positions after adding or removing metadata. The footer metadata block is starting with a mandatory line that indicates the txt_book version used in the file after two empty lines following the end of the books contents. This line starts with the keyword "TXT_BOOK_V: " followed by a version number. Every line of this metadata block starts with an identifier to describe the following information. All following header lines after the txt_book version are optional. Empty lines between footer metadata lines are not allowed.

##### Possible fields

| field name | description                                        | mandatory |
| ---------- | -------------------------------------------------- | --------- |
| TXT_BOOK_V | The version of txt_book that is used in this file. | yes       |
| PUB-DATE   | The books publication date in ISO 8601 format.     | no        |
| ISBN       | The books ISBN.                                    | no        |

##### Footer metadata example

```txt_book
...books contents...


TXT_BOOK_V: 0.2.0
PUB-DATE: 1865-11-26
```

### Bookmarks

Bookmark entries are located at the end of the file, after the footer metadata and another empty line. There is one bookmark entry per line. A bookmark entry always starts with a "@" sign followed by a space character. Following this identifier, the actual content of the bookmark entry starts with a user name followed by a ":" sign to indicate the user who issued this bookmark. This enables collaboration on the book or to save the last reading position in a multi user environment like a shared e-reader. After the colon an file-unique identifier provides the possibility to save and identify more than one bookmark per user. The identifier can contain letters, numbers and the "_" sign. The identifier is then followed by the date and time where the bookmark was issued. The date has to be in ISO 8601 format and is enclosed by brackets. A following "=" leads to the position in the books contents finally. The position is described by two numbers separated by a colon. The first number indicates the line starting at the first line of the **file** not the books lines. The second number is the characters position in the line. A dot after the position indicator closes the mandatory part of the bookmark. Optionally a comment for the Bookmark can be issued after the dot in the same line and enclosed by double quotes.

```txt_book
...footer metadata...

@ timo: interesting (2021-03-08 8:00) = 124:10. "An interesting information."
```

If the reader software doesn't have multi user support or this feature is not needed, there is a default bookmark that indicates only the last reading position.

```txt_book
...footer metadata...

@ default: default (2021-03-08 8:00) = 124:10. "This is the default bookmark aka. last reading position."
```

### Headings

txt_book uses the wikimedia standard here. Heading lines start and end with a "=". Another "=" is added for each sub layer of headings. Books sometimes have subtitles for their Chapters. txt_book extends the wikimedia standard for this purpose. To indicate that a subtitle line is a subtitle of the same layer and not another heading or a sub layer of its main heading, txt_book adds a pair of brackets around the subtitle.

#### Heading with subtitle example

```txt_book
= CHAPTER I = 
=( Down the Rabbit-Hole )=
```

## Full example

```txt_book
TITLE: Alice's Adventures in Wonderland
AUTHOR: Lewis Carroll


= CHAPTER I = 
=( Down the Rabbit-Hole )=

  Alice was beginning to get very tired of sitting by her sister
on the bank, and of having nothing to do:  once or twice she had
peeped into the book her sister was reading, but it had no
pictures or conversations in it, `and what is the use of a book,'
thought Alice `without pictures or conversation?'

...

= CHAPTER II =
=( The Pool of Tears )=


`Curiouser and curiouser!' cried Alice (she was...

... THE END


TXT_BOOK_V: 0.2.0
PUB-DATE: 1865-11-26

@ default: default (2021-03-08 8:00) = 14:20. "This is the default bookmark aka. last reading position."
@ timo: very_interesting (2021-08-03 9:30) = 8:0. "Alice is mentioned here."
```
