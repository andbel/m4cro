# M4CRO - A general purpose text pre-processor in PHP with no dependencies

M4CRO was born out of a disappointment with the GNU M4 text processor. M4 seemed like a promising tool, but I couln't make it work the way I needed, and out of this frustration I wrote M4CRO.  (To be clear, M4CRO is NOT derived from M4; it's been developed from scratch.)

M4CRO works by taking the input text, scanning it, looking for directives and definitions, and processing them, one by one, recursively, generating the output text along the way. If the input file contains no directives recognized by M4CRO, the output would be the exact copy of the input. 

To give a trivial example, suppose the input file contains the following text:

```
GREETINGS! This is a simple text of M4CRO preprocessor
define( 'GREETINGS', 'Hello World!' )
1. GREETINGS
2. Greetings
3. GREETINGS
This is a test, this is only a test.
```

After processing this text, M4CRO would generate the following output:

```
GREETINGS! This is a simple text of M4CRO preprocessor
1. Hello World!
2. Greetings
3. Hello World!
This is a test, this is only a test.
```

As you could have guessed, M4CRO scans the input text, transferring the characters into the output text, one by one. When it encounters the line that contains the **define** directive, it creates a rule that has the effect of substituting the text **GREETINGS** with the text **Hello World!**. From that point on, whenever it encounters the text **GREETINGS**, it replaces it with **Hello World!**. The rest of the text gets transferred to the output as it is.

M4CRO is a one-pass processor: when it encounters a directive like **define** in the example above, it applies the rule to the remaining text, but that rule has no effect on the text that has already been processed. 

M4CRO is case-sensitive: in the example above, the rule has been created to substitute the text **GREETINGS** only, and that rule has no effect on other versions of the same word, such as **Greetings**, which remain unchanges.

M4CRO is a recursive processor: when it performs a text substitution according to one of the rules, before appending it to the output text, it scans the resultant text again for other rules that could have appeared as the result of the substitution. It does the repeat scanning until no more substitutions have been performed, and the resultant text becomes a stable part of the output. To prevent infinite loops, M4CRO has some reasonable limit for the recursion levels, and generates an error if this limit is reached.

M4CRO can be used in your PHP files as follows:

```
<?php

include_once( 'm4cro-engine.php' );

$m4cro_engine = new M4CRO();

$text_out = $m4cro_engine->ProcessText( $text_in );

?>
```

It can also be used on the command line, by executing the m4cro.php script with the PHP interpreter:

```
$ php ./m4cro.php path_in path_out

```


