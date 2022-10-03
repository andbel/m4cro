# M4CRO - A general purpose text pre-processor in PHP with no dependencies

M4CRO was born out of a disappointment with the GNU M4 text processor. M4 seemed like a promising tool, but I couln't make it work the way I needed, and out of this frustration I wrote M4CRO.  (To be clear, M4CRO is NOT derived from M4; it's been developed from scratch.)

M4CRO works by taking the input text, scanning it, looking for directives and definitions, and processing them, one by one, recursively, generating the output text along the way. If the input file contains no directives recognized by M4CRO, the output would be the exact copy of the input. 

To give a trivial example, suppose the input file contains the following text:

```
This is a simple text of M4CRO preprocessor
define( 'GREETINGS', 'Hello World!' )
1. GREETINGS
2. GREETINGS
3. GREETINGS
This is a test, this is only a test.
```

After processing this text, M4CRO would generate the following output:

```
This is a simple text of M4CRO preprocessor
1. Hello World!
2. Hello World!
3. Hello World!
This is a test, this is only a test.
```

As you could have guessed, M4CRO scans the input text, and when it encounters the line that starts with **define** it creates a rule that has the effect of substituting the text **GREETINGS** with the text **Hello World!**. From that point on, whenever it encounters the text **GREETINGS**, it replaces it with **Hello World!**. The rest of the text gets transferred to the output as it is.

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

