# About

This library can be very helpful to standardize date-time formats in your project & get done easy with different clients timezones.

# Features

For all features using examples see [/example/index.php](https://github.com/barbushin/dater/blob/master/example/index.php).

### Timestamp/datetime input formats support

	$dater = new Dater(Dater_Locale_English());
	echo $dater->datetime();
	echo $dater->datetime(time());
	echo $dater->datetime(date('Y-m-d H:i:s'));
	
### Formats binding

	$dater->format(time(), 'd/m/Y'); // 2013/03/14
	$dater->addFormat('slashed_date', 'd/m/Y');
	$dater->format(time(), 'slashed_date'); // 2013/03/14

### Format options

All [date()](http://php.net/date) format options available and can be overrided or extended:

	$dater->addFormatOption('ago', function (DateTime $dateTime) {
		return floor((time() - $dateTime->getTimestamp()) / 86400) . ' days ago';
	});
	$dater->format(time() - 60*60*24*7, 'd F Y, ago'); // 14 March 2013, 7 days ago

### Locales support

	$dater->setLocale(new Dater_Locale_English());
	echo $dater->date(); // 03/21/2013
	echo $dater->now('j F Y'); // 21 March 2013
	
	$dater->setLocale(new Dater_Locale_Russian());
	echo $dater->date(); // 21.03.2013
	echo $dater->now('j F Y'); // 21 марта 2013
	
### Standard server & user format methods

	echo $dater->date(); // 03/21/2013 (depends on locale)
	echo $dater->time(); // 5:41 AM (depends on locale)
	echo $dater->datetime(); // 03/21/2013 5:41 (depends on locale)
	echo $dater->serverDate(); // 2013-03-21
	echo $dater->serverTime(); // 09:41:28
	echo $dater->serverDateTime(); // 2013-03-21 09:41:28
	echo $dater->clientDate(); // 2013-03-21
	echo $dater->clientTime(); // 05:41:28
	echo $dater->clientDateTime(); // 2013-03-21 05:41:28
	
### Timezones convertion

	$dater->setServerTimezone('Europe/Moscow');
	$dater->setClientTimezone('Europe/London');
	echo $dater->serverDateTime(); // 2013-03-21 08:18:06
	echo $dater->clientDateTime(); // 2013-03-21 04:18:06
	echo $dater->time(); // 04:18
	
### Timezone auto-detection

Based on JavaScript [jsTimezoneDetect](http://pellepim.bitbucket.org/jstz/) library with sending result to server by COOKIE.

	$timezoneDetector = new Dater_TimezoneDetector();
	echo '<html><head>' . $timezoneDetector->getHtmlJsCode() .'</head></html>'; // <script>...</script>
	echo $timezoneDetector->getClientTimezone(); // Europe/London
	
### Convert request datetime to server timezone

Is usefult to auto-convert all client request datetime data to server timezone.

	$_GET = array('filter' => array('startsFrom' => '2012-12-12 12:00:00'));
	$_POST = array('event' => array('starts' => '2012-12-12 12:00:00'));
	$_REQUEST = array_merge($_GET, $_POST);
	$daterDataHandler = new Dater_DataHandler($dater);
	$daterDataHandler->convertRequestDataToServerTimezone(); // all '2012-12-12 12:00:00' replaced to '2012-12-12 10:00:00'

### Convert text template datetime timezone

Is usefult to auto-convert all datetime in template date to client timezone. For example in Email template body.
	
	$data = 'Timestamp format: 1363238564 (will not be handled)
	Timestamp format: 1363238564[Y/m/d]
	Timestamp format: 1363238564[datetime]
	Server datetime format: 2013-03-14 09:22:44[Y/m/d]
	Server datetime format: 2013-03-14 09:22:44[time]
	Server datetime format: 2013-03-14 09:22:44';
	echo $daterDataHandler->handleDataTimezone($data); 
	
Will print:
	
	Timestamp format: 1363238564 (will not be handled)
	Timestamp format: 2013/03/14
	Timestamp format: 14.03.2013 07:22
	Server datetime format: 2013/03/14
	Server datetime format: 07:22
	Server datetime format: 2013-03-14 07:22:44
	
### Convert output datetime to client timezone
	
	$daterDataHandler->enableOutputTimezoneHandler();
	echo $data; // $data from previos example will print the same as in prevous example
	

## Recommended
* Google Chrome extension [PHP Console](http://goo.gl/b10YF)
* Google Chrome extension [JavaScript Errors Notifier](http://goo.gl/kNix9)
