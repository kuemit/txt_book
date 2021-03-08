# txt_book standard description v0.1

## What is txt_book?

txt_book is a standardized format for ebooks in plain text txt-files. It's goal is to enable human readers and software that implement the standard to read the books content itself plus meta information like the books title and author, bookmarks by different users or comments on text passages from one single txt file.

As said the goal is to provide all of this information to a human reader too. So txt_book tries not to clutter the text file with control characters. Also we txt_book tries to stay as natural as possible with space character usage.

## Elements

### Header

A txt_book book always has to start with a book header. Every line of the header starts with an identifier to describe the following information. TITLE, AUTHOR and TXT_BOOK_V are mendatory to be the first two lines. All following header lines are optional.

#### Header fields

| field name | description                                        | mendatory |
| ---------- | -------------------------------------------------- | --------- |
| TITLE      | The books title.                                   | yes       |
| AUTHOR     | The books author.                                  | yes       |
| TXT_BOOK_V | The version of txt_book that is used in this file. | yes       |
| PUB-DATE   | The books publication date in ISO 8601 format.     | no        |
| ISBN       | The books ISBN.                                    | no        |

#### Header example

```txt_book
TITLE: Alice's Adventures in Wonderland
AUTHOR: Lewis Carroll
PUB-DATE: 1865-11-26
```

### Bookmarks

Bookmark entries are located at the end of the file, after the books contents. There is one bookmark entry per line. A bookmark entry always starts with a "@" sign followed by a space character. Following this identifier, the actual content of the bookmark entry starts with a user name followed by a ":" sign to indicate the user who issued this bookmark. This enables collaboration on the book or to save the last reading position in a multiuser environment like a shared e-reader. After the colon an file-unique identifier provides the possibility to save and identify more than one bookmark per user. The identifier can contain letters, numbers and the "_" sign. The identifier is then followed by the date and time where the bookmark was issued. The date has to be in ISO 8601 format and is enclosed by brackets. A following "=" leads to the position in the books contents finally. The position is described by two numbers seperated by a colon. The first number indicates the line starting at the first line of the **file** not the books lines. The second number is the characters position in the line. A dot after the position indicator closes the mendatory part of the bookmark. Optionally a comment for the Bookmark can be issued after the dot in the same line and enclosed by double quotes.

```txt_book
@ timo: interesting (2021-03-08 8:00) = 124:10. "An interesting information."
```

If the reader software doesn't have multi user support or this feature is not needed, there is a default bookmark that indicates only the last reading position.

```txt_book
@ default: default (2021-03-08 8:00) = 124:10. "This is the default bookmark aka. last reading position."
```

### Headings

txt_book uses the wikimedia standard here. Heading lines start and end with a "=". Another "=" is added for each sublayer of headings. Books sometimes have subtitles for their Chapters. txt_book extends the wikimedia standard for this purpose. To indicate that a subtitle line is a subtitle of the same layer and not anoteher heading or a sublayer of its main heading, txt_book adds a pair of brackets around the subtitle.

#### Heading with subtitle example

```txt_book
= CHAPTER I = 
=( Down the Rabbit-Hole )=
```