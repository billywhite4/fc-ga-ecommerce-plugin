#FoxyCart Transaction Data to Google Analytics Ecommerce Plugin
This script mines transaction data in the FoxyCart Receipt HTML along with product data from `fc_json` to be sent to the Google Analytics Ecommerce plugin for tracking purchase details

In early versions of FoxyCart, `fc_json` does not pass data for the entire transaction and only includes details on individual products in the purchase. Thus, transaction data like shipping, tax, and transaction total must be gleaned from the Receipt teamplate HTML, while product-specific data like name, price, quantity, etc. are pulled from `fc_json`. 

#Dependencies
This specific example is for a FoxyCart 0.7.1 store on an FC subdomain like yourstore.foxycart.com, but it should still work on any pre-v2.0 store with light modification. Instructions may vary for custom subdomains like yourstore.yourdomain.com.

A prerequisite for this to work is setting up your Google Analytics Goal Conversions based on the detailed instructions here on the FoxyCart wiki:
https://wiki.foxycart.com/integration/googleanalytics_universal

Those instructions obviously also require Google Analytics Universal Analytics (analytics.js).

The data structure and methods for sending to GA is largely borrowed from the Examples on the Google Developers guide for the Ecommerce plugin:
https://developers.google.com/analytics/devguides/collection/analyticsjs/ecommerce

This script does not use or depend on jQuery, though if you prefer to use jQuery to find transaction data from the Receipt HTML values instead of using the `fcTextStripper()` function, that jQuery is below:

```javascript
var transId = $('.fc_order_id').find('.fc_text').html(); // transaction ID
var transSubtotal = stripUSD($('.fc_order_subtotal').find('.fc_text').html()); // transaction subtotal
var transShipping = stripUSD($('.fc_order_shipping').find('.fc_text').html()); // transaction shipping amount
var transTax = stripUSD($('.fc_order_tax').find('.fc_text').html()); // transaction tax amount
var transTotal = stripUSD($('.fc_order_total').find('.fc_text').html()); // transaction total
```

#Notes
This setup is for USD currency, but the `stripUSD()` function could be altered or extended to strip the prefixes of other currencies.

Categories in `fc_json` are the Shipping/Discount category set in FoxyCart, and are not necessarily taxonomic categories set in a CMS for grouping products for analysis. This should be considered if your FC categories are different than your product categories.

New installations of FoxyCart using 2.0 or higher should use the built-in [Google Analytics integration](https://wiki.foxycart.com/v/2.0/analytics).