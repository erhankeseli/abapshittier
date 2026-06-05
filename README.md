# ABAP Shittier

A Claude Code skill that does the opposite of the ABAP Pretty Printer. You give it
clean ABAP, it gives you back the ugliest version it can manage. The only rule is
that the result still compiles and behaves exactly the same.

It's the ABAP take on [Shittier](https://github.com/), the joke anti-Prettier.

## What it actually does

The Pretty Printer indents your code, fixes keyword casing, and aligns your
declarations. This undoes all of that. Casing goes random, indentation ignores
scope, alignment gets torn apart, lines break in stupid places, and the mandatory
spaces get padded out to look worse.

What it does not do is change behavior. If you run the tokens through it ignoring
whitespace and casing, you get the exact input back.

Some reasons you might want this:

* Showing a junior why the formatting standards are there
* A cursed commit for April 1st or a conference talk
* Throwing garbage at a linter or review process to see what it catches

It is not for obfuscation or hiding code. It's a joke.

## What it won't break

ABAP parses on tokens, so a few things have to stay intact or the code stops
activating. Shittier leaves these alone:

* `*` line comments stay in column 1
* required spaces around operators and parentheses get padded, never deleted
  (`lv_x = 1` becomes `lv_x   =  1`, which still compiles)
* string templates and literals are untouched
* statements, their closing periods, and `:` `,` chains stay as they are
* keywords and identifiers keep their actual letters, only the casing moves

## Install

```bash
# all projects
unzip abap-shittier.zip -d ~/.claude/skills/

# just this repo
unzip abap-shittier.zip -d .claude/skills/
```

The file has to land at `~/.claude/skills/abap-shittier/SKILL.md`, not one folder
deeper. Start a new Claude Code session and it picks it up.

## Using it

Ask for it in plain language:

> shittier this class
> un-format this method
> make this report unreadable

## Example

Before:

```abap
METHOD get_customer_balance.
  DATA: lv_total TYPE bapidoccur,
        lt_items TYPE STANDARD TABLE OF ty_item.

  SELECT * FROM ztable
    INTO TABLE @lt_items
    WHERE kunnr = @iv_kunnr.

  LOOP AT lt_items INTO DATA(ls_item).
    lv_total = lv_total + ls_item-amount.
  ENDLOOP.

  rv_balance = lv_total.
ENDMETHOD.
```

After:

```abap
   meThOd get_customer_balance.
DATA: lv_total TYPE bapidoccur,
        lt_items TYPE STANDARD TABLE oF ty_item.
      SeLeCt  *   fROm ztable    InTo TaBLe @lt_items   wHeRe kunnr = @iv_kunnr.
 LooP aT lt_items inTo DATA(ls_item). lv_total    =  lv_total +    ls_item-amount. ENDloop.
        rv_BALANCE
=
   lv_total.
ENDmeThod.
```

Same tokens, same result, unreadable.

## Layout

```
abap-shittier/
  SKILL.md    the skill itself
  README.md   this file
```

## License

MIT.
