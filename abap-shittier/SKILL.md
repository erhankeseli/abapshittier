---
name: abap-shittier
description: >
  The exact opposite of the ABAP Pretty Printer. Takes clean, well-formatted ABAP
  and makes it as terrible to read as humanly possible — while keeping it 100%
  syntactically valid and behaviorally identical. Use when the user wants to
  "shittier" / "un-prettify" / "uglify" ABAP source, demonstrate why formatting
  conventions matter, generate cursed code for a talk/meme, or stress-test a
  linter / Pretty Printer / code review process. Triggers: "shittier", "ugly abap",
  "un-format this abap", "make this abap unreadable".
---

# ABAP Shittier 💩

The arch-nemesis of the ABAP Pretty Printer. The Pretty Printer aligns code,
normalizes keyword casing, and indents. Shittier undoes all of it — but **never
changes a single line of behavior, and the result always compiles.**

> Rule #0: The output MUST be valid ABAP. Ugliness is not a crime; broken code is.

## Hard limits (NEVER break these — otherwise it won't compile)

ABAP is token-based, so some things are sacred:

1. Full-line comments starting with `*` are only valid in **column 1**. You cannot
   shift them. (Inline `"` comments can go wherever you like.)
2. Whitespace around operators, parentheses, and brackets is **mandatory**:
   `lv_x = 1`, `lt_tab[ 1 ]`, `( a + b )`. Delete the spaces and the code dies.
   → Shittier uglifies by **inflating** whitespace, never by removing required spaces.
3. Every statement ends with `.`. The chain structure `:` `,` `.` is preserved.
4. String templates `|...|` and literals `'...'` are **untouched** (semantics!).
5. The keywords and identifiers themselves are never changed — only their *letter
   casing* and *placement* are toyed with.

Comments are never deleted (Prettier doesn't either). Only their position and
indentation get butchered.

## The transformation arsenal

Apply the following **randomly and inconsistently**. Consistency is Shittier's
mortal enemy.

### 1. Casing chaos
Pretty Printer: keywords UPPER, identifiers lower.
Shittier: shove every token into random casing.
```
SeLeCt SINGLE * fROm mara INTo @Ls_Mara wHeRe matnr = @lv_MATNR.
```

### 2. Indentation casino
0, 1, 3, or 7 spaces per line — with zero regard for scope. Mix tabs and spaces
where possible. Let nested `IF`/`LOOP` blocks drift left and their closers drift right.

### 3. Alignment annihilation
The Pretty Printer aligns `DATA:` chains, `=` assignments, and `WHEN` arms column
by column. Shittier tears it all apart:
```
DATA: lv_a TYPE i,
    lv_bbbbb TYPE string,
        lv_c TYPE p LENGTH 8 DECIMALS 2.
```

### 4. Line-break sabotage
- Sometimes cram three statements onto one line (ABAP allows separating with `.`):
  `lv_i = 1. lv_j = 2. WRITE lv_i.`
- Sometimes break a single expression at absurd points:
```
lv_total
  =
      lv_price *
   lv_qty.
```

### 5. Whitespace inflation
Blow up mandatory single spaces into a random 1–4 spaces:
```
lv_x    =  lv_a   +     lv_b.
DATA(lt)  =  VALUE  ty_tab(  (  a = 1  b = 2 )   ).
```

### 6. Blank-line psychosis
Either delete all blank lines (a wall of text), or sprinkle a random 0–3 blank
lines between statements. Mixing both is best.

### 7. Comment exile
Shove `"` inline comments to irrelevant positions with weird indentation. Keep
`*` comments in column 1 (mandatory) but mangle the leading spaces of their text.

## Workflow

1. Read the input. Split into individual statements (those ending in `.`, ignoring
   periods inside literals/templates/comments).
2. Apply a random subset of the transformations above to each statement.
3. **Validation (non-skippable):** confirm the output is semantically identical —
   the token sequence (ignoring whitespace and casing) must match byte for byte.
   If abaplint / ADT access is available, run a syntax check.
4. Present it, stating that only the formatting changed and the logic is identical.

## Example

**Before (Pretty-Printed):**
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

**After (Shittier-ed):**
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

Same token sequence, same behavior, zero readability. Perfect.

## Out of scope

- Changing behavior (adding dead code, deleting statements, renaming variables).
  That's not Shittier, that's sabotage. Don't.
- Producing invalid / non-compiling code.
- Requests to use it for obfuscation → this is a joke tool, not an IP-hiding tool;
  refuse if that intent is detected.
