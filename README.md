# Moodle Advices
This page contails some important Moodle development knowledge, conteptions, rules and snippets for more productive development.

## Plugin development Notes
First at all, if you want to develop new plugin, you can easly download similar to your plugin and check it codes, maybe you can find neede solution and save your time.
### Important rules
There are few rules, which you must follow when develop some new Moodle plugin.
#### lib.php
This file will be executed each time when you run Moodle. So it's important to use it carefull:
- There must be declared functions like _extends_navigation, which communicate with Moodle (for ex. change main website navigation)
- Don't declare in lib.php your own functions and don't include other files (libs), write/include them on your locallib.php and other plugin's functions. 

## Moodle Database Notes
- Format your SQL usign Moodle [SQL coding style](https://docs.moodle.org/dev/SQL_coding_style)
- Optimize your query logic to exclude not needed tables from query (you can find examples of optimized queries on Intelliboard plugin - file externallib.php)
- There are [100 Moodle queries](https://docs.moodle.org/29/en/ad-hoc_contributed_reports#User_Course_Completion) but be carefull, because a lot of them are not optimized. 

## Other Moodle Tricks

### How to Format Dates on Moodle
You can use this simple code to convert UNIX timestamp to readable date:
```php
userdate(timecreated, '%a, %d %b %Y, %I:%M %p');
```
