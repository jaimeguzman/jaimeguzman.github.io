Guide to programmatic node creation in Drupal 7 {.blog-post-title}
-----------------------------------------------

This amazing tutorial, I found it in this
blog** [http://fooninja.net/2011/04/13/guide-to-programmatic-node-creation-in-dr...](http://fooninja.net/2011/04/13/guide-to-programmatic-node-creation-in-drupal-7/)**

-

*See also [Updating nodes programmatically in Drupal
7](http://fooninja.net/2011/06/06/updating-nodes-programmatically-in-drupal-7/).*

*Most of this was written in 2011 and while everything is still
perfectly usable, there’s more ground that I want to cover, such as
working with i18n, EntityFieldQuery, media, etc. I will hopefully have
the time to write a bigger and better guide later in 2013. If you have
any requests for things I should write about, let me know in the
comments!*

I had to import a few thousand items from a legacy database to a Drupal
7 site and found that it was quite easy to do so programmatically. Here
I’ll first show you the basic code for adding nodes and then I’ll talk
about different field types, including how to add images and term
references (taxonomy). If you have any questions, just ask in the
comments and I’ll be happy to help!

Basic node creation - example
-----------------------------

Put the code below in e.g. foo\_update.php. Make sure it’s not
accessible through the web (unless you don’t have shell access and must
execute it through the browser and know what you are doing). In other
words, put it outside your Drupal directory, and then just run php
foo\_update.php. Note that *you* have to make sure that the input is
valid! Test and test again, always make backups and get used to
examining nodes with e.g. drush php-eval
'print\_r(node\_load(\$nid))' (see below for more about that).

If you use drush, you can skip the bootstrap part and, from your Drupal
root directory, run drush scr
../foo\_update.php (assuming foo\_update.php is one directory above).

+--------------------------------------+--------------------------------------+
|     1                                |     <?php                            |
|     2                                |     # Bootstrap start                |
|     3                                |     define('DRUPAL_ROOT', '/path/to/ |
|     4                                | drupal/root/directory');             |
|     5                                |     $_SERVER['REMOTE_ADDR'] = "local |
|     6                                | host"; // Necessary if running from  |
|     7                                | command line                         |
|     8                                |     require_once DRUPAL_ROOT . '/inc |
|     9                                | ludes/bootstrap.inc';                |
|     10                               |     drupal_bootstrap(DRUPAL_BOOTSTRA |
|     11                               | P_FULL);                             |
|     12                               |     # Bootstrap end                  |
|     13                               |                                      |
|     14                               |     $bodytext = "Foo m bar fnord?";  |
|     15                               |                                      |
|     16                               |     $node = new stdClass(); // Creat |
|     17                               | e a new node object                  |
|     18                               |     $node->type = "article"; // Or p |
|     19                               | age, or whatever content type you li |
|     20                               | ke                                   |
|     21                               |     node_object_prepare($node); // S |
|     22                               | et some default values               |
|     23                               |     // If you update an existing nod |
|     24                               | e instead of creating a new one,     |
|     25                               |     // comment out the three lines a |
|     26                               | bove and uncomment the following:    |
|     27                               |     // $node = node_load($nid); // . |
|     28                               | ..where $nid is the node id          |
|     29                               |                                      |
|     30                               |     $node->title    = "A new node se |
|     31                               | es the light of day";                |
|     32                               |     $node->language = LANGUAGE_NONE; |
|     33                               |  // Or e.g. 'en' if locale is enable |
|     34                               | d                                    |
|     35                               |                                      |
|                                      |     $node->uid = 1; // UID of the au |
|                                      | thor of the node; or use $node->name |
|                                      |                                      |
|                                      |     $node->body[$node->language][0][ |
|                                      | 'value']   = $bodytext;              |
|                                      |     $node->body[$node->language][0][ |
|                                      | 'summary'] = text_summary($bodytext) |
|                                      | ;                                    |
|                                      |     $node->body[$node->language][0][ |
|                                      | 'format']  = 'filtered_html';        |
|                                      |                                      |
|                                      |     // I prefer using pathauto, whic |
|                                      | h would override the below path      |
|                                      |     $path = 'node_created_on' . date |
|                                      | ('YmdHis');                          |
|                                      |     $node->path = array('alias' => $ |
|                                      | path);                               |
|                                      |                                      |
|                                      |     if($node = node_submit($node)) { |
|                                      |  // Prepare node for saving          |
|                                      |         node_save($node);            |
|                                      |         echo "Node with nid " . $nod |
|                                      | e->nid . " saved!\n";                |
|                                      |     }                                |
|                                      |     ?>                               |
+--------------------------------------+--------------------------------------+

\$node-\>language is important! If you don’t have the locale module
enabled, the node will not be assigned any particular language. Or
rather, the language code used then is LANGUAGE\_NONE, which is a
constant with the value und (undefined) in Drupal. **If you have locale
enabled** nodes can exist in more than one language, and you should
specify the language code. Go to Configuration -\> Regional and language
-\> Languages to configure languages and see what code you should use.
For English, it would be en; for Swedish, it would be sv; etc.

Other things you might want to set:

-   \$node-\>status (1 or 0): published or not
-   \$node-\>promote (1 or 0): promoted to front page
-   \$node-\>sticky (1 or 0): sticky at top of lists
-   \$node-\>comment: 2 = comments open, 1 = comments closed, 0 =
    comments hidden

This all you need for basic node creation. I will continue with
different field types. The code below should be placed before
the node\_submit line. All fields can be accessed
with \$node-\>field\_fieldname, where fieldname is the name you find in
Structure -\> Content types -\> Manage fields.

**The best way** to see how Drupal nodes are made up, and how to work
with the various fields, is to simply look at the structure of an
existing node. Just create a new PHP file with the first bunch of lines
above (including drupal\_bootstrap) and add print\_r(node\_load(\$nid)),
where \$nid is the node id. Or
use [drush](http://drupal.org/project/drush): drush php-eval
'print\_r(node\_load(\$nid))'.

Setting various field types
---------------------------

### Text or integer field

Nothing special:

+--------------------------------------+--------------------------------------+
|     1                                |     $node->field_fnordtext[$node->la |
|                                      | nguage][0]['value'] = "Fnord fnord f |
|                                      | nord";                               |
+--------------------------------------+--------------------------------------+

Multiple values (make sure the field is configured to allow this):

+--------------------------------------+--------------------------------------+
|     1                                |     $node->field_author[$node->langu |
|     2                                | age][]['value'] = "Ford, Tom";       |
|                                      |     $node->field_author[$node->langu |
|                                      | age][]['value'] = "Fnord, Dom";      |
+--------------------------------------+--------------------------------------+

### Creation time

To set a node’s creation time, you’d think you could just
set \$node-\>created. And you can - if you don’t use node\_submit() (or
if you set it after node\_submit()). Looking at
the node\_submit() function in node.module, we find this line:

+--------------------------------------+--------------------------------------+
|     1                                |     $node->created = !empty($node->d |
|                                      | ate) ? strtotime($node->date) : REQU |
|                                      | EST_TIME;                            |
+--------------------------------------+--------------------------------------+

Which means that node\_submit() will set \$node-\>created to the current
time if \$node-\>date doesn’t exist. So, if you want to set the creation
time, you have to do something like this:

+--------------------------------------+--------------------------------------+
|     1                                |     $node->date = "2009-05-27";      |
+--------------------------------------+--------------------------------------+

This also means that if you are updating a node programmatically, don’t
forget to set \$node-\>date; otherwise node\_submit() will change the
creation time. Or, when updating a node, simply don’t
use node\_submit(), since all it does is populate author and creation
time. See [Updating nodes programmatically in Drupal
7](http://fooninja.net/2011/06/06/updating-nodes-programmatically-in-drupal-7/) for
more about updating.

### Date field (datetime, date, datestamp)

If you use the [date](http://drupal.org/project/date) module, you get
three new field types - date, datetime and datestamp. (See [this
page](http://drupal.org/node/262066) to read about the differences; also
check out [this discussion](http://drupal.org/node/269813).)

If your field is named datetest, you could do:

+--------------------------------------+--------------------------------------+
|     1                                |     // For datetime                  |
|     2                                |     $node->field_datetest[$node->lan |
|     3                                | guage][0][value] = "2011-05-25 10:35 |
|     4                                | :58";                                |
|     5                                |                                      |
|     6                                |     // For date                      |
|     7                                |     $node->field_datetest[$node->lan |
|     8                                | guage][0][value] = "2011-05-25T10:35 |
|                                      | :58";                                |
|                                      |                                      |
|                                      |     // For datestamp                 |
|                                      |     $node->field_datetest[$node->lan |
|                                      | guage][0][value] = strtotime("2011-0 |
|                                      | 5-25 10:35:58");                     |
+--------------------------------------+--------------------------------------+

Note that you don’t need to specify a complete date; for datetime and
date you can just pad with zeros, e.g. “2011-05-00 00:00:00” (datetime),
“2011-00-00T00:00:00” (date), etc. For datestamp you could just do
e.g. strtotime("2011-05-25").

Important: Also note that while the exact value you specify will be
stored in the database, the actual time displayed on the site might be
different depending on timezone settings. When you create a new
datetime/date/datestamp field, you get to choose between [five different
timezone handling methods](http://drupal.org/node/767182). The default
one is “site’s time zone”:

> When entering data into the field, the data entered is assumed to be
> in the site’s time zone. When the data is saved to the database, it is
> converted to UTC.

However, if you set a date field programmatically like in the above
example then no conversion takes place, so make sure you account for the
field’s timezone settings. Or in other words, if you use “site’s time
zone”, make sure the time is in UTC.

### Boolean field

Single on/off checkbox:

+--------------------------------------+--------------------------------------+
|     1                                |     $node->field_bork[$node->languag |
|                                      | e][0]['value'] = 1;                  |
+--------------------------------------+--------------------------------------+

### Term reference (taxonomy) field

Set the term reference field tags to taxonomy term id 25 (for more than
one term, just repeat the line; and note that it doesn’t matter whether
the widget type is select list, check boxes/radio buttons or
autocomplete):

+--------------------------------------+--------------------------------------+
|     1                                |     $node->field_tags[$node->languag |
|                                      | e][]['tid'] = 25;                    |
+--------------------------------------+--------------------------------------+

*(If you are trying this with the tags field in the default article
content type, and have locale enabled, and have set \$node-\>language to
e.g. ‘en’, and it doesn’t seem to work: I also encountered this oddity
(bug?). For some reason tags are saved
to \$node-\>field\_tags[und] instead of \$node-\>field\_tags[en], while
e.g. body is saved to \$node-\>body[en] as expected. If I create a new
term reference field, it works as it should. So either do that, or
change the above to \$node-\>field\_tags[und][]['tid'].)*

As you can see, you need the know the taxonomy term’s id. Fortunately,
there’s a Drupal function to help us with
this: [taxonomy\_get\_term\_by\_name()](http://api.drupal.org/api/drupal/modules--taxonomy--taxonomy.module/function/taxonomy_get_term_by_name).
You supply the name (“Italy”) and it returns an array of matching term
objects, so you can do something like this:

+--------------------------------------+--------------------------------------+
|     1                                |     if($foo = taxonomy_get_term_by_n |
|     2                                | ame('Italy')) {                      |
|     3                                |         $foo_keys = array_keys($foo) |
|     4                                | ;                                    |
|                                      |         $node->field_tags[$node->lan |
|                                      | guage][]['tid'] = $foo_keys[0];      |
|                                      |     }                                |
+--------------------------------------+--------------------------------------+

Unfortunately, you can’t specify a certain vocabulary with this
function. This means that if you have the same term name in more than
one vocabulary, the above code will just use whatever happens to come
first. If you want to specify a certain vocabulary, say the one with id
9, you could do something like this:

+--------------------------------------+--------------------------------------+
|     1                                |     $foo = taxonomy_get_term_by_name |
|     2                                | ('Italy');                           |
|     3                                |     foreach($foo as $term) {         |
|     4                                |         if($term->vid == 9) {        |
|     5                                |              $node->field_tags[$node |
|     6                                | ->language][]['tid'] = $term->tid;   |
|                                      |         }                            |
|                                      |     }                                |
+--------------------------------------+--------------------------------------+

(One way of figuring out the vocabulary id is to
run print\_r(taxonomy\_get\_vocabularies());)

But what if you want to create new vocabulary terms for those that
aren’t already in the database? You can
use [taxonomy\_term\_save()](http://api.drupal.org/api/drupal/modules--taxonomy--taxonomy.module/function/taxonomy_term_save/7),
like this:

+--------------------------------------+--------------------------------------+
|     1                                |     $new_term = array(               |
|     2                                |         'vid' => 1,                  |
|     3                                |         'name' => 'Fugazi',          |
|     4                                |         // You can optionally also s |
|     5                                | et id of parent term, e.g. 'parent'  |
|     6                                | => 25                                |
|     7                                |     );                               |
|                                      |     $new_term = (object) $new_term;  |
|                                      |     taxonomy_term_save($new_term);   |
+--------------------------------------+--------------------------------------+

\$new\_term is very conveniently updated with the tid of our newly
created term, which you can get with \$new\_term-\>tid.

### Node references

If you use the [references
module](http://drupal.org/project/references) for node/user references
you can set a reference like this:

+--------------------------------------+--------------------------------------+
|     1                                |     // 453 is the id of the referenc |
|     2                                | ed node                              |
|                                      |     $node->field_author[$node->langu |
|                                      | age][]['nid'] = 453;                 |
+--------------------------------------+--------------------------------------+

This way you can also add multiple references in the same field - just
repeat.

### Entity field references

I haven’t had time to update this document properly (yet!) but Mark
Losey wrote the following in a comment (thanks Mark!):

+--------------------------------------+--------------------------------------+
|     1                                |     $node->field_my_reference[$node- |
|                                      | >language][0]['target_id'] = $entity |
|                                      | _id;                                 |
+--------------------------------------+--------------------------------------+

“If you are using the product reference field in drupal commerce it’s a
variation on that:”

+--------------------------------------+--------------------------------------+
|     1                                |     $node->field_product_reference[$ |
|                                      | node->language][0]['product_id'] = $ |
|                                      | product_id;                          |
+--------------------------------------+--------------------------------------+

### Image field

Attaching an image to any given image field is easy. Create a file
object, copy the file and associate the file object with the image
field:

+--------------------------------------+--------------------------------------+
|     1                                |     $file_path = drupal_realpath('fo |
|     2                                | o.jpg');                             |
|     3                                |     $file = (object) array(          |
|     4                                |                 'uid' => 1,          |
|     5                                |                 'uri' => $file_path, |
|     6                                |                 'filemime' => file_g |
|     7                                | et_mimetype($file_path),             |
|     8                                |                 'status' => 1,       |
|     9                                |     );                               |
|     10                               |     // You can specify a subdirector |
|                                      | y, e.g. public://foo/                |
|                                      |     $file = file_copy($file, 'public |
|                                      | ://');                               |
|                                      |     $node->field_image[$node->langua |
|                                      | ge][0] = (array) $file;              |
+--------------------------------------+--------------------------------------+

### Pathauto URL aliases

Joakim Hedlund comments (thanks!) that you can turn off the automatic
path aliases generated by pathauto with the following:

+--------------------------------------+--------------------------------------+
|     1                                |     $node->path['pathauto'] = FALSE; |
+--------------------------------------+--------------------------------------+

Note that you have to turn pathauto off like this if you don’t want it
to overwrite custom aliases. See [How does Pathauto determine if the
‘Automatic URL alias’ checkbox should be checked or
not?](http://drupal.org/node/1167612) for more information.



