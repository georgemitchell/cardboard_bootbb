# cardboard_bootbb
Enhnacements to the BootBB theme to support the CardBoard.

## Template changes

### Login Issues
The latest version of MyBB includes a hidden key in form posts to prevent CSRF exploits.  This generally takes the form of having the following hidden field in any form requiring a post:

```
<input name="my_post_key" type="hidden" value="{$mybb->post_code}"/>
```

According to [this article](http://bialoaded.com/thread-can-t-login-mybb-forum-authorization-code-mismatch-please-read), the following templates need to be updated to allow for successful login:

* error_nopermission
* header_welcomeblock_guest
* member_login
* portal_welcome_guesttext

### WYSIWYG Issues
We have noticed problems with the WYSIWYG editor on certain browsers / OSes.  Although the editor can be disabled either globally or per user, we have a fix that works on browsers with known issues.  We use the user agent string to detect whether the client is using an iPad or iPhone and if so, default to source mode.  In the:

* ungrouped_codebuttons

```
<script type="text/javascript">
	var agent_re = /^Mozilla[\/]5[.]0 [(]iP(ad|hone).+/g;
	var disable_wysiwyg = agent_re.exec(navigator.UserAgent) != null;
var partialmode = {$mybb->settings['partialmode']},
opt_editor = {
	plugins: "bbcode,undo",
	style: "{$mybb->asset_url}/jscripts/sceditor/textarea_styles/jquery.sceditor.{$theme['editortheme']}?ver=1808",
	rtl: {$lang->settings['rtl']},
	locale: "mybblang",
	enablePasteFiltering: true,
	autoUpdate: true,
	emoticonsEnabled: {$emoticons_enabled},
	runWithoutWysiwygSupport: disable_wysiwyg,
	emoticons: {
...
```