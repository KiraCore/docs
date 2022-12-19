# [⏎](README.md#Roadmap) KIP_62
> Query & Manage Proposals

Crate a new tab where all active and expired proposals can be viewed and managed. Page should present a list of proposals ordered by the time remaining to finish or enact the proposal (otherwise expiration date for already finalized proposals). So that it is easy for the user to identify in the top of the list all proposals which are time sensitive.

_NOTE: Voting rules can be found [here](https://github.com/KiraCore/docs/blob/master/spec/iteration-1/kip_20.md#voting-rules)_


Table/list of proposals should include info such as proposal id, proposal type, countdown and status as % of votes required to pass or reach quorum (activate proposal). Furthermore a visual label should be present indicating that proposal is **INACTIVE** (didn't reached quorum), **ACCEPTED**, **REJECTED**, or is in the process of **ENACTING** after it already passed.

If proposal was submitted but was not started yet, then countdown (days,hours,minutes,seconds) should represent time remaining util voting period ends, if proposal passed then countdown should represent time until enactment. If proposal is enacted or already expired countdown can be replaced with a date-time of when the proposal was finalized, that is when it was enacted or rejected.

Every element on the proposal list must be expandable that is it must be possible to view all details of each proposal when element is clicked. If current account which is logged-in has a permission to vote on some specific (active) proposal it must be possible for it to easily submit a vote of a type supported by any given proposal (as each proposal might have different vote types such as YES/NO/ABSTAIN/etc).

If someone who voted on a given proposal it must be further visible for him - which vote type he casted. Note that it must be possible to change te vote as long as proposal didn't expired or is in the process of enacting.

In the main table/list whe should present in some visual (color) way which proposals a specific logged in user can take part in and position them all in the top of the list (unless they are expired). We should also make a simple filter allowing to display only active proposals. Proposals page or KIP_61 page should have further a query ability so that proposal with a given id can be easily found. Website should also accept a value in the URL so that proposals can be easily queried and shared e.g. `/proposal=123`.




