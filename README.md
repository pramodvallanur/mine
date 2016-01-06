# Monetization #

## Phase Zero ##

### Scope ###

 - Fee associated to a plan
 - Support only a fixed or a flat fee
	 - No usage based pricing
 - Support for only two fee structures - The rate limits at the plan
   apply irrespective of the fee structure
	 - a. One-time : Single fee charged for life time access to the plan  
	 - b. Recurring: On a periodic basis 
		 - b.1: Only Calendar month 
		 - b.2: For interim period sign up, we support only pro-rata 
		 - b.3: Pre month billing: For example, Andre would be charged upfront for the month 
 - Support only one currency to be setup 
 - No support for refund  
 - No taxation 
 - No invoice generation
 - No Analytics around Monetization

We shall provide a sample integration to one payment gateway on Git. The same could be used to extend or rewrite other PG integrations by customers or services.

The below write up does not suggest of doing this within APIM or should be achieved through Webhooks or through vendor extension or this being completely new component, this gives us a chance to write this as a micro service ?

I have written this assuming its done through User Interface

### API Manager: Plan Definition ###

 - There are 2 APIs defined
	 - Routes
	 - Forecast
 - Steve creates a Climb-On Product
 - Adds both the APIs in to the Product
 -  Creates 3 different plans
	 - Bronze : Routes
	 - Silver: Forecast
	 - Gold: Routes & Forecast
 - For each plan, he ticks if the plan is monetized
	- Bronze: None (Free)
	- Silver: Yes
	- Gold: Yes
 - If the plan is defined as Monetized, Monetization section, captures the details
	 - Charge Type: One-time or Recurring
		- Silver: Recurring
		- Gold: One-time
	 - Fee: The price of the plan
		 - Silver: 10
		 - Gold: 200
	 - Currency: Specifies the Currency used for charging (Should currency be common across all plans in the product and should this be a parameter at product level)
		- Silver: USD
		- Gold: EUR

### Developer Portal ###

 - Andre logs in to Developer portal, views the Climb-On Product
 - Views the APIs included in the product & the plans in the Product
	 - Silver & Gold plan carries additional details around Monetization 	   provided by Steve

**Bronze Plan Sign up**

 - Registers an App called B-App 
 - Signs up for Bronze plan against the B-App 
 - As Bronze plan is not a monetized plan, the flow is same as today

**Sign up for Monetized Plans**

 - First time Silver or Gold plan Sign up
	 - Selects Silver/Gold Plan, clicks on Use this plan
	 - As the plan is monetized
	   - He is directed to a page to capture his credit card details
	   - No card details or no PCI information would be stored in APIM
	   - A Unique Id for the card profile is returned back along with validation status
	   - He is re-directed to the developer portal, On successful validation of the card, Andre is shown the App selection screen against the plan
	   - Unique Id for card profile and date of subscription is stored against the subscription

 - Subsequent Gold or Silver plan Sign up
 	 - Selects Silver / Gold Plan, click on Use this plan
 	 - As the plan is monetized
	 	 - Check if any Unique ids for card profile is stored against any subscription that Andre has
	 	 - If available, call the PG SDK to show the card details and asks for confirmation as to if same card to be used
	 	 - If Andre wants to enter a new card, he is directed to CC page Rest of the steps remains same

### Biller Component ###
- A scheduler that run every day
	- Check if the day is start of the month
		- If not
			- Query all new subscription for the past day against monetized plans
			- With subscription Id, retrieve subscription date, charge type & Fee
			- If charge type is recurring, then calculate actual fee to be charged for pro-rated billing
		- If start of the month
			- Query all active subscriptions against monetized plans
			- With subscription Id, retrieve the Fee against the plan
	- Retrieves the Card profile against each subscription
	- Iteratively calls the Payment Gateway with the profile & the fee to be charged against it
	- Payment gateway shares the status of each payment request again in batch or as a req-response based on Payment Gateway Integration

- Handling Payment Failures
	- Based on the Payment gateway status, if failed the app subscription would be suspended

### Other Processes ###

 - Initial Monetization setup
	 - We enable only one PG setup per install across catalogs, across provider orgs in phase zero
	 - Sign up of the API provider as a merchant with the PG would be done offline
	 - Similar to how how we used ADP on Git, we shall share 'An' PG integration on Git
		 - Typically this could include using the SDK from the PG which internally has the merchant Id, secret etc. so that all we do is call the SDK from portal & the biller component
