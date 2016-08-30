# Examples

Examples of usage of Inventorus.

## PHP SDK

### Getting started

Start with creating client instance. You need to use composer's autoloader.

```PHP
require_once "vendor/autoload.php";

$client = new Inventorus\InventorusClient($yourApiKey);
```

### Fetch Steam apps

You can fetch applications from user's inventory.

```PHP
$apps = $client->getUsersApps('76561198080641698');
foreach ($apps as $app) {
  echo 'User has app ' . $app->name;
}
```

### Fetch Steam items

Either fetch items from applications you obtained previously, or request items from specific app.
For example fetch CS:GO backpack.

```PHP
$items = $client->getUsersItems('76561198080641698', 730, array(2));
foreach ($items as $item) {
  echo 'User has item ' . $item->name;
}
```

### Create import trade

You need to create import trade to import items into your warehouse.

```PHP
$import = new Inventorus\Models\ImportRequest;
$import->steamid = '76561198080641698';
$import->tradeToken = 'x3CeVhts';
$import->message = 'Hello person!';
$item = new stdClass;
$item->appid = 730;
$item->contextid = 2;
$item->assetid = '6144820778';
$import->items = array($item);
$import->successUrl = 'http://myapplication.com/import-hooks/success';
$import->failUrl = 'http://myapplication.com/import-hooks/fail';
$import->updateUrl = 'http://myapplication.com/import-hooks/update';
$importId = $client->createImport($import);
echo 'Import with id ' . $importId . ' was created.';
```

### Get import history

Use history to look at your created import trades.

```PHP
$history = $client->getImportHistory();
foreach ($history as $importId) {
  echo 'You have created import with id ' . $importId;
}
```

### Get import trade

You can check your import trade state anytime,
but we recommend using hook urls instead of constantly checking.

```PHP
$import = $client->getImport('e69c3df0-2da8-4a41-970e-bd4b4a864df5');
echo 'Import has status ' . $import->status;
```

### Create export trade

Create export trade to move item out of your warehouse.

```PHP
$export = new Inventorus\Models\ExportRequest;
$export->steamid = '76561198080641698';
$export->tradeToken = 'x3CeVhts';
$export->message = 'Hello person!';
$export->items = array('d93518cc-9235-4828-8cdf-fd15bb19d3ce');
$export->successUrl = 'http://myapplication.com/export-hooks/success';
$export->failUrl = 'http://myapplication.com/export-hooks/fail';
$export->updateUrl = 'http://myapplication.com/export-hooks/update';
$exportId = $client->createExport($export);
echo 'Export with id ' . $exportId . ' was created.';
```

### Get export history

Use history to look at your created export trades.

```PHP
$history = $client->getExportHistory();
foreach ($history as $exportId) {
  echo 'You have created export with id ' . $exportId;
}
```

### Get export trade

You can check your export trade state anytime,
but we recommend using hook urls instead of constantly checking.

```PHP
$export = $client->getExport('c5bac711-1958-4d69-ad8b-7c828aafde8f');
echo 'Export has status ' . $export->status;
```

### List warehouse

You may need to list your own items stored in warehouse.

```PHP
$items = $client->listWarehouse();
foreach ($items as $item) {
  echo 'You own item with id ' . $item;
}
```

### Get item details

Listing warehouse returns only ids of your items.
You often need to get more information about your item.

```PHP
$details = $client->getItemDetails('d93518cc-9235-4828-8cdf-fd15bb19d3ce');
echo 'Item has name ' . $details->name;
```
