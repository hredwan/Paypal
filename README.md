## Wakanda Paypal Module ##

This is a module that implements the Paypal REST API for Wakanda, it's still very basic, but it will allow you to handle payment easily.

If you want to use this module, it's highly recommended that's you don't use it directly in your waPage, but build another module on top of it.

for now it only implements paypal payments, I'm working on other parts too like ExpressCheckout.
### Getting Started ###

In order to start using the paypal module:

1. create another JS Module in Wakanda (i.e: payments.js)
        
        var module = require('paypal');
        
        var paypalModule = new module.Paypal();
        var payments = {};
        
        payments.create   = function(params)
        {
        	
        	var response 	= 	paypalModule.payments.create(params.intent, params.payer, params.transactions, params.redirect_urls);
        	//you can format the response first, check for errors etc
        	return response;
        }
        
        for( var element in payments )
        {
        	exports[element] = payments[element];
        }

2. Setup your credentials in paypal/config.js (you can use sandbox to get test credentials)

        config.client_id = "AYdFyhBqEOdJMdurm-oaUWAgO5bXIm52anvLctjSNePa856eQ3XS4nOaSM9D";
        config.secret    = "YOUR_SECRET";
        
3. Set payment.js you've created as RPC Service and Include it in your page.
4. in your page, you can add a form that capture credit card details (first name, last name, card number, cvv2, expire date(year & month), type (visa, mastercard, discover, amex))
5. in your page script add a code that handles the payment, it would like this:

        Submit.click = function Submit_click (event)
          {
        		var credit_card = {};
        		
        		credit_card.type			= $$('type').getValue();
        		credit_card.number			= $$('number').getValue();
        		credit_card.cvv2			= $$('cvv2').getValue();
        		credit_card.expire_month	= $$('expire_month').getValue();
        		credit_card.expire_year		= $$('expire_year').getValue();
        		credit_card.first_name		= $$('first_name').getValue();
        		credit_card.last_name		= $$('last_name').getValue();
        				
        		var payer = {};
        		
        		payer.payment_method = "credit_card";
        		payer.funding_instruments = [{"credit_card": credit_card}];
        		
        		var transactions = [
        			    {
        			      "amount": {
        			        "total": "7.47",
        			        "currency": "USD"
        			      },
        			      "description": "This is the payment transaction description."
        			    }
        			  ];
        		//optional	  
        		var redirect_urls  = {return_url: "www.wakanda.org", cancel_url:"www.wakanda.org"}	  
        		
        	    
        
        		var response = payment.createAsync({
        					onSuccess:function(data)
        					{
        						alert(data.state);
        					},
        					params: [{
        						    intent: "sale",
        						    payer: payer,
        						    transactions: transactions,
        						    redirect_urls: redirect_urls
        						}]
        		});
        									
        	};
          
6. Checkout the details about the fields in [Paypal Developer Center](https://developer.paypal.com/webapps/developer/docs/api/)          
