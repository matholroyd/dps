# Direct Payment Standard


## Brainstorming

Publish a Direct Payment Standard (DPS) that allows vendors to advertise their accepted payment options, which can be used by buyers directly or 3rd-party services to complete arbitrary payment transactions. 

- DPS would probably want to be a service that responds to a HTTPS GET request, at an advertised URL endpoint, with e.g. a simple JSON array of payment options, e.g. PayPal/Credit card entered directly/specific cryptocurrencies/country or region specific direct payment options/etc.

- The buyer should make a get request that specifies the user to pay, e.g. bob.smith@example.com. So a complete GET request might be https://example.com/dps?vendor=bob.smith@example.com
  - That said, probably useful to have a default vendor account that is paid out to, which would likely be useful for companies + people with personal websites that use their name, e.g. matholroyd.com.

- DPS URL could be advertised via DNS, e.g. with TXT entries, or on homepage of the website itself, e.g. by adding a hidden metatag to the home page.
  - TXT entry could be "dps:endpoint url=https://example.com/dps"
  - metatag could be <meta property="dps:endpoint" content="https://example.com/dps">. Note that for a homepage HTML meta tag, the domain of the webpage will often be different to the root domain, e.g. www.example.com vs example.com. This might be a problem since might not be a good idea to have a subdomain specify the DPS endpoint for root domain. E.g. ideally vendors can specify their DPS address to look similar to an email, so bob@example.com, but probably not good idea to let www.example.com say where the DPS endpoint is for example.com.

- DPS service should then also be able to respond to additional queries
  - e.g.
    - Get new cryptocurrency payment address
    - Get direct credit card payment form/URL
    - Get PayPal payment endpoint
    - Get list of services/products
    - Get list of vendors
    - Request refund for an earlier transaction
    
- For vendors, there is a lot of work to get a functioning DPS service as specificed above up and running. Hence probably want to provide both plugins for popular web frameworks + standalone DPS server that is easy to setup and run + a managed DPS service.
  - For managed DPS service, this could be like a Patreon, quasi-Stripe/PayPal replacement, where user creates an account, then sets up payout options (e.g. link Stripe account, link PayPal account), then also add some pre-baked purchase options for buyers, e.g. $10/month subscription.
  - For standalone DPS service, would be similar to UI managed DPS service for vendors.

## Introduction  & Description of core problem 

The current state of censorship being enforced on pockets of the online community is undesirable by preventing peaceful and lawful transactions from occuring. This censorship exposes a clear weakness of the current standard practice within the tech community of enabling online payments- that is, the widespread reliance of vendors on 3rd-party services for processing payments puts vendors at the mercy of these centralized payment processors, payment processors who reserve the right to revoke their services at any time. Hence this setup comes in direct conflict with the right of businesses and their audience/customers/clients to transact lawfully and peacefully with one another.

In short, the status quo in the tech community is not sufficienly meeting the rights of private citizens to voluntarily transact with one another.

DPS's goal is to address this shortcoming head on, by allowing transactions on tech platforms to happen more directly between the 2 parties, and thus be inheritently more resilient to MITM attacks (which censorship is a type of). 

## Goal

On an abstract level, what we want to do is within the dev community is elevate payment processing from being handle by a payment processor directly (e.g. like using Twitter), it is instead an open standard that is run by a platform directly or outsourced to a generic provider (e.g. like email). With this shift, payment processing changes from being 100% dependent on 3rd-parties to an inheritantly decentralized protocol which is thus more flexible and robust to MITM attacks (censorship is a type of MITM attack), and may help reduce other risks/attacks such as doxxing.

Come up with a censorship-resistant protocol/standard, that can be used by vendors to more directly take payments from buyers while abstracting away some or all of the reliance on 3rd-party services, 3rd-party services that may reneg on their promise to process transactions between vendor and buyer. 

Additional goals
- UI presented to buyers for PayPal and credit cards should be no more complicated (e.g. have same or less number of steps; require the same or less information) then on your average interface that accept credit cards or PayPal. 
- UI for vendors should be easy enough for the average non-tech user to fill out the details needed to start accepting payments. This is likely going to require recommended standards for 3rd-party plugins, since DPS is likely going to need support for plugins writtne by the community for e.g. region/country-specific merchant accounts, as well as plugins for languages + web frameworks + CMSs (i.e. WordPress).


## Method


### Overcoming the Censorship problem

- If DPS can initiate membership on incidental, secondary platforms when a Buyer interacts with a Vendor directly, then this can help make Vendors more resistant to platform de-platforming. 
  - E.g. if Vendors advertise their DPS address only, and interacting with a DPS address can inform a Buyer about the Vendor's products/services/subscriptions/etc directly, and then a purchase automatically adds the Buyer to the Platform (and whatever benefits/consequences that has, such as giving access to exclusive content), then that process itself is resistant to deplatforming. 
  - It is resistant to deplatforming because:
    - If the Vendors infrastructure/marketing/etc points Buyers to the DPS service, then that doesn't need to change when a deplatforming event occurs
    - The Vendor is able to transition Buyers from one platform to another, whether or not their products/services/subscriptions/etc can remain intact or not.
  


### Questions

Why is the DPS standard needed?

- A standardized protocol (like the email SMTP protocol) is a proven way to ease the burden on developers when they need to switch from one service provider to another. In short, without a standardized protocol, the burden is likely to be too high for the tech community to bother moving away from the major payment processing platforms, e.g. PayPal and Stripe, and thus it simply will not happen. 

- For instance, currently it is quite common for websites that accept payments to only implement a very small number payment processors, with the most common number of payment options offered by websites being 1. And the and payment processors choosen is (obviously) most likely to be the number 1 or number 2 player for that market region, e.g. PayPal or Stripe for US. On top of that, many sectors of the tech economy have a small number of players that dominate each segment, e.g. Patreon for online membership subscripotions, YouTube for videos, etc. In combination this means vendors are highly exposed to censorhop attacks applied by their service channels (e.g. YouTube) and whatever limited number of payment processor options those channels offer (e.g. SubscribeStar had their PayPal option removed).

- Hence DPS allows vendors to circumvent or reduce the reliance upon these 3rd-party services directly for payment, and instead process it themselves, or with a trusted 3rd-party DPS service who they can change on a whim. 

How will DPS reduce censorhip specifically?

- DPS increases the burden of MITM attacks (which censorship is a type of) by 
    - 1) Making it easier to change from one payment processor to another.
    - 2) Making it easier to setup backup payment processors in the event one or more payment processing providers are unavailable.
    - 3) Making platforms less ideal targets, since DPS can faciliate direct relationships between Vendors and Buyers, so that a platform's ability to censor a Vendor and a Buyer is more limited.
    - 4) Fragmenting the attackers since it becomes harder for them to agree on a target (since platforms become less ideal targets) as well as increasing the number of independent targets (instead of being able to go after 1 big target, e.g. a platform, attackers may need to attack 100s of targets to get the same effect as before, e.g. individuals/business directly), which thus makes said attacks less likely to occur and less effective.

Why will buyers need or want to use DPS for payments?

- There are some cases where buyers would want to do use DPS for their direct benefits, but there are also indirect reasons for buyers using DPS
  - Some direct reasons included:
    - For push-style payments, DPS is a good candiate to act as a name-resolver for payments since there is no limit to what currencies it could resolve for. E.g. all cryptocurrency payments become trivial since DPS addresses are easier to enter + remember when completing a purchase. This in turn means a wider variety of transactions can occur, e.g. paying someone from your mobile when all they have to tell you is their DPS address.
    - DPS addresses can be stored more readily by buyers in e.g. an address book, which for example can be used by accounting software to pay suppliers. 
    - Since DPS is intended to support push-payments out-of-the-box, buyers might want to use DPS to facilitate their day-to-day push-payments (i.e. to do push-payments for some subscription service as opposed to pull-payments).
    - Can bolster privacy since payments can reduce the number of 3rd-parties involved, or even eliminate altogther in the case of cryptocurrency payments. Additionally privacy for cryptocurrencies can be bolstered by generating unique cryptocurrencies addresses each transaction (or even splitting up transactions).
  - Some indirect benefits to DPS addresses are:
    - Although buyers might not want to use a DPS system, vendors might use it to help them combat censorship, since it becomes easier for them to shift from one DPS provider to another. Hence buyers will benefit by being able to interact with a larger pool of vendors, vendors who cannot be as easily censored.
  
Technical challenges?

- Different payment gateways have different APIs + rely on direct control of the UI experience, e.g. PayPal asks user to sign in.
  - Hence when platform is integrating DPS, the most flexible system would need to handle a variety of workflows, e.g. taking a user to a 3rd-party website vs payments being fulfilled at some indeterminate time.

- Although a platform might use DPS 100%, and as thoroughly as possible, how is that actually going to benefit buyers/vendors?
  - One way DPS can benefit users is to protect Vendors from the PayPal/SubscribeStar situation, where the payment processor (PayPal) revoked access to the platform (SubscribeStar). DPS would reduce the likelihood of that happening if Vendors were able to still transact on a Platform even if the Platform's payment processors access has been revoked.
  - Can DPS save Vendors from the Patreon problem though? Only if the actual service offered by the platform is itself a standarized service that can be switched for an alternative, e.g. perhaps something like EOS DApp. Otherwise, saving Vendors from the Patreon problem, is not fully possible, as Platforms will want/need to maintain the right to deny anyone service.
    - That said, DPS could help indirectly by reducing the pressure applicable to platforms, e.g. since platform doesn't need direct control of payment processors, payment processors' rules/terms-of-service may not apply at all to the platform. 
  - E.g. if a Patreon clone uses DPS, that doesn't mean the Patreon clone wont' shut down a Vendor's account in the future. So if there is going to be a benefit to Vendors, it would have to be the relationship between Vendor + Buyer persists somehow, in a meaningful way, beyond the platform. 
    - If we accept that DPS is used in a way that means e.g. subscriptions are persisted directly between Buyers and Vendors, that would make it more difficult for a platform to charge platform fees, i.e. they'd need to come up with a different business model, e.g. subscriber count fee, content hosting fees. 
    - One way this could happen is the subscription between Buyer and Vendor could be directly tied to each other. But is that really significant? Its useful for a Patreon type platform, but it doesn't help when the platform is something like Amazon (unrelated one-off purchases), Youtube (ad revenue organized by the platform).
  
  
What are the challenges to DPS becoming mainstream?

- There is a lot of initial research and development that will need to be done such that common transactions can be completed end-to-end via DPS.

- There is a shift in the attitude of developers + vendors + buyers required, to ensure DPS ultimately proves successful. Additionally tech users are effectively going to need to learn a new payment API, which means the API documentation needs to be high quality and the API itself needs to be well designed.

- DPS exposes parties to attack vectors that they would not normally be exposed to. 
  - For instance:
    - If a vendor runs a DPS service themselves, they are more directly resposible for ensuring e.g. credit card details are not leaked.
    - Buyers might be at higher risk of fraudulent transactions, since the payment processor might be done by a smaller vendor who is more difficult to get a refund from.
  - It should be noted though, that for each of the above mentioned risks, therr a clear mediation steps buyers/vendors can take.
    - Vendors can continue to use trusted 3rd-party services to process payments, e.g PayPal, Stripe, to handle credit cards, customer details.
    - Buyers can refuse to complete transactions with vendors who don't offer a payment option they trust.
    - Vendors can protect themselves from common attack vectors by following basic security practices, e.g. hold money for X days before assuming its safe to withdraw, dont' store client details.
    
### Benefits/problems

Platforms

- Benefits
  - Platform could use a 3rd-party DPS service to handle all payment stuff, and thus require minimum coding to actually get up and running.
  - A DPS library could significantly cut-down coding workload by abstracting awat the different gateways.
  - Attract Vendors/Buyers by offering a more robust service.
  - Avoid pressure from payment processors since payments not being handled by the Platform.
  - Can still deplatform users at will.

- Problems
  - UI experience could be worse if the user is taken to multiple other sites, e.g. taken to 3rd-party DPS service, then taken to PayPal sign in.
  - Certain transaction-fee-based business models might be untenable if e.g. the users of the site transact directly with one another.
  - The platform cannot gaurantee the reliability of the overall service, since it now depends on Vendor DPS services.
  - Offering DPS may mean more Buyers/Vendors are likely to transact off-platform, which reduces the business value of attracting those users in the first place.
  - If transcations happen off-platform, the Platform might not be able to determine whether transactions actually happened. Hence can have situations where Vendor and Buyer disagree about reality, e.g. did the Buyer actually pay; did the Vendor actually give a refund? Now the Platform lacks critical information to resolve these problems.
  - The platform may not be able to initiate refunds reliably, meaning they Buyers would be at the mercy of Vendors.
  - The platform may not be able to do pull transactions at all, and hence this makes the workflow for the Platform and Vendors more complicated.
   
Vendors

- Benefits
  - Ability to set up additional payment processors for accepting money means the Vendor has a more robust payment processing workflow.
  - Can setup direct relationships with Buyers more readily, meaning future transactions can occur off-platform, even beyond a deplatforming event.
  - Refunds could be 100% under the control of the Vendor, and not the whims of the Platform.

- Problems
  - DPS doesn't alleviate the direct problem of deplatforming.
  - Vendor is now responsible either running DPS or picking a reliable 3rd-party DPS service to get paid.
  - Refunds might be the responsibility of the Vendor.

Buyers

- Benefits
  - Get access to Vendors who would not otherwise operate due to deplatforming.
  - Keep track of payments/spending across multiple platforms.
  - Could accept only push transactions.
  
- Problems
  - Instead of relying on the credibility of a Platform to "do the right thing", Buyers might be at the mercy of Vendors, e.g. for refunds.
  - Could be at higher risk of fraud since e.g. CC details might be directly sent to Vendors, as opposed to only the Platform.
  - Might be required to setup a DPS service to pay Vendors, which is extra effort. That entails being forced to make a decision (e.g. find a trustworty DPS service) when normally you could just trust the Platform.

### Initial development

For DPS to have initial success, it will likely require the upfront development of a variety of open source software and fully-function services, parts of which can in combination satisfy 100% end-to-end transactions that are most common within the community that is most need of a robust payment solution.

E.g.
- A patreon clone that adheres to the DPS standard.
- An out-going payment service specifically to take advantage of the DPS address standard, meaning it can process payments like "send $20 US to bob@example.com via PayPal".
- Plugin for popular CMSs + frameworks that allows vendors to accept payments on their site, which itself adheres to the DPS standard.
  - Good candidates would be:
    - Wordpress plugin for vendor-specific workflows
    - Ruby + python libraries for low-level interaction, probably as a buyer
    - Rails + Django libraries/plugins, for high-level, vendor-specific workflows 
- Chrome/firefox plugins that can detect a DPS site and facilitate DPS payments.
 

## Cases

List of cases that DPS should be able to handle

### Rough draft

1. User giving a donation, e.g. accept arbitrary payment from anonymous source.
2. User subscribing to an explicit tier/list/program that is offered by the vendor.
3. User paying for an explicit product.
4. Vendor accepts PayPal/Stripe/direct merchant account with local bank/cryptocurrency.
5. Vendor refunds PayPal/Stripe/direct merchant account with local bank/cryptocurrency.


Essential 1-3 can be summarized as payment comes in for thing X that should thus trigger series of things on vendors platform/site, e.g send product, add user to to priviliged list or exlusive memebers-only portion of website, etc.
 
For 4-5, the ideal design for DPS is to be flexible enough to handle the different processing steps required for a variety of payment processors whilst also having the smallest number of atomic functions (i.e. API) which can handle a wide variety of the common operations/payments, including those of a more complex nature.  The biggest trips up will likely arise from differences among push vs pull mechanics (i.e. crypto vs fiat), as well as bare-minimum requirements for e.g. processing a credit card on one platform vs another.


