Simple Product:
Conceptually, It's a physical product in Magento 2 having shipping options; like Shirt, Trouser, Bag, etc.
1. How we create:
    a. Go to Magento-Admin > CATALOG > Products > Use 'Add Product' dropdown in header by selecting 'Simple Product'
    b. Select 'Attribute Set' which suites your product.
       Concept behind EAV Attribute, Attribute Group, Attribute Set:
       1. EAV Attribute:
    c. Fill all mandatory fields as well as other important fields.
    d. Save the Simple Product.
    e. You could see in 'catalog_product_entity' table under 'type_id' column, there stores 'simple'.
2. Customizable Options:
    a. It is an option/facility available on the admin interface while you are creating any new product.
    b. It is available while you are creating:
        i.  Simple Product
	ii. Virtual Product
	iii.Configurable Product
	iv. Downloadable Product
       and not available while you are creating:
	i.  Grouped Product
	ii. Bundle Product
    c. Purpose: Suppose you have added a product 'motog' mobile with plastic back cover and you have set it's price 25000 and it is displayed ok in frontend with all their available fields value. Now you want provide an option to your customer that they can go with mobile having default back-cover with plastic body or now they can get the mobile with steel and silver back cover as well by paying additional 2000 and 5000 more. respectively So, for this, now create an option using this as like: 
        i.  Option Title: Back Cover
	ii. Option Type: Drop-down
	iii.Required(i.e. mandatory): Checked/Unchecked
	Now option created, now create values available for this option:
	    i.  Title: Steel
	    ii. Price: 2000
	    iii.Price Type: Fixed	//Fixed/Percent(of base price)
	    iv. SKU: steel
	    setting 2nd value for above created option:
	    i.  Title: Silver
	    ii. Price: 5000
	    iii.Price Type: Fixed	//Fixed/Percent(of base price)
	    iv. SKU: silver
	Print-Screen: creating_customizable_options.jpg
	It would be displayed in frontend as dropdown with product having possible selection values as steel+2000/silver+5000.
	Print-Screen: customizable_options_use_demo_in_frontend.jpg
