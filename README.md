
# static-card.liquid
## Inserted inside the checkout form, when submit it includes a coupon automatically applied on checkout, coupon contains price rules.
{% if customer.tags contains 'b2b' %}
  <input type="hidden" name="discount" value="b2b">
{% endif %}

# quantity-selector.liquid
## Inserted before the first quantity_increment_value assignment. This is the quantity setup for default value, min and step for b2b tagged customer.

{% assign qtyPerCase = nil %}
{% unless  customer.tags contains "b2b" %}
     <script>
          console.log("is not b2b ")
      </script>
    {% else %}
      {% if context == 'product' %}
          {% if product.metafields.custom.qtyPerCase %}
            {% assign  qtyPerCase = product.metafields.custom.qtyPerCase %}
          {% endif %}
      {% else %}
         {% if context == 'cart' %}
          {% if item.product.metafields.custom.qtyPerCase %}
            {% assign  qtyPerCase = item.product.metafields.custom.qtyPerCase %}
          {% endif %}
         {% endif %}
    {% endif %}
{% endunless %}

# empire.js.liquid
## Inserted before the ProductDetails class. Responsible for the custom total next to the quantity input label in product page (hidden on cart context), 

class GlobalVariables {
    static currentPrice = 1.00;
    static currentQty = 1;
    static money_format = null;
    static showContent = null;
    static updateCurrentPrice(newPrice) {
    GlobalVariables.currentPrice = newPrice;
    GlobalVariables.newCost()
    }
    
    static updateCurrentQty(newQty) {
    GlobalVariables.currentQty = newQty;
    console.log("updating updateCurrentQty")
    GlobalVariables.newCost()
    }
    static updateMoneyFormat(money_format) {
    GlobalVariables.money_format = money_format
    }
    static newCost() {
        const totalCost = ((GlobalVariables.currentPrice * GlobalVariables.currentQty)/100).toFixed(2);  
        const spanElement = document.querySelector('[data-product-context-total]');
        let totalCostToMoney = Shopify.formatMoney(totalCost, GlobalVariables.money_format)
        spanElement.innerHTML = totalCostToMoney;
    }
}

### Method implementation locations
#### On _updatePrice(variant) 
- updateCurrentQty
- updateCurrentPrice

#### On _editItemQuantityOnProductContext(target)
- updateCurrentQty

polcadots
hedara
  -Nothing follows-