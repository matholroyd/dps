# Direct Payment Standard

This standard (DPS) is intended to specify a common way for entities to advertise payment options as well as facilitate payments directly between 2 parties. 

The core purpose of DPS is to help shift the tech industry status quo, whem processing payments, from reliance on 3rd-party proprietary APIs to a standardize protocol that is more flexible and robust (e.g. Twitter API vs email SMTP).

Currently the stardard consists of these parts:

1. **DPS addresses**, which are resolved to a specific DPS server + username, which are used to specify who is to be paid.
1. **DNS records** which specify the DPS endpoint (and other details) for a given domain.
1. **DPS server protocol**, which can respond to queries such as listing payment options and facilitating processing a payment.

## DPS addresses

DPS address are written either like typical email addresses or an email address without the username portion. Examples are:

    bobsmith@example.com
    shop@bobsmith.com
    @bobsmith.com

## DNS layer

To allow DPS address to resolve to a predictable endpoint, a DNS records specifies where the DPS endpoint is for a given domain. The convention is as follows:

- Add a `TXT` DNS record for the domain you want to resolve with the format `'dps:endpoint url=<YOUR_DPS_SERVER_ROOT_URL>'`
- The URL must be a HTTPS URL, and thus a valid HTTPS certificate must exist.

For instance, if your domain is `example.com`, you might set DNS record to point to `https://example.com/dps` or `https://dps.example.com`, and thus your DNS record might look like:

    'dps:endpoint url=https://dps.example.com'
    'dps:endpoint url=https://www.example.com/dps'

Note:

- Non-root path URLs should be allowed, e.g. `/dps`, so that a DPS endpoint can be added to an existing website, and thus a domain owner can leverage an existing HTTPS certificate for a website they are already running.

*Additional DNS records...*

Probably will need additional records for cryptographic signing, to help reduce spam and authenticate a request did indeed come from a certain domain.


## DPS server protocol

*Need to flesh this out...*

Actions (suggestions for now):

1. `GET /serivces` => Return JSON list of payout options + other metadata about the server
1. `GET /XXX`  => Request an address for an upcoming push-payment, e.g. for a cryptocurrency. Returns a JSON string with:
    * The cryptocurrency address/PayPal email.
    * A timestamp of how long that address is valid for. 
    * A transaction ID that will be associated with the next push-transaction completed, and thus used to reference this upcoming transaction in the future.
 
 
  
- `<code>` will need to be from a standardized list that changes infrequently for existing items (if at all). This is somewhat a problem for things like cryptocurrencies that can be forked at will. Might want to incorporate something like a date into the names, as well as use commmon abbreviations used by exchanges, such as `btc` for mainline Bitcoin and `bch` for Bitcoin cash.