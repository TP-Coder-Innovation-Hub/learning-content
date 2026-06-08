# What is an API

## What

An API (Application Programming Interface) is how software systems talk to each other. It defines what requests can be made, how to make them, and what responses to expect.

## The Waiter Analogy

Imagine a restaurant:

- You (the customer) want food
- The kitchen (the server) prepares food
- You don't walk into the kitchen and cook yourself
- The waiter (the API) takes your order to the kitchen and brings food back

You don't need to know how the kitchen works. You just need to know:
- What's on the menu (what you can request)
- How to order (the format of your request)
- What you'll get back (the response)

An API works the same way. It's a standardized menu of things one system can ask another system to do.

## A Real Example

When you check the weather on your phone:

1. Your weather app sends a request to a weather service API: "What's the weather in New York?"
2. The weather service looks up the data
3. The API sends back: "72°F, sunny"
4. Your app displays it nicely

Your app didn't measure the weather. It asked a service that did. The API is the agreed-upon way to ask and receive that information.

## Why APIs Matter

### For Business

- APIs let different software work together without custom integration each time
- They enable partnerships — your service can be used by other companies' apps
- They standardize communication — everyone uses the same "menu"
- They allow controlled access — you decide what others can and cannot do

### For Developers

- APIs let you build on top of existing services instead of reinventing everything
- You can use Google Maps in your app without building a mapping system
- You can process payments without building a banking system
- You can send emails without building a mail server

## Types of APIs

### Internal APIs

Your own systems talking to each other. The mobile app talks to your backend API.

### Partner APIs

You give specific partners access to your API. A hotel booking site gives airlines access to check room availability.

### Public APIs

Anyone can use them (usually with registration). Google Maps API, Stripe payment API, Twitter API.

## What Makes a Good API

- **Clear documentation** — Developers know what they can do and how
- **Consistent behavior** — Similar operations work similarly everywhere
- **Predictable errors** — When something goes wrong, the error tells you why
- **Versioned** — Changes don't break existing users
- **Fast enough** — Responses come back in a reasonable time

## Common Misconceptions

- "API" is not a product. It's an interface. It defines how to interact with a product.
- APIs are not just for tech companies. Banks, airlines, weather services, governments all have APIs.
- An API is a contract. Both sides agree on what the requests and responses look like. Changing the contract breaks things.
