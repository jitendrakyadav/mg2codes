Create and use of Block:
Here we will look, how we ceate Block and use/call it's functions in view(i.e. .phtml files) and in Controller. 
1. Go to VendorDashboard module.
2. There are 4 blocks:
   a. Block/Adminhtml/Sales/Sales.php
   b. Block/Adminhtml/Order/Order.php
   c. Block/Adminhtml/Product/Product.php
   d. Block/Adminhtml/Customer/Customer.php
3. These all blocks are particularly designed for backend/admin only that's why all are inside Adminhtml directory.
4. These blocks are used in corresponding templates:
   a. view/adminhtml/templates/sales/sales.phtml
   b. view/adminhtml/templates/order/
      A. recent_orders.phtml 
      B. order_history.phtml
   c. view/adminhtml/templates/product/top_selling_products.phtml
   d. view/adminhtml/templates/customer/top_customers.phtml
   These all templates are called in view/adminhtml/templates/dashboard.phtml
   These blocks/templates mappings are clearly mentioned in 
      view/adminhtml/layout/vendordashboard_dashboard_index.xml
      A layout file naming convention is nicely explained under branch 'create_a_magento_admin_page' in readme.txt under point 14.
      In other words, for each page in magento there is one layout file as above.
      Any template file can call their mapped block functions as mentioned in layout file using $block object (or might be using $this object).
   Remember, here is one main block and 4 sub-blocks like Sales.php, Order.php, Product.php, Customer.php 
5. An action of a Controller like:
      Controller/Adminhtml/CustomAjax/Index.php
      (Look here functions - getOrderChartData, getProductChartData, getCustomerChartData)
   could call any block's functions whether it mentioned or not-mentioned in layout file created for this action/page.
   
 
