---
title: WordPress
layout: page
parent: App Notes
---

## Change Domain
If you plan on using a custom domain or restoring an existing site, it's best to set the custom domain *before* completing the WordPress installation assistant.

If you need to change the domain after installation, follow those steps:

1. First change the domain in the `wp_options` table, options `home` and `siteurl` using the pod's [database access](/faq/#accessing-the-pods-database) feature.
2. Update the domain in your *PikaPods* control panel.
3. Finally, some resources, like images may still point to the old domain on pages or posts. Solution is to use a [search and replace plugin](https://wordpress.org/plugins/better-search-replace/) to properly update those references. Otherwise images and files may keep referencing the old domain.


## Adjust PHP Settings
The default upload limit is set to 64 MB. You can increase this or adjust other PHP settings by editing the `uploads.ini` file using [SFTP](/faq/#accessing-pod-files-using-sftp).