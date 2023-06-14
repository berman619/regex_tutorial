# Understanding Regular Expressions: Matching a US Phone Number

Hey there! Ever wonder how your favorite apps validate something like a US phone number? If you've tried to type in something that's not a number, they instantly know it's not valid. Magic? No, it's all thanks to Regular Expressions, or 'regex' for short. In this guide, we'll take a closer look at the regex used for matching US phone numbers. Trust me, by the end of this, you'll look at that strange string of characters and it'll all make sense!

## Summary

So, let's talk about the star of today's show: `/^\(?\d{3}\)?[-.\s]?\d{3}[-.\s]?\d{4}$/`. Don't be scared off by all those slashes, brackets, and letters - this is just a regex pattern that matches US phone numbers in various formats. It's like a master key that can unlock whether a string of text looks like a phone number. Cool, right? Stick with me and I'll break down each piece of this regex, showing you how each part contributes to the overall magic!

## Table of Contents

- [Anchors](#anchors)
- [Quantifiers](#quantifiers)
- [Grouping and Capturing](#grouping-and-capturing)
- [Bracket Expressions](#bracket-expressions)

## Regex Components

### Anchors

Anchors in regular expressions, like the caret (`^`) and the dollar sign (`$`), do not match any actual characters themselves. Instead, they match a position before, after, or between characters.

The caret (`^`) at the beginning of our regex signals that the pattern following it should appear at the start of the string input. In our phone number regex, it expects to first see `\(?\d{3}\)?`, which could be either three digits or three digits wrapped in parentheses (like "(123)" or "123").

The dollar sign (`$`) has a complementary role. It asserts that the string input should end with `\d{4}`, or four digits, optionally preceded by a separator. 

Let's bring this to life with an example. Take the string "123-456-7890". This string begins with three digits and ends with four digits, which aligns perfectly with the expectations set by our regex anchors. So it's a match!

But what about the string "abc 123-456-7890 xyz"? Despite containing "123-456-7890" in the middle, it doesn't conform to the regex requirements because it starts with "abc" and ends with "xyz", not three and four digits respectively. The anchors require thart the regex matches the string from beginning to end, and not just any portion of it, so this string would not pass.


### Quantifiers

A quantifier in a regex determines how many instances of an element there should be for a match to be found. In our regex (`/^\(?\d{3}\)?[-.\s]?\d{3}[-.\s]?\d{4}$/`), the quantifiers we're using are {3} and {4}.  The `\d` in our regex is a character class that matches any digit.

The `{3}` after `\d` means that we're looking for exactly three digits in a row. So, `\d{3}` is looking for a sequence of exactly three digits, like "123". We're using this for the first three digits, which is the area code, and the second block of numbers (the fourth through sixth digits). 

Similarly, the `{4}` after `\d` means we're looking for exactly four digits in a row. So `\d{4}` is looking for a sequence of exactly four digits, like "7890".

Let's consider some examples:

The string "123" would match `\d{3}` because it contains exactly three digits.

The string "7890" would match `\d{4}` because it contains exactly four digits.

However, the string "12" would not match `\d{3}` because it only contains two digits, not three.

Similarly, the string "789" would not match `\d{4}` because it contains only three digits, not four.

Using these quantifiers, we can ensure that a US phone number has the correct amount of digits in the correct places.

### Grouping

Next on our list is grouping, which is handled in regex using parentheses `( )`. Grouping allows us to treat multiple characters as a single unit, or group. If we want to apply a quantifier to more than one character, we can group them together. 

In our phone number regex (`/^\(?\d{3}\)?[-.\s]?\d{3}[-.\s]?\d{4}$/`), we use parentheses to group certain elements, or characters and their associated conditions, as a single unit. This group - `\(?\d{3}\)?` - consists of an optional opening parenthesis, exactly three digits, and an optional closing parenthesis. By placing these elements inside parentheses, we're telling the regex to treat this combination as a single element.

Let's break down `\(?\d{3}\)?`:

- `\(` and `\)` are escaped parentheses that match actual parenthesis characters in our input string, which people sometimes use around an area code in a phone number
- `\d{3}` matches exactly three digits

- The `?` after `\(` and the second `?` after `\)` make the parentheses optional.

- As a group, `\(?\d{3}\)?` is looking for exactly three digits that are optionally enclosed in parentheses. This could match "123" or "(123)".

Here's how this works in practice:

- The string "123" matches because it contains exactly three digits.

- The string "(123)" also matches because it contains exactly three digits enclosed in parentheses, and our regex allows for that.

- The string "12)" or "(12" would not match because they don't have three digits and the parentheses aren't correctly placed.

So by grouping, our regex can accommodate US phone numbers that have their area code with or without parentheses!

### Bracket Expressions

Bracket expressions in regex, denoted by square brackets `[]`, allow us to define a character class, which is a set of characters from which we want to match just one instance. Anything you put into a bracket expression will be matched as a single character.

In our phone number regex (`/^\(?\d{3}\)?[-.\s]?\d{3}[-.\s]?\d{4}$/`), we don't use a traditional bracket expression, but we use a similar concept with `[-.\s]?`. This represents a character class that matches either a dash "-", a period ".", a whitespace character "\s", or no character at all due to the `?` quantifier making the entire class optional.

In the context of matching phone numbers... the area code, central office code, and line number are usually separated by a character. This character can be a dash, a period, or a space. Alternatively, there might not be a separator at all. Our character class `[-.\s]?` allows for any of these possibilities.

Let's look at a few examples:

The string "123-456-7890" matches our regex because the "-" is included in our character class.

The string "123.456.7890" also matches because the "." is included in our character class.

The string "123 456 7890" matches because the whitespace character "\s" is included in our character class.

Finally, the string "1234567890" also matches because the character class is made optional by the `?` quantifier, which means no character at all is a valid match.

This way, our regex can flexibly match a variety of phone number formatting styles by using a character class.

## Author

My name is Zach Berger and I am a student at the Columbia Coding Boot Camp. You can find be at Github at [berman619](www.github.com/berman619).
