#be.chiro.civi.idcache

This extension provides functions to retrieve ID's of CiviCRM entities
based on their names. It uses CiviCRM's cache to reduce database access.

A couple of examples might make things clear:

    // retrieve the group id of the group with name mygroup.
    $myGroupId = CRM_IdCache_Cache_Generic::getId('Group', 'mygroup');
    // retrieve an id of a custom field
    $myCustomFieldId = CRM_IdCache_Cache_CustomField::getFieldId('Volunteer_Information', 'camera_skill_level');
    // retrieve the field name of the custom field for use with the API.
    $myApiField = CRM_IdCache_Cache_CustomField::getApiField('Volunteer_Information', 'camera_skill_level');


The idea is to use this in combination with 
[org.civicoop.configitems](https://github.com/CiviCooP/org.civicoop.configitems).
Suppose you define your custom fields with configitems, like in the example file
[custom_groups.json](https://github.com/CiviCooP/org.civicoop.configitems/blob/master/resources_examples/custom_groups.json).
Then you can create classes like this:

```
public class MyFields() {
  static function SCHOOL_NAME() {
    return CRM_IdCache_Cache_CustomField::getApiField('college_degree', 'school_name');
  }
  static function GRADUATION_DATE() {
    return CRM_IdCache_Cache_CustomField::getApiField('college_degree', 'graduation_date');
  }
  // ...
}
```

Now every time you want to use these fields in an API call, you can do this as follows:

    $result = civicrm_api3('Contact', 'get', [
      MyFields::SCHOOL_NAME() => 'some school name',
    ]);

This has several advantages:

* your code does not depend on ID's directly, so it nicely testable.
* the ID's are cached, you don't have to access the database for knowing them.
* because of the static functions, your IDE will prevent you from writing typo's in field names.

As with all my extensions ;) this is work in progress. I use some kind of object oriented programming,
but it does not feels completely right. And the caching is only as good or bad as the cache system
your CiviCRM instance is using. But also as always, I will accept pull requests :-)
