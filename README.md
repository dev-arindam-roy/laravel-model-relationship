# laravel-model-relationship
Laravel model relationship

```php
<?php

namespace App\Models;

use Illuminate\Database\Eloquent\Factories\HasFactory;
use Illuminate\Database\Eloquent\Model;
use App\Models\ProductBundleFree;
use App\Models\Products;
use App\Models\Brand;
use App\Models\Unit;

class ProductVariants extends Model
{
    use HasFactory;

    protected $table = 'product_variants';
    protected $primaryKey = 'id';

    public function baseProduct() {
        return $this->belongsTo(Products::class, 'product_id', 'id');
    }

    public function productUnit() {
        return $this->belongsTo(Unit::class, 'unit_id', 'id');
    }

    public function productBrand() {
        return $this->belongsTo(Brand::class, 'brand_id', 'id');
    }

    public function childProducts() {
        return $this->hasMany(ProductBundleFree::class, 'variant_id', 'id');
    }

    public function bundleProducts() {
        return $this->hasMany(ProductBundleFree::class, 'variant_id', 'id')->where('type', 1);
    }

    public function freeProducts() {
        return $this->hasMany(ProductBundleFree::class, 'variant_id', 'id')->where('type', 2);
    }
}

```

```php
<?php

namespace App\Models;

use Illuminate\Database\Eloquent\Factories\HasFactory;
use Illuminate\Database\Eloquent\Model;
use App\Models\ProductVariants;
use App\Models\Purchase;
use App\Models\Batch;
use App\Models\User;
use App\Models\Unit;
use App\Models\Sale;

class SaleProduct extends Model
{
    use HasFactory;

    protected $table = 'sale_products';
    protected $primaryKey = 'id';

    public function saleInfo() {
        return $this->belongsTo(Sale::class, 'sale_id', 'id');
    }

    public function purchaseInfo() {
        return $this->belongsTo(Purchase::class, 'purchase_id', 'id');
    }

    public function batchInfo() {
        return $this->belongsTo(Batch::class, 'batch_id', 'id');
    }

    public function vendorInfo() {
        return $this->belongsTo(User::class, 'vendor_id', 'id');
    }

    public function productInfo() {
        return $this->belongsTo(ProductVariants::class, 'product_id', 'id');
    }

    public function unitInfo() {
        return $this->belongsTo(Unit::class, 'unit_id', 'id');
    }
}

```

```php
public function userRoles() {
    return $this->hasMany(UserRole::class, 'user_id', 'id');
}

public function userProfile() {
    return $this->hasOne(UserProfile::class, 'user_id', 'id');
}

public function orders() {
    return $this->hasMany(Cart::class, 'customer_id', 'id');
}

/** Same table relation */
public function customerAgent() {
    return $this->belongsTo(User::class, 'agent_id', 'id');
}
```
