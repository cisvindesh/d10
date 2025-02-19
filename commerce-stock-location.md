In **Drupal Commerce 2**, if you need to create a `commerce_stock_location` entity programmatically, follow these steps:

---

### **Step 1: Ensure Required Modules Are Installed**
1. **Drupal Commerce**: Ensure you have **Drupal Commerce** and **Commerce Stock** modules installed.
   ```bash
   composer require drupal/commerce_stock
   drush en commerce commerce_stock -y
   ```

2. **Check Entity Availability**: The `commerce_stock_location` entity is provided by modules like **Commerce Stock API** or **Commerce Stock Location**. Ensure these are installed.

---

### **Step 2: Create a Commerce Stock Location Programmatically**
Use the following code in a custom module or via a script:

```php
use Drupal\commerce_stock\Entity\StockLocation;

$values = [
  'name' => 'Main Warehouse',  // Set the location name
  'status' => TRUE, // Active status
  'uid' => 1, // Assign user (optional)
  'field_address' => [  // Optional: If using an address field
    'country_code' => 'US',
    'locality' => 'New York',
    'administrative_area' => 'NY',
    'postal_code' => '10001',
    'address_line1' => '123 Main Street',
  ],
];

// Create and save the entity
$stock_location = StockLocation::create($values);
$stock_location->save();

OR

use Drupal\commerce_stock_local\Entity\StockLocation;
$query = \Drupal::entityQuery('commerce_stock_location')
    ->condition('type', 'default')
    ->condition('name', 'Warehouse-2')
    ->accessCheck(TRUE);
  $results = $query->execute();
  if(count($results) == 0) {
    $values = [
      'name' => 'Warehouse-2',  // Set the location name
      'status' => TRUE, // Active status
      'type' => 'default',
    ];
    // Create and save the entity
    $stock_location = StockLocation::create($values);
    $stock_location->save();
  }

```

---

### **Step 3: Verify the Created Entity**
- Run `drush cache:rebuild` to clear cache.
- Check in the admin UI under **Commerce → Stock Locations** to see if the new location appears.

---

### **Optional: Load an Existing Stock Location**
If you need to load an existing `commerce_stock_location` entity by ID:
```php
$stock_location = StockLocation::load($location_id);
```

To load all stock locations:
```php
$stock_locations = \Drupal::entityTypeManager()->getStorage('commerce_stock_location')->loadMultiple();
```

---

### **Step 4: Assign Stock Location to a Product**
If you want to associate a stock location with a **Commerce Product**:
```php
$product = \Drupal\commerce_product\Entity\Product::load($product_id);
$product->set('stock_location', $stock_location->id());
$product->save();
```

---

### **Final Notes**
- Ensure you clear cache after adding new locations:  
  ```bash
  drush cr
  ```
- If using address fields, confirm the **Address module** is installed and enabled.

This method allows you to create and manage **Commerce Stock Locations** dynamically in Drupal Commerce 2. 🚀
