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
#### Global variables
Dont declare global variables on the beginning of included file, its not necessary because they are still visible on that part of file. Make them global on the functions and class methods because without that you will have no access for example to $DB or $USER.
```php
// wrong
global $USER, $DB;

function getUserInfo(){
  // good
  global $DB, $USER;
  $DB->get_record('user', array('id'=>$USER->id) );
}
```
## Moodle Webservices

### Configuration tips

If you enabled webservices and enable protocols for your Moodle website via adminpanel, you still need to add API methods to you webservice, else they will not be accessible for your webservice clients.

## Moodle Events
In the latest Moodle versions you should use [Event2](https://docs.moodle.org/dev/Event_2) to define your events.

## Moodle Database Notes
- Format your SQL usign Moodle [SQL coding style](https://docs.moodle.org/dev/SQL_coding_style)
- Optimize your query logic to exclude not needed tables from query (you can find examples of optimized queries on Intelliboard plugin - file externallib.php)
- There are [100 Moodle queries](https://docs.moodle.org/29/en/ad-hoc_contributed_reports#User_Course_Completion) but be carefull, because a lot of them are not optimized. 

## Work with Files

### Get User Image
```php
public static function getUserImage($user){
        global $DB, $CFG;
        if( $user->picture > 0 ){
            $user_priofile_pic = $DB->get_record("files", array("id" => $user->picture ));
            if($user_priofile_pic){
                $imagepath = $user_priofile_pic->contextid."/user/".$user_priofile_pic->filearea."/".$user_priofile_pic->filename;
                $user->image = $CFG->wwwroot."/pluginfile.php/".$imagepath;
            } else {
                $user->image = $CFG->wwwroot."/user/pix.php/0/f1.jpg";    
            }
        } else {
            $user->image = $CFG->wwwroot."/user/pix.php/0/f1.jpg";
        }
        return $user;
}
```
Note\* if you want to display image of user outside Moodle website, you need to enable flag on adminpanel (to be extended)

### Check if file was really uploaded into filemanager
```php
$usercontext = context_user::instance($USER->id);            
$draftfiles = $fs->get_area_files($usercontext->id, 'user', 'draft', $data['thumbnail'], 'id');
if (count($draftfiles)){ 
  
}
```

## Other Moodle Tricks

### How to Format Dates on Moodle
You can use this simple code to convert UNIX timestamp to readable date:
```php
userdate(timecreated, '%a, %d %b %Y, %I:%M %p');
```
