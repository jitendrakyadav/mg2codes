Asynchronous and Bulk Web API/Endpoints:

Magento Supports:
    1. Magento 2.3.1 added support for Asynchronous and Bulk API/Endpoints.
    2. Bulk endpoints work on AMQP(Advanced Message Queuing Protocol).
    3. AMQP supports Asynchronous-Messaging.


How actually Asynchronous and Bulk Web API/Endpoints works:
    1. It supports only POST & PUT API methods.
    2. Magento has created many bulk-handlers for the same.
    3. When we prefix assync/ with the synchronous Web API, it starts working as asynchronously request.
    4. When we prefix assync/bulk, the synchronous Web API started working as asynchronously bulk request.
    5. It means, no new APIs for this, it works with older APIs using newly developed bulk-handler in Magento.

What is RabbitMQ and what role it plays in Magento 2.3.1:
    1. It’s a AMQP supporting tool/server.
    2. It’s written in Erlang programming language.
    3. It uses many jargon like exchange, producer, queue, receiver, etc.
    4.	Magento has code-support for any 3rd party AMQP tool like RabbitMQ.

What Advantages of using Bulk API with RabbitMQ:
    1. When a bulk API calling occurs, bulk-handler activated and it redirect the request to RabbitMQ.
    2. RabbitMQ breaks this in many separate unique requests and store all requests in their queue.
    3. Actually, Magento works hear as message producer.
    4. It holds all request inside queue until Magento ready to receive it.
    5. Queue request could be receives through some fixed Magento commands or any CRON having these commands.

How this new Magento feature changes our code-flow/architecture:
    1. I have check for bulk-customer creation and it was successful.
       Attached: bulk_api_request.png, bulk_api_response.png, rabbitmq_screen.png, rabbitmq_screen_after_consumer_run.png, consumers_listing.png, run_consumer.png
    2. I have check for bulk-product creation and it was successful.
       Attached: product/ asynchronous_product_call_request.png, product/ asynchronous_product_call_response.png, product/ rabbitMQ_print_screen.png
    3. There is no need to develop middle-layer for product & customer creation.
    4. For order creation support as well, I hope there don’t need to develop middle-layer. I have not looked much more for it, but to support the same, we would need to develop a new Magento Module for it.
    5. Reason behind it is, order creation does not occure just using a sinle API, it requires calling many APIs, like:
	a. First setting store
	b. Create admin token
	c. Create customer
	d. Create Cart
	e. Add items in cart
	f. Call API to get shipping cost
	g. Get address as well
	h. Then create order API with payment

Step by Step Implementation:
    1. Install RabbitMQ server:
	https://www.rabbitmq.com/download.html
    2. Install Magento 2.3.1 and added following in setup:
	--amqp-host="<hostname>" --amqp-port="5672" --amqp-user="<user_name>" --amqp-password="<password>" --amqp-virtualhost="/"
    3.If Magento installed already, then add code for RabbitMQ support in app/etc/env.php as following:
	'queue' => [
        	'amqp' => [
            	'host' => 'localhost',
            	'port' => '5672',
            	'user' => 'admin',
            	'password' => 'admin@123',
            	'virtualhost' => '/',
            	'ssl' => ''
        	]	
    	]
	Attached: configure_message_queue.png
    4. Start RabbitMQ server using following commands:
	rabbitmq-server start
    5. Install plugins to use RabbitMQ GUI as following:
    6. rabbitmq-plugins.bat enable rabbitmq_management
    7. Use: http://localhost:15672 to access RabbitMQ using following default credentials:
	Username: guest
	Password: guest
       Attached: rabbitmq_screen.png
    8. At this time, you might use setup:upgrade in Magento
    9. Get Admin token using following rest api:
	http://mgwd231.local.com/rest/V1/integration/admin/token
	having header: Content-Type: application/json
    10.Now use this to call a bulk asynchronous API as following:
	http://mgwd231.local.com/rest/async/bulk/V1/customers
	having headers: 
	Content-Type: application/json
	Authorization: Bearer ‘Admin token’
	Having request-body like:
	[{
                "customer": {
                                "email": "rupesh@cybage.com",
                                "firstname": "Rupesh",
                                "lastname": "Jadhav"
                },
                "password": "admin@123"
	},
	{	
                "customer": {
                                "email": "pritesh@cybage.com",
                                "firstname": "Pritesh",
                                "lastname": "Kumar"
                },
                "password": "admin@123"
	}]
	Attached: bulk_api_request.png
    11.	It’s response as:
	{
    		"bulk_uuid": "c2622723-1766-4f78-a41b-808e229c5c8e",
    		"request_items": [
        		{
            			"id": 0,
            			"data_hash": null,
            			"status": "accepted"
        		},
        		{
            			"id": 1,
            			"data_hash": null,
            			"status": "accepted"
        		}
    		],
    		"errors": false
	}
	Attached: bulk_api_response.png
    12.	Come to RabbitMQ screen, it shows ready-items in queue.
	i.e. it doesn’t disturb Magento DB till now, it just push their request in RabbitMQ.
    13.	(Windows + r) -> Search ‘rabbitmq’, click there as Windows install RabbitMQ as their service. 
    14.	If not starts yet, start rabbitmq server as following:
	rabbitmq-server start
    15.	Go to Magento root-directory and use following commands:
	php bin/magento queue:consumers:list
	php bin/magento queue:consumers:start async.operations.all
    16.	Look on RabbitMQ screen, queue is blank now as items have been pushed to Magento.
	Attached: rabbitmq_screen_after_consumer_run.png
    17.	Check Magento DB and Admin customer-grid, new customers are showing there.

Supporting URLs & Study matterials:
    1. https://devdocs.magento.com/guides/v2.3/rest/asynchronous-web-endpoints.html
    2. https://devdocs.magento.com/guides/v2.3/rest/bulk-endpoints.html
    3. https://devdocs.magento.com/guides/v2.3/install-gde/prereq/install-rabbitmq.html
    4. https://devdocs.magento.com/guides/v2.3/rest/list.html
    5. https://webkul.com/blog/here-we-will-learn-how-to-configure-and-use-rabbitmq-in-magento-2-3/
    6. https://youtu.be/m9GdUOtxhhA

Supporting new added Magento Modules:
    1. framework-amqp
    2. framework-bulk
    3. framework-message-queue
    4. module-message-queue

Supporting new added Magento tables:
1. magento_bulk
2. magento_acknowledged_bulk
3. queue
4. queue_lock
5. queue_message
6. queue_message_status
