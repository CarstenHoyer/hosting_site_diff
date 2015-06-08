# hosting_site_diff
This is an Aegir 2.4 module for comparing site configuration setups. It is cuurently missing its main functionality.

The code can be found at Drupal.org: https://www.drupal.org/sandbox/cahodk/2502585

This is a mini article explaining the module.

## Aegir tasks
Aegir has a frontend and a backend (called hosing and provision). The frontend lets the administrator setup new platforms, sites, tasks etc. The backend runs drush commands in the context of the sites controlled by Aegir.

Hosting modules are installed as regular modules in the hostmaster folder, while Provision modules are installed as drush extension in the .drush directory.

This module consist of hosting_site_diff and provision_site_diff.

In hosting_site_diff I define a new hosting task:

```
/**
 * Implements hook_hosting_tasks().
 */
function hosting_site_diff_hosting_tasks() {
  $tasks = array();

  $tasks['site']['snapshot'] = array(
    'title' => t('Config Snapshot'),
    'description' => t('Make a snapshot available for site diff.'),
    'dialog' => TRUE,
    'hidden' => FALSE,
    );

  return $tasks;
}

/**
 * Implements hook_node_operations().
 */
function hosting_site_diff_node_operations() {
  $operations = array();

  $operations['site-diff-snapshot'] = array(
    'label' => t('Site: Config Snapshot'),
    'callback' => 'site_diff_op_snapshot',
    );

  return $operations;
}

/**
 * Callback for site diff operation.
 */
function hosting_site_diff_op_snapshot($nodes) {
  foreach ($nodes as $nid) {
    hosting_add_task($nid, 'site_diff_snapshot');
  }
}
```
