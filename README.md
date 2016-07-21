# php-mime-mail-parser

A fully tested mailparse extension wrapper for PHP 5.4+

[![Latest Version](https://img.shields.io/packagist/v/php-mime-mail-parser/php-mime-mail-parser.svg?style=flat-square)](https://github.com/php-mime-mail-parser/php-mime-mail-parser/releases)
[![Total Downloads](https://img.shields.io/packagist/dt/php-mime-mail-parser/php-mime-mail-parser.svg?style=flat-square)](https://packagist.org/packages/php-mime-mail-parser/php-mime-mail-parser)
[![Software License](https://img.shields.io/badge/license-MIT-brightgreen.svg?style=flat-square)](LICENSE)

## Why?

This extension can be used to...
 * Parse and read email from Postfix
 * Create and send webmail 
 * Store email information such a subject, HTML body, attachments, and etc. into a database

## Is it reliable?

Yes. All known issues have been reproduced, fixed and tested.

We use Travis CI to help ensure code quality. You can see real-time statistics below:

[![Build Status](https://img.shields.io/travis/php-mime-mail-parser/php-mime-mail-parser/master.svg?style=flat-square)](https://travis-ci.org/php-mime-mail-parser/php-mime-mail-parser)
[![Coverage](https://img.shields.io/coveralls/php-mime-mail-parser/php-mime-mail-parser.svg?style=flat-square)](https://coveralls.io/r/php-mime-mail-parser/php-mime-mail-parser)
[![Quality Score](https://img.shields.io/scrutinizer/g/php-mime-mail-parser/php-mime-mail-parser.svg?style=flat-square)](https://scrutinizer-ci.com/g/php-mime-mail-parser/php-mime-mail-parser)

## How do I install it?

The easiest way is via [Composer](https://getcomposer.org/).

To install the latest version of PHP MIME Mail Parser, run the command below:

	composer require php-mime-mail-parser/php-mime-mail-parser

## Requirements

The following versions of PHP are supported:

* PHP 5.4
* PHP 5.5
* PHP 5.6
* PHP 7
* HHVM

Make sure you have the mailparse extension (http://php.net/manual/en/book.mailparse.php) properly installed: 

	pecl install mailparse      		#PHP Version = 7
	pecl install mailparse-2.1.6		#PHP Version < 7
	
	
If you have trouble installing mailparse on Ubuntu, take a look at [this tutorial](http://wiki.cerbweb.com/Installing_PHP_Mailparse_Ubuntu). 

Also note that you may need to create `/etc/php5/mods-available/mailparse.ini` file and include the line `extension=mailparse.so`. Then run `sudo php5enmod mailparse` to enable it.

## How do I use it?

```php
<?php
// Include the library first
require_once __DIR__.'/vendor/autoload.php';

$path = 'path/to/mail.txt';
$Parser = new PhpMimeMailParser\Parser();

// There are three methods available to indicate which mime mail to parse.
// You only need to use one of the following three:

// 1. Specify a file path to the mime mail.
$Parser->setPath($path); 

// 2. Specify a php file resource (stream) to the mime mail.
$Parser->setStream(fopen($path, "r"));

// 3. Specify the raw mime mail text.
$Parser->setText(file_get_contents($path));

// Once we've indicated where to find the mail, we can parse out the data
$to = $Parser->getHeader('to');             // "test" <test@example.com>, "test2" <test2@example.com>
$addressesTo = $Parser->getAddresses('to'); //Return an array : [[test, test@example.com, false],[test2, test2@example.com, false]]

$from = $Parser->getHeader('from');             // "test" <test@example.com>
$addressesFrom = $Parser->getAddresses('from'); //Return an array : test, test@example.com, false

$subject = $Parser->getHeader('subject');

$text = $Parser->getMessageBody('text');

$html = $Parser->getMessageBody('html');
$htmlEmbedded = $Parser->getMessageBody('htmlEmbedded'); //HTML Body included data

// Pass in a writeable path to save attachments
$attach_dir = '/path/to/save/attachments/';
$Parser->saveAttachments($attach_dir);

// Get an array of Attachment items from $Parser
$attachments = $Parser->getAttachments();

//  Loop through all the Attachments
if (count($attachments) > 0) {
	foreach ($attachments as $attachment) {
		echo 'Filename : '.$attachment->getFilename().'<br />'; // logo.jpg
		echo 'Filesize : '.filesize($attach_dir.$attachment->getFilename()).'<br />'; // 1000
		echo 'Filetype : '.$attachment->getContentType().'<br />'; // image/jpeg
	}
}

?>
```

## Can I contribute?

Feel free to contribute!

If you report an issue, please provide the raw email that triggered it. This helps us reproduce the issue and fix it more quickly.

### License

The php-mime-mail-parser/php-mime-mail-parser is open-sourced software licensed under the [MIT license](http://opensource.org/licenses/MIT)
