This is an excerpt from PR I wrote. It attempts to quickly describe my testing philosophy.

## Post

I would also like to continue the conversation about our testing philosophy as a company. I've written down my thoughts below if you're interested, but don't feel obligated to read this.

I tend to break tests into two categories: 

(1) Unit - These thoroughly test our atomic pieces of software (e.g., methods, classes). I accomplish this through highly-focused tests that:
&nbsp;&nbsp;(a) Capture all of the important branching
&nbsp;&nbsp;(b) Capture all the IO for the method/class/whatever
&nbsp;&nbsp;(c) Ignore as much application state as possible.
(2) Integration - These test that we get reasonable responses when our atomic pieces talk to each other. These tests
&nbsp;&nbsp;(a) Ignore most branching
&nbsp;&nbsp;(b) Ignore most of the IO details
&nbsp;&nbsp;(c) Ignore as much application state as possible (which is inevitably a lot more than with unit tests)

I like to think about the integration-unit distinction in terms of coverage. When I'm writing unit tests, I'm striving for 100% coverage one small piece a time. When I'm writing integration tests, I like to pretend that I already have 100% coverage. This approach helps me write focused tests -- which are small, easy to read, and robust (i.e., only fails when the thing it's testing breaks).

This above philosophy doesn't include e2e tests as a separate thing from integration tests. This is because of ignorance, not by design.

Tying this all back to this specific PR. I implemented the `Seeder` classes with hopes to accomplish 2a. Right now we're using it in the e2e seed file which runs before each e2e test. In the future, I think it would be better that each e2e test reseeds the db by directly invoking the `Seeder`s. This way the the tests will be very explicit about what state it needs, only adding what it needs. I hope the `Seeder` is elegant enough that this setup will only be 10 or so lines per test.

I also made the e2e test more robust by only testing a few representative values in the json response. This way if we decide to slightly change what we return (which is a-whole-nother conversation on API versioning), the test won't break. It'll only break if the different APIs and dbs stop talking to each other properly. I'm not 100% sure that this is the right choice. This is where not having a coherent philosophy on integration vs e2e tests hurts me.

Anyway, thanks for coming to my TedTalk.
