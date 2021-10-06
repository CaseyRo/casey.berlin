---
Title: # if missing, it's parsed from the first heading if the content starts with one

Slug:  # if missing, will be obtained from file/directory name

Category: something          # this can be a comma-delimited string, or a YAML list

Tags:     bar, baz           # this can be a comma-delimited string, or a YAML list

Author:   caseyromkes@gmail.com    # user id is looked up by email address or user login

Excerpt: |  # You can set a custom excerpt, which can contain markdown
  Some *amazing* blurb that makes you want to read this post!

Date: 2017-12-11 13:41 PST        # Dates can be anything recognized by PHP, and
Updated: April 30, 2018 3:46pm    # use Wordpress's timezone if no zone is given

Status:    # string, Wordpress `post_status`
Draft: true    # unquoted true/false/yes/no -- if true or yes, overrides Status to `draft`
Template:  # string, Worpdress `page_template`
WP-Type:   # string, Wordpress `post_type`, defaults to 'post'

WP-Terms:  # Worpdress `tax_input` -- a map from taxonomy names to terms:
  some-taxonomy: term1, term2   # terms can be a string
  other-taxonomy:               # or a YAML list
    - term1
    - term2

ID: urn:uuid:65ed1fea-c098-48d0-b08c-e97b6177b655 # UUID part of this

Comments:   # string, 'open' or 'closed', sets Wordpress `comment_status`
Password:   # string, sets Wordpress `post_password`
Weight:     # integer, sets Wordpress `menu_order`
Pings:      # string, 'open' or 'closed', sets Wordpress `ping_status`
MIME-Type:  # string, sets Wordpress `post_mime_type`

Post-Meta:  # array of meta keys -> meta values; only the given values are changed
  a_custom_field: "Good stuff!"
  _some_hidden_field: 42
  delete_me: null  # setting to null deletes the meta key

Set-Options:  # an option path or array of option paths; each will be set to the post's db ID
  - edd_settings/purchase_history_page  # e.g., make this page the "my account" page for both EDD
  - lifterlms_myaccount_page_id         # and LifterLMS.  See "Working With Options" for more info

HTML:  # override markdown conversion for specific fields
  Excerpt: "<p>This is html</p>"   # a string means, "use this HTML instead of what's in the field"
  body: true                       # non-false non-string means, "field is HTML, not markdown"
---
