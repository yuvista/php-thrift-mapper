# PHP-Thrift-Mapper

Convert a PHP array into an Apache Thrift struct type.

[![Build Status](https://travis-ci.org/edvakf/php-thrift-mapper.svg)](https://travis-ci.org/edvakf/php-thrift-mapper) [![Coverage Status](https://coveralls.io/repos/edvakf/php-thrift-mapper/badge.svg?branch=coveralls&service=github)](https://coveralls.io/github/edvakf/php-thrift-mapper?branch=coveralls)

## What is this?

A Thrift struct;

```
struct Bonk
{
  1: string message,
  2: i32 type
}
```

generates a PHP source like the following.


```php
class Bonk {
  static $_TSPEC;

  /**
   * @var string
   */
  public $message = null;
  /**
   * @var int
   */
  public $type = null;

  public function __construct($vals=null) {
    if (!isset(self::$_TSPEC)) {
      self::$_TSPEC = array(
        1 => array(
          'var' => 'message',
          'type' => TType::STRING,
          ),
        2 => array(
          'var' => 'type',
          'type' => TType::I32,
          ),
        );
    }
    if (is_array($vals)) {
      if (isset($vals['message'])) {
        $this->message = $vals['message'];
      }
      if (isset($vals['type'])) {
        $this->type = $vals['type'];
      }
    }
  }

  public function getName() {
    return 'Bonk';
  }
```

Now, if I want to convert my PHP array to this class, there is no easy way.

## Here comes the ThriftMapper

It populates the Thrift object with the PHP array.

```php
$ary = [
  "message" => "Hello!",
  "type" => 123,
];

$bonk = ThriftMapper::map(new Bonk(), $ary);

var_dump($bonk);
```

This code outputs;

```
object(ThriftTest\Bonk)#19 (2) {
  ["message"]=>
  string(6) "Hello!"
  ["type"]=>
  int(123)
}
```
