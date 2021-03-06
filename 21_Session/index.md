{
	"topic": "Session Bundle"
}

## Configuration

Your session configuration file is located in your app config directory.

_File:_ `app/config/session.config.php`

Every session has it's own configuration, means defining for example the lifetime in the first configuration dimension won't work.

### main session

Defines the session manager that get's used when no manager instance name is given.

```php
// CCSession::manager();
'main' => array(
	...
),

// CCSession::manager( 'other' );
'other' => array(
	...
),
```

### Options

<table class="table table-bordered">
	<tr>
		<th>Key</th>
		<th>Default</th>
		<th>Description</th>
	</tr>
	<tr>
		<td>lifetime</td>
		<td><code>0</code></td>
		<td>The session lifetime. 0 means just during the browser session.</td>
	</tr>
	<tr>
		<td>min_lifetime</td>
		<td><code>CCDate::minutes(5)</code></td>
		<td>The minimum session lifetime, this one is a bit diffrent than the normal lifetime. This defines how long a session is valid without any user action.</td>
	</tr>
	<tr>
		<td>gc.enabled</td>
		<td><code>true</code></td>
		<td>Garbage collector, deletes old and outdated sessions when enabled.</td>
	</tr>
	<tr>
		<td>gc.factor</td>
		<td><code>25</code></td>
		<td>The factor defines how often should gc be executed, ca. every x request. You should scale this value with the number of page views you got. When you plaan a massive system you should disable gc and delete old sessions using a background job.</td>
	</tr>
	<tr>
		<td>driver</td>
		<td><code>file</code></td>
		<td>
			Choose the driver to store the sessions with.
			We hopefully ship with:
			<ul>
				<li>array ( Just for testing purposes. )</li>
				<li>file</li>
				<li>cookie</li>
				<li>database</li>
			</ul>
		</td>
	</tr>
</table>

### Example

This is an example file driver configuration:

```php
'main' => array(
	'driver'	 => 'file',
	
	// The session lifetime. 0 means just during the browser session.
	'lifetime'	=> 0,
	
	// The minimum session lifetime, this one is a bit diffrent 
	// This defines how long a session is valid without any user
	// action.
	'min_lifetime'	=> CCDate::minutes(5),
	
	// Garbage collector
	// The gc deletes old and outdated sessions.
	'gc' => array(
		'enabled' => true,
		
		// The factor means how often should gc be executed.
		// ca. every x request. You should scale this value 
		// with the number of page views you got. When you plaan
		// a massive system you should disable gc and delete old
		// sessions using a background job.
		'factor' => 25,
	),
),
```

## Drivers

CCF ship with the following drivers:

 * **file**: _PHP serialized._
 * **json**: _json encoded file._
 * **cookie**: _json encoded and crypted string in the cookies._
 * **db**: _Stored using the DB handler._
 
 
### Cookie Driver

The cookie driver has some custom options:

<table class="table table-bordered">
	<tr>
		<th>Key</th>
		<th>Default</th>
		<th>Description</th>
	</tr>
	<tr>
		<td>cookie_suffix</td>
		<td><code>-session-store</code></td>
		<td>The name used for the cookie that holds the session data.</td>
	</tr>
	<tr>
		<td>crypt_salt</td>
		<td><code>null</code></td>
		<td>A salt to encrypt the cookies with, to avoid user manipulation.</td>
	</tr>
</table>
 
 
### Database Driver

The database driver has some custom options:

<table class="table table-bordered">
	<tr>
		<th>Key</th>
		<th>Default</th>
		<th>Description</th>
	</tr>
	<tr>
		<td>database</td>
		<td><code>null</code></td>
		<td>The database instance used to store the data.</td>
	</tr>
	<tr>
		<td>table</td>
		<td><code>sessions</code></td>
		<td>The table used to store the session data.</td>
	</tr>
	<tr>
		<td>index_fields</td>
		<td><code>array( ... )</code></td>
		<td>An array of session fields that are indexed. These fields wont get serialised. This means they should exist as columns in the database table.</td>
	</tr>
</table>
