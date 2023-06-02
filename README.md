
# static-card.liquid
### Inserted inside the checkout form, when submit it includes a coupon automatically applied on checkout, coupon contains price rules. input value depends on coupon code
```
{% if customer.tags contains 'b2b' %}
  <input type="hidden" name="discount" value="b2b">
{% endif %}
```
# quantity-selector.liquid
### Inserted before the first quantity_increment_value assignment. This is the quantity setup for default value, min and step for b2b tagged customer.

```
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
```

# empire.js.liquid
### Inserted before the ProductDetails class. Responsible for the custom total next to the quantity input label in product page (hidden on cart context), 

```
class GlobalVariables {
      static currentPrice = 1.00;
      static currentQty = 1;
      static money_format = null;
      static showContent = null;
      static isb2b = null;
      static spanWSP = null;
      static cartTotals = 0.00;
      static addMorePriceContainer = null;

      static updateCartTotals(newTotals, newAddMorePriceContainer ) {
        
        const convertedAmount = Number(newTotals.replace(/[$,]/g, ""));
        const resultForAddMore = (250.00 - convertedAmount) < 0 ? 0 : (250.00 - convertedAmount).toFixed(2) ;
        
        GlobalVariables.cartTotals = convertedAmount;
        if(GlobalVariables.addMorePriceContainer == null) {
           GlobalVariables.addMorePriceContainer = newAddMorePriceContainer; 
        } 
         
         document.querySelector('[data-add-more-price]').innerHTML = "$" + resultForAddMore
        //GlobalVariables.addMorePriceContainer.innerHTML = Shopify.formatMoney(resultForAddMore, GlobalVariables.money_format)
        console.log(convertedAmount, "updateCartTotals",Shopify.formatMoney(resultForAddMore, GlobalVariables.money_format))
        
      }
      
      static updateWSPContainer(newSpanWSP) {
        GlobalVariables.spanWSP = newSpanWSP;
      }
      
      static updateCurrentPriceAndQty(newPrice, newQty) {
        GlobalVariables.currentPrice = newPrice;
        GlobalVariables.currentQty = newQty;
        GlobalVariables.newCost()
      }
      static updateCurrentPrice(newPrice) {
        GlobalVariables.currentPrice = newPrice;
        GlobalVariables.newCost()
      }
      static updateCurrentQty(newQty) {
        GlobalVariables.currentQty = newQty;
        GlobalVariables.newCost()
      }
      static updateMoneyFormat(money_format) {
        GlobalVariables.money_format = money_format
      }
      static updateB2BStatus(isB2B) {
        GlobalVariables.isb2b = isB2B
        console.log(GlobalVariables.isb2b, "GlobalVariables.updateB2BStatus")
      }
      static newCost() {
        const totalCost = ((GlobalVariables.currentPrice * GlobalVariables.currentQty)/100).toFixed(2);
        const b2bPrice = ((GlobalVariables.currentPrice * 0.5)/100).toFixed(2)
         //document.querySelector('[data-b2b-wsp]').innerHTML = Shopify.formatMoney(b2bPrice, GlobalVariables.money_format)
        const b2bCost = (totalCost/2).toFixed(2)
        let finalCost =  totalCost
        if(GlobalVariables.isb2b) {
          finalCost =  b2bCost
          if(GlobalVariables.spanWSP) {
            console.log(GlobalVariables.spanWSP, "GlobalVariables.spanWSP")
            GlobalVariables.spanWSP.innerHTML = Shopify.formatMoney(b2bPrice, GlobalVariables.money_format)
            
          }
        }
        const spanElement = document.querySelector('[data-product-context-total]');
        //const spanWSP = document.querySelector('[data-b2b-wsp]');
        let totalCostToMoney = Shopify.formatMoney(finalCost, GlobalVariables.money_format)
        spanElement.innerHTML = totalCostToMoney;
        // if(spanWSP) {
        //   //spanWSP.innerHTML = Shopify.formatMoney(b2bPrice, GlobalVariables.money_format)
        // }
         
      }
    }
      
```

### Method implementation locations
#### On _updatePrice(variant) method
- updateCurrentQty
- updateCurrentPrice

#### On _editItemQuantityOnProductContext(target) method
- updateCurrentQty

polcadots
hedara
  -Nothing follows-