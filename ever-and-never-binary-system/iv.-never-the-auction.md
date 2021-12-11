# IV. Never The Auction

> Let him look to his bond.
>
> &#x20; — William Shakespeare

When the price is determined (it is being determined instantly) the auctions can be performed. We assume two kinds of auction: direct (NEVER/EVER), reverse (EVER/NEVER) and their decentralized variants which we note as dAuctions (analogous to dePool).

The main arguing and counter arguments for using the Schelling schemes based on the idea of collusion attacks. That can be a case if the quote makers are biased in their incentives to make correct quotes or live in an asymmetrically conjoined world (the latter is not a case as all procedures on the blockchain are symmetrically open for all users). And that is why we propose the auction mechanism to verify the quotations made on the previous step. To re-verify, to be more precise.

We propose the following scheme:

1. Auction is  performed on demand with a minimum lot size. It sells as many Neversas demanded by the winners but filtering participants by the required amount (one cannot participate if bids are valued smaller than given).
2. It is designed as Vickrey auction (sealed-bid, second price).
3. The bid sealing is also performed by the commit-reveal scheme similar to the one mentioned above.
4. It has the predefined zero position based on quoting result, so that the winner must submit the bid higher (or same) than quoting price.
5. The quoting price should not be disclosed before the auction starts.
6. The winner address, winning and paid (second) price are disclosed at the end of auction.
7. If no one wins, the auction is considered as failed and the lot is not sold.
8. Auction is paid, and the payment is to go to the validators, and as a payment for larger liquidity (because of minimum lot value).
9. After the auction every participant can buy the desired amount of Nevers by the price determined at the last auction increased by some factor.&#x20;

We assume that validators are unbiased in their opinion unless they could conspire for some non market reward (bribe). If some potential auction participant bribes the validators at the quoting phase to make the EVER/USD price higher to buy NEVER cheaper and she wins, then they will indirectly decrease the EVER collateral backing NEVER which harms the system which they validate.&#x20;

In contrast — if they for some reason conspire to agree on a lower Ever/USD price, which makes the Never/EVER price higher, there will be no winners in auction, which will locally stop the economic process. So in general the validators have incentives to keep the consensus price close to real market value to:

1. Establish the correct backing of the NEVERs
2. To let NEVER issuing be performed in proper way

#### D’Auction

D’Action contract is designed to be the mechanism to allow users with limited amounts of buying power to take part in the auction using the accumulated potential. They can organize the group of players to represent the single auction participant accumulating their demands to reach the restrictions for making bids. That is made in a very analogous way dePools are designed.

The contract contains the following roles:

1. Aggregator (representor). Account which owns the dAuction contract. Its obligations include making a proper bid (which should be not less than a certain percentage of cumulative buying demand), making a bid in a main auction at proper time.
2. Participant. Account which gives the contract rights to bid from her name acting together with aggregator
3. Contract itself. Accumulates the bids in a proper way, allows the representative to make a price bid, sends the bid to the auction contract, pays all correct fees, collects the results and distributes the won lot between them all and returns or rebids the original bids if the contract loses the auction.
4. The aggregator and participants buy price is finally adjusted by their amounts and roles (aggregator has some additional benefits as a reward for being a representor)

All the D’Auction constants are subject to be determined at implementation phase and should establish adequate incentives for all players.

dAuction can be performed once or at an instant (until win) way. If some participant exists the dAuction decreasing total buy potential less than minimum lot size, D’Auction needs to find new participants to fit the requirements. If D’Auction wins it distributes the bought lot and closes.

The D’Auction participants in any way should have more benefits than any user in the after auction phase to incentivize participants to enter it.&#x20;

As the D’Auction representative should not be a validator no punishment mechanism is currently proposed as well as no reputation is recorded as we suggest that the new auction allows participants to reorganize in a new D’Auction.&#x20;

The list of currently available open D’Auction should be also available with all important parameters transparently given to let newcomers easily choose and participate based on their internal preferences.

The duration of D’Auction non-winning lifetime can also be specified in advance, after which D’Auction is closed and returns the collected bids back independently on status.
