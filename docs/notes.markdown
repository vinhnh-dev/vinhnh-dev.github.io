---
layout: page
title: Small Notes
permalink: /notes/
parent: Miscellaneous
nav_order: 7
---

## Redis

### Connect 
```
redis-cli -h <host> -p <port> -a <password>
Ex: redis-cli -h 127.0.0.1 -p 6379 -a mypassword

# Select database
SELECT 0

# Find session keys
KEYS *

# Hoặc tìm cụ thể theo pattern session
KEYS PHPREDIS_SESSION:*
```

### Check data type
```
TYPE <key>
Ex: TYPE sess_5f9377272105deb125efc57b3cef1737
```

### Get gzip compressed data
```
redis-cli -h 0.0.0.0 -p 6370 HGET sess_5f9377272105deb125efc57b3cef1737 data \
| php -r "
\$raw = file_get_contents('php://stdin');
\$raw = trim(\$raw);
if (strpos(\$raw, ':gz:') === 0) {
    \$raw = substr(\$raw, 4);
}
\$out = @gzuncompress(\$raw) ?: @gzinflate(\$raw) ?: @gzdecode(\$raw);
echo \$out ?: 'Cannot decompress';
"
```

### Sample compressed data 

```
_session_validator_data|a:4:{s:11:"remote_addr";s:13:"203.56.157.52";s:8:"http_via";s:0:"";s:20:"http_x_forwarded_for";s:13:"203.56.157.52";s:15:"http_user_agent";s:111:"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/146.0.0.0 Safari/537.36";}default|a:3:{s:9:"_form_key";s:16:"vqjItcHxCFEqp3zA";s:12:"visitor_data";a:7:{s:13:"last_visit_at";s:19:"2026-03-24 06:05:07";s:10:"visitor_id";s:7:"2234031";s:8:"quote_id";s:6:"956088";s:15:"do_quote_create";b:1;s:17:"do_customer_login";b:1;s:11:"customer_id";s:5:"94623";s:10:"session_id";s:32:"5f9377272105deb125efc57b3cef1737";}s:24:"previous_active_sessions";a:1:{i:0;s:32:"351915983ad868d55078f20439860bc1";}
```

### Unserialize data 
```
<?php
$raw = 'Paste the whole string here';

// Parse each namespace
$result = [];
$offset = 0;
while ($offset < strlen($raw)) {
    $pos = strpos($raw, '|', $offset);
    if ($pos === false) break;
    $varname = substr($raw, $offset, $pos - $offset);
    $offset = $pos + 1;
    $value = unserialize(substr($raw, $offset));
    $result[$varname] = $value;
    $offset += strlen(serialize($value));
}

echo json_encode($result, JSON_PRETTY_PRINT | JSON_UNESCAPED_UNICODE);
```


## Xdebug

Run cron job with Xdebug enabled

```
 PHP_IDE_CONFIG="serverName=mcdev.test.com" XDEBUG_TRIGGER=yes ./n98-magerun2.phar sys:cron:run <cron-job-code>
```

## Linux


Get list files modified in last 5 days
```
find /app \( -name ".php" -o -name ".js" \) -mtime -5 -type f
```

Decrypt value saved in database server

If you place the script at:

Magento root: var/decrypt.php
then __DIR__ is <magento_root>/var, so you must require the bootstrap one directory up.

Create var/decrypt.php with this content:
```
<?php
declare(strict_types=1);

use Magento\Framework\App\Bootstrap;

require __DIR__ . '/../app/bootstrap.php';

$bootstrap = Bootstrap::create(BP, $_SERVER);
$objectManager = $bootstrap->getObjectManager();

/** @var \Magento\Framework\Encryption\EncryptorInterface $encryptor */
$encryptor = $objectManager->get(\Magento\Framework\Encryption\EncryptorInterface::class);

// Paste the encrypted DB value here (example: from core_config_data.value)
$encrypted = '0:YOUR_ENCRYPTED_VALUE_HERE';

echo $encryptor->decrypt($encrypted) . PHP_EOL;

```

### Magento 

Test db connection 
```
php -r '
$e=require "app/etc/env.php";
$c=$e["db"]["connection"]["default"];
$dsn=sprintf("mysql:host=%s;port=%s;dbname=%s;charset=utf8",$c["host"],$c["port"]??3306,$c["dbname"]);
$u=$c["username"]; $p=$c["password"];
$t=microtime(true);
$pdo=new PDO($dsn,$u,$p,[PDO::ATTR_TIMEOUT=>5]);
echo "PDO OK in ".round(microtime(true)-$t,3)."s\n";
'
```