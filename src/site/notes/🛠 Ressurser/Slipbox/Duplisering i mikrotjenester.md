---
{"dg-publish":true,"permalink":"/ressurser/slipbox/duplisering-i-mikrotjenester/","dgHomeLink":true,"dgPassFrontmatter":false}
---

# Duplisering i mikrotjenester
Sam Newman har følgende å si om duplisering i mikrotjenester:

I had a few questions on this, including:

> Is it OK to share some code between multiple microservices? - Mariusz

And:

> What is the best practice for sharing object like 'customer' between bounded contexts: model copy-paste, shared lib or just correlation Id? - Piotr Łochowski

In general, I dislike code reuse across services, as it can easily become a source of coupling. Having a shared library for serialisation and de-serialisation of domain objects is a classic example of where the driver to code reuse can be a problem. What happens when you add a field to a domain entity? Do you have to ask all your clients to upgrade the version of the shared library they have? If you do, you loose independent deployability, the most important principle of microservices (IMHO).

Code duplication does have some obvious downsides. But I think those downsides are better than the downsides of using shared code that ends up coupling services. If using shared libraries, be careful to monitor their use, and if you are unsure on whether or not they are a good idea, I'd strongly suggest you lean towards code duplication between services instead.

[Kilde](https://samnewman.io/blog/2015/06/22/answering-questions-from-devoxx-on-microservices/)