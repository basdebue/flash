# flash

**`flash`** is a minimalistic flashcard application for the terminal written in portable, POSIX-compliant sh[*](#known-quirks). It doesn't have a lot of features other than being tiny, well-documented and extremely minimalistic.

If you're on the hunt for a full-featured software solution with answer verification and spaced repetition, you should probably look elsewhere, but people in search of a simple flashcard application for their terminal-based workflow will feel right at home!

## Usage

The simplest way of using `flash(1)` is to pipe it a text file:
```sh
$ flash < example.txt
```

Input is always taken from standard input and cannot be passed as an argument. Nothing prevents one from using the application interactively, but that way of doing things isn't very useful as the answer would be displayed right alongside the question.

Each line of the input represents a flashcard, with its two "sides" separated by a colon. Leading whitespace is removed and empty lines are ignored. This example shows 3 cards that should behave like you would expect them to:
```
  foo: bar
  baz: quux

hello: world
```

If needed, the delimiter can be configured using the `FLASH_DELIMITER` environment variable. You can also use the delimiter as a regular character on either side of the card by escaping it with a `\`.

To show the right-hand side of the cards first, pass the `-r` flag:
```sh
$ flash -r < example.txt
```

Any other features, such as randomization of the card order or a limit to the number of shown cards, are to be implemented yourself. An example using `shuf(1)`:
```sh
# Show 10 random cards from `example.txt`.
$ shuf -n 10 example.txt | flash
```

For any further info, refer to the manual page or the [source code](./flash) itself!

## Known quirks

`flash` makes use of exactly two non-POSIX features by calling `read -sn` when awaiting user input. If either of these options is not supported by your shell, `flash` will gracefully downgrade to a standard `read` invocation, in which case you will have to specifically enter a newline in order to advance to the next card.

## License

This project is licensed under the [ISC license](LICENSE). Any contribution intentionally submitted for inclusion by you shall also be licensed as such, without any additional terms or conditions.
