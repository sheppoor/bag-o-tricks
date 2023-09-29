# Shep's Bag o' Tricks

A collection of resources, snipits, and links that I find useful and want at my fingertips. Some might be useful to others so I'm making it public. I do take suggestions but I'm picky.

This is a start, I will continue to consolidate and update over time.

## Sub Pages

Overflow. Some topics just need their own page.

* [Javascript](javascript.md)
* [SQL](sql.md)
* [Development Environment](devenv.md)

## General

### Trends

* <https://cwe.mitre.org/top25/archive/2022/2022_cwe_top25.html> - useful in various discussions
* <https://insights.stackoverflow.com/survey/> - always important for trends, but overused and over sited as definitive
* <https://trends.google.com/> - oft forgotten critical resource for trend analysis
* <https://gs.statcounter.com> - needed to convince managers we don't need to support IE. Also has screen resolution stats over time.
FF=Gecko, Safari=Webkit, Chrome=Blink

## RegEx

* <https://cheatography.com/davechild/cheat-sheets/regular-expressions/> - Favorite quick cheatsheet
* <https://regex101.com/> - excellent testing site. Has a quick ref. Also, constructs an explanation in plain English. Also has a unit test construction, but I find VSCode plugins better. Also has a library, but I find it lacking.
* <https://www.regexlib.com> - better regex library, but still not great.

### Specific formulas

* CAS numbers. Note the check digit is incorrect in some official numbers so it can't always be considered reliable.

```regex
\d{1,7}-\d{2}-\d$
```

*More `TODO`, find better library resources, add some of my personal creations.*

## Markdown

By far my favorite quick reference - so clean, so accessible. Covers all the basics very well.  
<https://www.markdown-cheatsheet.com/>

Markdown flavors and some notes:

* [Github](https://docs.github.com/en/get-started/writing-on-github/working-with-advanced-formatting/creating-and-highlighting-code-blocks) - language-specific code highlight with triple backtick wrapper, with the open followed by the language name.
* [WikiText](https://en.wikipedia.org/wiki/Help:Wikitext) for Wikipedia
* [SharePoint](https://learn.microsoft.com/en-us/contribute/markdown-referencehttps://learn.microsoft.com/en-us/contribute/markdown-reference) which is most non-compliant. Strikeout requires `<s>` html. Don't try links to images and SharePoint resources, breaks too frequently, but external web is fine.
* [Pure markdown](https://daringfireball.net/projects/markdown/syntax) original spec

 *`TODO` There is no definitive source that clearly marks the difference between the major markups. Low priority.*

## CSS3

<https://en.wikipedia.org/wiki/Sass_(style_sheet_language)#Features> - Quick rundown of SCSS & Saas, and resulting CSS

<https://sass-lang.com/documentation/syntax> - SCSS docs

Minor Sass notes:

* A collection variable is just a space delimited list. Crappy design.
* Interesting way of conditional inclusion: `border-radius: if($rounded-corners, 5px, null);`
* Escaping embedded string quotes sucks. Look up if needed.
* Comments: `/* */` gets included in CSS, `//` is source SCSS only and isn't in final CSS.

## Other specific technologies

### Chrome

Open multiple instances of Chrome each with an independent profile. Underutilized feature in my opinion. Much more powerful than just adding an incognito window. Replace `homedir` and `profilename` as appropriate.

```bash
/usr/bin/google-chrome-stable --user-data-dir="/home/homedir/chrome/profilename" &
```

### Git

* <https://cheatography.com/samcollett/cheat-sheets/git/> - I don't love this cheatsheet, I think we can do better
* <https://git-scm.com/book/en/v2> - Great book to learn Git, and then serves as a reference. Older but all still applicable. CC-NC-SA

### Bootstrap

* <https://cheatography.com/masonjo/cheat-sheets/bootstrap/> - cheatsheet kinda sucks, would like to make my own. The world needs a 1-page ref sheet.

### CRON

* <https://quickref.me/cron> - shockingly complete

### Excel functions

I'm sick of Excel, but it's a reality in business. So many ETL processes still need to go through Excel because that's where the client starts.

[Convert Excel to JSON](https://codebeautify.org/excel-to-json) - nice tool, want to build my own some day with specific enhanced and integrated features

<!--
Date field into mySql date format
```Excel
=TEXT(A1,"'YYYY-MM-DD'")
```

Quote field or set to null

```Excel
=IF(ISBLANK(A1),"null","'"&A1&"'")
```
-->
VLookup but with exact match condition

```Excel
=IF(ISNA(VLOOKUP(E6,$A$2:$B$99,1,0)),"null",VLOOKUP(E6,$A$2:$B$99,2,0))
```

## Bash / Linux

Linux Mint Mate. It works, it's sufficient. My choice since 2008.

<https://cheatography.com/davechild/cheat-sheets/linux-command-line/> - cheatsheet ok, does a poor job with chmod which is what I was looking for in the first place.

### Screenshot Shortcut

I only ever want to screenshot an area, I hate that the default is whole screen. First, create `.screenshot-area`

```Bash
#!/bin/bash
sleep 1
/usr/bin/mate-screenshot --area
```

then in Keyboard Shortcuts, add a Custom called "Print Area", key is "Print" (which is the "PrtSc" key), pointed to /home/myhomedir/.screenshot-area

### Microphone Toggle

```Bash
#!/bin/bash
/usr/bin/amixer -c 2 sset Mic toggle
# Tail gets " [on]" or "[off]", then xargs is a dirty way to trim the leading space. Ugly but works.
MICSTAT=$(/usr/bin/amixer -c 2 sget Mic | tail -c 6 | xargs)
notify-send -t 1000 "Webcam Microphone" "Toggled "$MICSTAT
```

then in Keyboard Shortcuts, add a Custom called "Mic Toggle", key is "Alt-Pause", pointed to /home/myhomedir/.webcam-mic-toggle

#### Simple play/pause key binding

In the keyboard shortcuts, re-map Pause key to "Play (or play/pause)". So much better than multi-key combo for such a common and immediate need task.

## Fonts and Icons

* <https://fonts.google.com/> - definitive for FOSS fonts
* <https://icons.getbootstrap.com/> - almost as good as FontAwesome for many use cases
* <https://fontawesome.com/> - The best, only a few bucks, but sometimes FOSS is more important
* <https://fonts.google.com/icons> - for special cases

## Other

* Always Firefox with uBlock Origin - not for ad reduction, but for speed and safety.
* <https://choosealicense.com/licenses> Easy to understand open source license selection. Not in-depth, but a good discussion starting point when needed.
* <https://icoconvert.com/> - Create favicon.ico files from an image. Sufficient, not great.

## Hardware

* Monitors: 4960x1600 PLP layout, 1200x1600 Dell 2007FP x2 + Dell UP3017 or U3011  2560x1600
* Trackball: Logitech M570 or M575, not the newer one
* i7-5820K 3.30GHz × 12, GeForce GTX 970, EVO970 1TB, 24GB, Mint 22 / Mate
* <https://www.cpubenchmark.net/> - I can never remember the name when I need it

## 3D Printing

* OpenSCAD
* FreeCad
