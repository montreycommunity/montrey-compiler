# Features

## Case Sensitivity

`_H<>` is case-sensitive.  `_H<Hello>` and `_H<hello>` are different hashes; the same goes for `YHash(string)`.  You can disable case sensitivity by using `_I<>` instead (`_H` for * **H**ash*, `_I` for * **I**nsensitive*), and `.sensitive = false`:

```pawn
switch (YHash(string, .sensitive = false))
{
// This will give a compile-time error about duplicate cases.
case _I<hello>:
case _I<Hello>:
}
```

## Collisions

Hashes are semi-unique.  For two strings there is a very good chance that they will have different hashes, but it isn't guaranteed.  Sometimes you may write a switch and get a duplicate case error, even when all the strings are different.  BUT CHECK THEY ARE DIFFERENT FIRST.  I can't stress this enough - DOUBLE and TRIPLE check your code, in the vast majority of cases that error happens when you accidentally put the same string twice (don't for get case-insensitive hashes will match).  If you still get the error you can pick a different hashing algorithm.  There are three in the library: Bernstein (djb2), the default; fnv-1 (Fowler-Noll-Vo); and fnv-1a:

```pawn
switch (YHash(string, .type = hash_fnv1))
{
case _H@f<example>:
case _H@f<using>:
case _H@f<fnv1>:
}
```

```pawn
switch (YHash(string, .type = hash_fnv1a))
{
case _H@a<example>:
case _H@a<using>:
case _H@a<fnv1a>:
}
```

You can explicitly specify djb2 with `_Hb<>`.  You can also combine with case-insensitivity with `_Ib<>`, `_Ia<>`, and `_If<>`.

## Packed Strings

If your input string is packed, pass `.pack = true` to `YHash()`.

## Max Length

If you only want to hash, say, 5 characters from the input string, pass `.len = 5` to `YHash()`.

## Very Long Constants

`_H<>` is designed to be easy to use, but it has a slight limitation (especially on the old compiler).  The code generated by it becomes very long IN TEXT - but the compiler reduces this length down to one single number, so don't worry about "optimisation".  The code generated is a very complex sum, but the compiler performs sums at compile-time to only put the final answer in the AMX.  Having said that, because of the length of the sum this can sometimes become too long (espcially on the old compiler, where the line length limit was 512).  If this happens you can use the old version - `_H()` instead.  This is far more efficient in generated source-code, but more complex to use:

```pawn
const newVersion = _H<hello there>;
const oldVersion = _H(h,e,l,l,o, ,t,h,e,r,e);
```

The obvious problem being the need to use `,` between every character.

