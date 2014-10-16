---
layout: post
title: Integrate Real - Danger of Stubs and Fakes
---



As soon as you create a contract, treat dependencies like third-party suppliers. How would you operate if the API you were trying to use was provided by Google or Github?

Your order of preference (I like to think of it as a "reality scale") at EVERY point (local dev, pre-checkin, CI, test/staging deployments, live-preview, live) of integration should be:

1. Live Service (actual code, actual data)
1. Sandbox (actual code, fake data)
1. Full Executable Component
1. Provider-provided Stubs Fakes
1. Consumer-provided out-of-process stubs/fakes
1. Consumer-provided in-process stubs/fakes

Too often I see teams creating stubs to "protect themselves" from other teams ("working in silos" anybody?).

The point of integrating early is that integration IS HARD! Therefore using stubs to make it easier defies the point.

Reasonable reasons for lowering the "reality scale" integration point are:

- No access (permanently or temporarily/intermittently)
- Contractual/Licencing restrictions (e.g. rate limits)
- Inability to Control Data
- Speed of Execution

BUT... for each of the above:

1. Consider the Test Pyramid (Do these tests need to run at all? Are you using integration to test unit behaviour?)
1. Work with your providers to try and remove the limitations. It's to both of your advantage to Integrate early AND real.