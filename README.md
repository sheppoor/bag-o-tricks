# Shep's Bag o' Tricks

A collection of resources, snipits, and links that I find useful and want at my fingertips. Some might be useful to others so I'm making it public. I do take suggestions but I'm picky.

This is a start, I will continue to consolidate and update over time.

## General

### Trends

* <https://cwe.mitre.org/top25/archive/2022/2022_cwe_top25.html> - useful in various discussions
* <https://insights.stackoverflow.com/survey/> - always important for trends, but overused and over sited as definitive
* <https://trends.google.com/> - oft forgotten critical resource for trend analysis
* <https://gs.statcounter.com> - needed to convince managers we don't need to support IE. Also has screen resolution stats over time.
FF=Gecko, Safari=Webkit, Chrome=Blink

## Dev Environment

### VSCode plugin essentials

This list doesn't include project stack from extension packs such as Angular and Java.

* Regex Previewer (from Christof Marti) - great for in-line quick testing, unit test creation
* Dependency Analytics (from RedHat) - specific to Java projects but not in the extension pack
* Paste JSON as Code (from quicktype)
* SonarLint (from SonarSource)
* Hex Editor (from Microsoft) - best out there though they could do better
* Material Icon Theme (from Philipp Kief) - still my fav - this is for the VSCode UI, not necessarily dev
* Thunder Client (from Ranga Vahineni) - Like postman but integrated
* vscode-pdf (from tomoki207) - basic in-line viewer
* markdownlint (from David Anson)
* Code Spell Checker (from Street Side Software) - personal choice, can get in the way but much better than the alternatives

Specialized / depending on project:

* Docker (Microsoft)
* SQLite Viewer (from Florian Klampfer) - ok / sufficient
* LaTeX Workshop (James Yu) - Haven't used yet / for a specific project

Missing:

* A better hex editor
* A column mode editor tool - old school editors (UltraEdit for example) were better and feature complete for dealing with fixed position files.
* I hate all the color pickers, need a good one that's not external to VSCode.

### VSCode customizations

All my standard changes to the `settings.json` file

*`TODO` look up what I've done before and add it here*

### External to VSCode

* <https://codebeautify.org/> - great tools: Encoding / decoding, validators, viewers. Lots now within VSCode but still an important resource. This one site supplants many older specific sites. Some features aren't best-in-class, but sufficient.

### ESLint customizations

Goes into the `eslint.rc` file

```javascript
module.exports = {
  'env': {
    'browser': false,
    'commonjs': true,
    'es2021': true,
  },
  'extends': [
    'google',
  ],
  'parserOptions': {
    'ecmaVersion': 13,
  },
  "ignorePatterns": ["**/development/*.js"],
  'rules': {
    'quotes': 'off',
    'object-curly-spacing': 'off',
    'require-jsdoc': 'off',
    'max-len': [
      'error',
      {
        'code': 100,
        'tabWidth': 2,
        'ignoreComments': true,
        'ignoreUrls': true,
        'ignoreStrings': true,
        'ignoreTemplateLiterals': true,
      },
    ],
    'no-multiple-empty-lines': [
      'error',
      {
        'max': 3,
      },
    ],
    'no-trailing-spaces': [
      'error',
      {
        'ignoreComments': true,
      },
    ],
  },
};
```

Notes: Single quote forced standard is overkill. curly brace spacing off for improved readability. 100 code width for lots of cases where code is still very readable (don't abuse). Three blank lines is fine for structure and readability in some cases. Trailing space standard gets in the way with comments.

## RegEx

* <https://cheatography.com/davechild/cheat-sheets/regular-expressions/> - Favorite quick cheatsheet
* <https://regex101.com/> - excellent testing site. Has a quick ref. Also, constructs an explanation in plain English. Also has a unit test construction, but I find VSCode plugins better. Also has a library, but I find it lacking.
* <https://www.regexlib.com> - better regex library, but still not great.

Specific formulas

* `\d{1,7}-\d{2}-\d$` - CAS numbers. Note the check digit is incorrect in some official numbers so it can't always be considered reliable.

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

## SQL

### mySql / MariaDB

Create DB

```SQL
CREATE DATABASE mydb CHARACTER SET utf8 COLLATE utf8_general_ci;
```

Overview, list all tables, cols

```SQL
SELECT VERSION(), DATABASE(), CONNECTION_ID(), USER(), CURRENT_USER();
SELECT TABLE_NAME FROM information_schema.TABLES WHERE TABLE_SCHEMA = 'mydb';
SELECT * FROM information_schema.`COLUMNS`;
SELECT * FROM mysql.user;
```

Typical Create

```SQL
CREATE TABLE my_table (
  id int IDENTITY(1,1) NOT NULL,
  foreign_table_id int DEFAULT NULL,
  seq int DEFAULT NULL,
  is_active int DEFAULT '1',
  created_user varchar(20) DEFAULT NULL,
  created_dt datetime DEFAULT NULL,
  lastedit_user varchar(20) DEFAULT NULL,
  lastedit_dt datetime DEFAULT NULL,
  PRIMARY KEY (id)
);
```

Typical Alter

```SQL
ALTER TABLE my_table ADD new_col_name VARCHAR(50) NULL;
```

How to drop all tables. Builds a script, doesn't actually auto-run. Much safer alternative.

```SQL
USE information_schema;
SELECT CONCAT('DROP TABLE ',table_name,';') fld FROM TABLES WHERE table_schema = '<this_db_name>';
```

Find skipped id numbers from a table "t" mytable

```SQL
SELECT * FROM (
SELECT @id1+1 first_empty_id, id-@id1-1 count_of_empty_ids, @id1 := id this_id
FROM mytable t, (SELECT @id1 := 1) dummy_table
ORDER BY id ) t1
WHERE t1.count_of_empty_ids > 0;
```

Date/Time

* Don't use `SYSDATE()`, use `NOW()`. Problem: `SYSDATE()` will execute upon each use, `NOW()` is single execute.
* `SELECT YEAR(SYSDATE())`
* `SELECT SYSDATE()` - this is the at-the-moment system date/time, can fire n times in a single query
* `SELECT DATETIME()` - Same as sysdate but only fires once for the query. Get the same date/time for multiple inserts or rows.
* `SELECT CURTIME()`
* `SELECT DATEDIFF(DATE(FLOOR(SYSDATE())),'2000-01-01');` - days since a date

Other cool stuff

* `SELECT LEAST(NULL,500);`
* `SELECT LAST_INSERT_ID();` - most recent inserted ID from an auto increment

-- # of rows that would have been returned if there was no "limit". Follow-up query.
SELECT user_id
FROM site_user
LIMIT 10;
SELECT FOUND_ROWS();

<https://cheatography.com/davechild/cheat-sheets/mysql/> - mostly for type definitions and fn names

*`TODO` adding indexes, other data types (blobs in particular), foreign constraints, full text search example or reference. Plus I have a snipit cache somewhere, add it*

### Microsoft SQL Server

* <https://cheatography.com/davechild/cheat-sheets/sql-server/> - terrible cheatsheet, but a start

*`TODO` scripted conversion from MySql code to add*

### Oracle

Converting Oracle into mySql: "Create Table" statements (specific need)

* Zap any database name e.g., database_name.table_name --> table_name
* replace "prompt" with "--"
* For primary key, often replace NUMBER with AUTO_INCREMENT
* `NUMBER` --> `INT`
  * Watch out for embedded "NUMBER" text in fields.
  * Some like NUMBER(1) need to be TINYINT(1)
* `VARCHAR2` --> `VARCHAR`
* `DATE DEFAULT SYSDATE` --> `TIMESTAMP DEFAULT CURRENT_TIMESTAMP`
* Add `PRIMARY_KEY(<field_name>)`
* append `ENGINE=INNODB DEFAULT CHARSET=utf8`

Converting Oracle insert statements to mySql (also very specific need)

* Watch out for reserved words. e.g., change " name, " to " `name`, "
* Zap the date inserts.
  * Search Mask, w/ unix regex: `to_date('\d{2}-\d{2}-\d{4} \d{2}:\d{2}:\d{2}', 'dd-mm-yyyy hh24:mi:ss')`
  * Set it to null or now().

Cool Oracle Stuff

* If you add a comment into your select statement of "/*csv*/" and run as a script (important!) then it will output CSV strings!
  * `select /*csv*/ field1, field2 from table;`
* To insert a single quote, just quote the quote. 
  * 'The school''s name is ''My High School'' and that''s it.'
* To insert a "&", it can be escaped with a "\". However I've has some trouble with this. Only a problem in sqlPlus or sqlDeveloper.
  * To turn off the feature of using "&" for variables, run `SET DEF OFF;` then run the insert or update query.

SELECT @row := @row + 1 AS ROW, t.*
FROM site_user t, (SELECT @row := 0) r

### SQLite

* <https://cheatography.com/richardjh/cheat-sheets/sqlite3/> - command line

## Javascript

Moved on to modern ECMAScript, leaving all the old workarounds behind (all removed).

* `JSON.parse()` takes a string of valid JSON. Throws error if invalid.
  * Optional replacer function param, can be used to manipulate the value on the way in.
  * `JSON.parse(jsonObj, (k, v) => {`
* `JSON.stringify()`
  * First param is the JSON object, second param is a replacer (optional)
  * replacer can be an array `['key2', 'key1']` which filters the JSON to just those keys
  * replacer can be a function `JSON.stringify(jsonObj, (k, v) => {`
    * return value is value, key remains the same
    * return `undefined` from the fn to not include a value

### Snipits

*`TODO` so many to add...*

<!--
Find all attributes for a js obs
`TODO` THIS IS MY VERY OLD ONE, replace. Also, lots to add.

```Javascript
myobj.each(obj.attributes, function(i, attr){
    try {
        var n = attr.name;
        var v = attr.value;
        console.log (n+": "+v);
    }
    catch(err) {}
    });
```
-->

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

## Other

* <https://fonts.google.com/> - definitive for FOSS fonts
* <https://icons.getbootstrap.com/> - almost as good as FontAwesome for many use cases
* <https://fonts.google.com/icons> - for special cases
* Always Firefox with uBlock Origin - not for ad reduction, but for speed and safety.
* <https://choosealicense.com/licenses> Easy to understand open source license selection. Not in-depth, but a good discussion starting point when needed.

## Hardware

* Monitors: 4960x1600 PLP layout, 1200x1600 Dell 2007FP x2 + Dell UP3017 or U3011  2560x1600
* Trackball: Logitech M570 or M575, not the newer one
* <https://www.cpubenchmark.net/> - I can never remember the name when I need it


## 3D Printing

* OpenSCAD
* FreeCad
