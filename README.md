
# INSTALL

Copy the executable pykipandoc in /usr/lib/ikiwiki/plugins/ (as root) *OR* copy the executable pykipandoc _and_ proxy.py in ~/.ikiwiki/plugins/ and set liburl => '/home/<username>/.ikiwiki' in <youwiki>.setup.

In IkiWiki setup file add 'pykipandoc' to plugins array and disable 'mdwn' plugin.

## CAVEANT

'mdwn' extension is hardcoded in pykipandoc source, as you can see: if you want to use markdown IkiWiki's standard plugin, you have to either 

* assign a new extension (maybe 'pmdwn') to pandoc's markdown _and_ provide a new longname to the htmlize hook, or

* implement some sort of conditions and the like? ...:)

## SETUP

As you can see, all setup pandoc-iki does is hardcoded in subprocess.Popen call; this probably (hopefully) will change, but is not a must-have to me.

I haven't implemented a getsetup hooks: this probably (hopefully) will change, but is not a must-have to me.

# ISSUE/BUG

Big bugs and endless ignorance need Biggest advices: *as with pandoc-iki*, all works at compile-time, none at web-runtime.
I.e. click on 'preview' or 'save page' in editpage return nothing: none in preview, nor in the edited wiki page. At least in pandoc-iki.

In pykidoc the result is: 

   pandoc: : hGetContents: invalid argument (Invalid or incomplete multibyte or wide character)

_I think this is **related to UTF-8**, and to some IkiWiki code requesting to editpage in another encoding, as turn out looking at HTTP POST request._
Maybe [this](http://old.nabble.com/Bug-373203%3A-ikiwiki%3A-utf8-not-handled-in-image-titles-td4848774.html) refers to the same issue? It's (yet) a gettext problem?
Mdwn perl plugin provides a workarund for a similar problem:

     # Workaround for perl bug (#376329)
     $content=Encode::encode_utf8($content);
	eval {$content=&$markdown_sub($content)};
	if ($@) {
	   eval {$content=&$markdown_sub($content)};
	   print STDERR $@ if $@;
	}
	$content=Encode::decode_utf8($content);
	return $content;

Looking for something similar is the reason of that strange try: unicode etc... in pykipandoc.

Without this problem resolved, this extension is -- obviously -- useless. Please [help me](mailto:temmenel@gmail.com)!

Thankyou to all.