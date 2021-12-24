# III. Never The Elector

> “The observed outcome may be one that everybody prefers, it may be one that nobody prefers, or it may be one that some prefer and others deplore.”
>
> &#x20;          ― Thomas C. Schelling

Never the Elector contract is designed to collect the external Ever/USD pricing value and is permanently running and therefore collects the data in an instant way. But the collection is made in an encrypted way. The revealing mechanism is not performed on each data collection.

Quotation is made based on a decentralized and blind (sealed bidded) scheme by Never the Validators. This ensures no one of the validators is aware of the quotes made by others in the commit-reveal scheme as discussed below. More sophisticated ZKP algorithms could also be  implemented to raise the non-disclosure properties.

At a computationally unpredicted (cryptographically random) moment the check of then given prices is performed. That is, all the data is to be revealed by submitters, and then checked with the given hash (commit) and analyzed. The correct in just mentioned sense values are considered as the initial price set. Those participants who cannot correctly reveal the data are considered as pre-malicious.

Those participants whose values lie between 25th and 75th quartile are considered as honest and are rewarded. Those participants who feed the outlying data are considered as pre-malicious with some cumulative rank and they lose the current reward which goes to the honest participants with respect to this rank.&#x20;

So let rank _r_ be a number in _\[0,1]_ real valued interval. We set that _r = 0_ corresponds to the honest participant and _r = 1_ to a malicious one. Initially all participants have _rank = 0_. With the given rank the reward is calculated as $$(r-0.5)*r_0$$ where $$r_0$$is the reward for an honest participant.

The rank is updated as follows: new rank is $$r’ = r*(1-a) + a*r_c$$, where _r_ is old rank, _a_ - some constant in _\[0,1]_ and $$r_c$$- the rank of the currently analyzed feed. $$r_c$$ is calculated based on the difference between median value which is assumed as consensus value (see also explanations below) and given by the current participant.&#x20;

$$
\begin{cases}
    r_c = 0, \text  {for } v_{25} < v < v_{75} ; \\
    r_c = 1-P (v_i < v), \text  {for } v ≤ v_{25} ; \\
    r_c = 1-P (v_i > v), \text {for } v ≥ v_{75} 
  \end{cases}
$$

where P (v ⋳ V)  is the empirical probability that some random vi from the current price set lies in the interval V. So $$1 - P (v_i > v) \to 1$$ with v -> “_max given number_” and the same way $$1 - P (v_i < v) \to 1$$ with v ⟶ “min given number”. $$v_{25}$$ and $$v_{75}$$ correspond to the edges of the honesty interval defined above. The iterative relation $$r’ = r*(1-a) + a*r_c$$ accumulate “errorness” of the feed with some rate a. The rate can be understood if we suggest  the constant values i.e.

$$
\begin{cases}
    r_0 = 0 \\
    r_{n+1} = r_n*(1-a) + r_c*a \\
    r_n \to r_c \text { if } n \to \infty
  \end{cases}
$$

Besides this, the validator is banned forever (excluded from the validators’ set) if it has undecreased _rank > 0.5_ during a certain number of checks. Note that if the validator would give the correct answers its rank will be instantly decreasing as _a < 1_ and therefore before it gets the total ban it could potentially clear the cumulative malicious rank.

Refusing to feed the price value is considered the same as feeding data with some constant rank which is a subject to determine based on convergence experiments on the pre-implementation (PoC) phase.

We realize that the quotation can be performed in an automatic or semiautomatic way, e.g. by feeding the contract with the real deal prices from any exchange they trust. That in particular means that they cannot verify each feeded value on each step and therefore permanently punish them for “incorrect” price would be unfair. And that is why we give them the pre-malicious status as a kind of warning to check their systems. On the warning they can stop all feeding processes and fix them.&#x20;

In the case when Schelling mechanism cannot provide single price (e.g. multimodal distribution) or when the normality check fails the quoting contract can

* provide a series of values (activating a correspondent number of auctions);
* refuse to provide any value, adjusted all participants with a certain rank (is a subject to investigate during the PoC);
* in the case of multiple values, the winner auction ranks the losing modes with a certain rank of suspiciousness.

Incentivization to make a quote should take the following aspects into account:

1. Quote makers are to be incentivized to make a quote - that is to feed the protocol with some information (the price value in our case).
2. They are to be incentivized to feed a correct quote - that is the given price value should be close to real market price as much as possible.

We assume that quote makers are incentivized to do their job by the following reasons:

1. To stabilize network common consensus
2. To make the NEVER issuing justified and reasonable
3. To bring the correct economic characteristics in the system
4. Not to lose the validators reward
5. Not to lose their stake at all
6. To gain rewards from stakes made with EVERs from Never the Auction
7. To gain extra reward when suspicious participants slashed

To perform consensus on the given data the Schelling point mechanism is proposed. It had been originally introduced in 1960 by Thomas Schelling in his book “The Strategy of Conflict” and basically is used to represent the method to acquire the common knowledge which is based on unbiased human behavior in an informationally symmetric world (which we assume as the working case).

We use the same notions and ideas which originally come from Vitalik Buterin and roughly repeat the main steps of the quoting protocol (based on commit-reveal scheme):

1. During each block, all participants can submit a salted hash of the EVER/USD price together with their EVER address (commit).
2. During the quoting phase (as well as the check phase, see above), users can submit the value and salt whose hash they provided at the previous stage (reveal).
3. Define the “correctly submitted values” as all values N where Hash (N+ADDR+Salt) was submitted in the first block and N was submitted in the second block, both messages were signed/sent by the account with address ADDR and ADDR is one of the allowed participants in the system.
4. Sort the correctly submitted values.
5. Every user who submitted a correct value between the 25th and 75th percentile gains a reward of a certain amount of EVER which is collected from the suspicious participants.
6. The quoting phase makes the same procedure for suspicious and malicious accounts as at the check phase.

As mentioned in the same paper, “the protocol does not include a specific mechanism for preventing sybil attacks; it is assumed that proof of work, proof of stake or some other similar solution will be used” (Proof Of Stake in the Everscale case).

The current final price estimation is basically calculated as the median value of the correct dataset. It is later used in auction contracts (the consensus value is a subject to adjust if anomalies have been observed in distribution shapes and parameters).
