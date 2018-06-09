# openvpn-online-user-manager
Openvpn get online user from managment

Set ```management localhost 6666``` in server.conf and get online user from it with php

```php

<?php
function get_online_users() {

	$fp = fsockopen("localhost", 6666, $errno, $errstr, 30);

	if (!$fp) {
		return [];

	} else {
	    fwrite($fp, "status\n");
	
	    $data = [];
	    $do_get = false;
	    while (!feof($fp)) {
	        $get = fgets($fp, 128);
	  
	    	if( preg_match('#ROUTING TABLE#', $get ) )
	    		break;

	        if( $do_get )
	    		$data[] = $get; //trim( explode( ",", $get )[0] );

	        if( preg_match('#Common Name#i', $get ))
	        	$do_get = true;

	    }


	    fclose($fp);
	}

	return $data;
}

print_r( get_online_users() );

?>
```
