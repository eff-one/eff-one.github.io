# Changes in Word Filter Feature:
Ref: **MyBB release version 1.8.18**

## Changes:
With this release MyBB has redefined the system how word filters were working til now.

## Issues:

- Word as REGEX set to "YES": [Invalid REGEX strings saved by Admins can cause PHP error / warning at front-end](https://community.mybb.com/thread-218804.html).
- Word as REGEX set to "NO": [Inefficient usage of symbol(s) in dynamic word filters.](https://community.mybb.com/thread-218066...pid1306324)

## Resolution:
_Patch:_
https://github.com/mybb/mybb/pull/3353

## Impact:
- **Word as regex set to 'YES':**
    - _Effect:_ **N/A**. Onwards, words defined as REGEX by an admin will be checked for validity of the expression before it gets saved to confirm no warning or error at front end with invalid expression defined.
    - _Corrective Action:_ **N/A**
  
- **Word as regex set to 'NO':**

    - _Effect:_ The dynamic word filters (using symbols to catch unknown characters) already set by admins will not work as intended due to the logic change in symbol `(*)` usage. From now onwards the symbols `'*'` (any number of any character) and `'+'` (one number of any character) will be used efficiently. For example: **`*on*`** will catch `'congo'`, `'ontology'` or `'moron'`. However, **`on+`** will catch `'one'` it will not catch `'ton'` or `'onion'`. **`my++`** will catch `'mybb'` and `'myth'`, but will not catch `mya`,`'mummy'` or `'mystery'`.
    - _Corrective Action:_ Admins are required to edit all the already existing dynamic word filters and define the word as per the new logic to achieve intended behavior.

## Files Changed:
- `inc/class_parser.php`
- `admin/modules/config/badwords.php`
- `inc/languages/english/admin/config_badwords.lang.php`
