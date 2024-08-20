# Nodeping Ansible Role

This role is able to create and update NodePing checks.

## Caveats

Since we're unable to save IDs in Ansible the label you pass with a check is the primary key. Means, if this label exists, the existing check will be updated, if the label does not exist, a new check will be created. This could potentially cause duplicates.

## Variables

* `nodeping_api_token`: Your API token to the Nodeping API.
* `nodeping_checks`: An array of objects with checks. An object can have this values:
  * `label` (Required): Display name of the check and "primary" key to check whetever a check has to be created or updated (see above).
  * `target` (Required): Target of your check.
  * `type`: Type of your check. Defaults to either `HTTPADV` if `username` has been provided for basic authentication or else, `HTTP`.
  * `username`: Username for a check. We use it primarly for HTTP checks with basic authentication, but if you provide a `type` which supports `username`, it will work as well.
  * `password`: Password for a check. We use it primarly for HTTP checks with basic authentication, but if you provide a `type` which supports `password`, it will work as well.
  * `port`: Port to use with the check.
  * `state`: State of check. Must be either *present* or *absent*
  * `contentstring`: The string to match the response against. Can be negated with the `invert` parameter.
  * `invert`: Used for 'Does not contain' functionality in checks, in conjunction with `contentstring`. If omitted, defaults to no.
  * `sens`: Integer for the number of rechecks before this check is considered 'down' or 'up', possible values are 2, 5, 7, 10. The Sensitivity determines how much the service will recheck a website before concluding it is down. There are five available settings of High (2), Medium (5), Low (7), and Very Low (10). High is generally recommended, but if your website is prone to flapping, you may want to set the Sensitivity lower. If omitted, defaults to 2 (High)
* `nodeping_notifications`: An array of objects with target to notify. Important: We won't create those, so be sure to manually create them in advance. An object needs to include these values:
  * `contact`: An ID of the contact you want to notify.
  * `notifydelay`: Delay until Nodeping will notify this contact.
  * `notifyschedule`: When this contact should be notified. Nodeping provides several [default values](https://nodeping.com/nodepingnotifications.html#scheduling), but you could add new ones as well.