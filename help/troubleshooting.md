# Troubleshooting Errors

![](img-troubleshooting-2021-09-09-22-36-24.png)

Sometimes you may find yourself facing a problem that doesn't have a clear solution. These troubleshooting tips may help you solve problems you run into.

## **Issues on the Exchange**

### **INSUFFICIENT\_OUTPUT\_AMOUNT**

> The transaction cannot succeed due to error: AnpansRouter: INSUFFICIENT\_OUTPUT\_AMOUNT. This is probably an issue with one of the tokens you are swapping.
>
> the transaction cannot succeed due to error: execution reverted: anpansrouter: insufficient\_output\_amount.

You're trying to swap tokens, but your slippage tolerance is too low or liquidity is too low.

#### **Solution**
1. Refresh your page and try again later.
2. Try trading a smaller amount at one time.
3. Increase your slippage tolerance:
   1. Tap the settings icon on the liquidity page.
   2. Increase your slippage tolerance a little and try again. ![](img-troubleshooting1-2021-09-09-22-46-26.png)
4. Lastly, try inputting an amount with fewer decimal places.

#### **Reason**
**This usually happens when trading tokens with low liquidity.**

That means there isn't enough of one of the tokens you're trying to swap in the Liquidity Pool: it's probably a small-cap token that few people are trading.

However, there's also the chance that you're trying to trade a scam token which cannot be sold. In this case, AnpanSwap isn't able to block a token or return funds.

### **INSUFFICIENT\_A\_AMOUNT or INSUFFICIENT\_B\_AMOUNT**

> Fail with error 'AnpansRouter: INSUFFICIENT\_A\_AMOUNT'  
> or  
> Fail with error 'AnpansRouter: INSUFFICIENT\_B\_AMOUNT'

You're trying to add/remove liquidity from a liquidity pool \(LP\), but there isn't enough of one of the two tokens in the pair.

#### **Solution**
**Refresh your page and try again, or try again later.**

Still doesn't work?

1. Tap the settings icon on the liquidity page.
2. Increase your slippage tolerance a little and try again.

![](img-troubleshooting1-2021-09-09-22-46-26.png)

#### **Reason**
The error is caused by trying to add or remove liquidity for a liquidity pool \(LP\) with an insufficient amount of token A or token B \(one of the tokens in the pair\).

It might be the case that prices are updating too fast when and your slippage tolerance is too low.

![](img-troubleshooting2-2021-09-09-23-00-01.png)

![](img-troubleshooting3-2021-09-09-22-58-19.png)

#### **Solution for nerds**
OK, so you're really determined to fix this. We really don't recommend doing this unless you know what you're doing.

There currently isn't a simple way to solve this issue from the AnpanSwap website: you'll need to interact with the contract directly. You can add liquidity directly via the Router contract, while setting amountAMin to a small amount, then withdrawing all liquidity.

### **Approve the LP contract**

Head to the contract of the LP token you're trying to approve.  
For example, here's the ETH/WBNB pair: [https://bscscan.com/address/0xed0ed1579c8698474d45cb701e74ea212fbf4ee6](https://bscscan.com/address/0xed0ed1579c8698474d45cb701e74ea212fbf4ee6#code)

1. Select **Write Contract**, then **Connect to Web3** and connect your wallet.

![](img-troubleshooting4-2021-09-09-23-10-49.png)

2. In **section "1. approve",** approve the LP token for the router by entering
   1. spender \(address\): enter the contract address of the LP token you're trying to interact with
   2. value \(uint256\): -1

### Query "balanceOf"

1. Switch to **Read Contract.**
2. In **5. balanceOf**, input your wallet address and hit **Query**.
3. Keep track of the number that's exported. It shows your balance within the LP in the uint256 format, which you'll need in the next step.

![](img-troubleshooting5-2021-09-09-23-17-23.png)

### Add or Remove Liquidity

Head to the router contract: [https://bscscan.com/address/0xe8d2c4ca811e0445b83326abe85b7c40c66759ed\#writeContract](https://bscscan.com/address/0xe8d2c4ca811e0445b83326abe85b7c40c66759ed#writeContract)

1. Select **Write Contract** and **Connect to Web3** as above.
2. Find **addLiquidity** or **removeLiquidity** \(whichever one you're trying to do\)
3. Enter the token addresses of both of the tokens in the LP.
4. In **liquidity \(uint256\),** enter the uint256 number which you got from "balanceOf" above.
5. Set a low **amountAMin** or **amountBMin**: try 1 for both.
6. Add your wallet address in **to \(address\)**.
7. Deadline must be an epoch time greater than the time the tx is executed.

![](img-troubleshooting6-2021-09-09-23-25-42.png)

**Warning:** This can cause very high slippage, and can cause the user to lose some funds if frontrun

### **AnpansRouter: EXPIRED**

> The transaction cannot succeed due to error: AnpansRouter: EXPIRED. This is probably an issue with one of the tokens you are swapping.

Try again, but confirm \(sign and broadcast\) the transaction as soon as you generate it.

This happened because you started making a transaction, but you didn't sign and broadcast it until it was past the deadline. That means you didn't hit "Confirm" quickly enough.

### **Anpans: K**

> The transaction cannot succeed due to error: Anpans: K. This is probably an issue with one of the tokens you are swapping.

Refresh the page and try again, or increase slippage tolerance via the settings icon and try again.

This probably happened because you're trying to buy or sell tokens during a big price movement. The frontend is getting outdated information \(e.g. outAmount\) from the smart contract, causing the swap to fail.

### **Anpans: TRANSFER\_FAILED**

> The transaction cannot succeed due to error: execution reverted: Anpans: TRANSFER\_FAILED.

Make sure you have 30% more tokens in your wallet than you intend to trade, or try to trade a lower amount. If you want to sell the maximum possible, try 70% or 69% instead of 100%.  
Caused by the design of Restorative Rebase tokens like tDoge or tBTC.  
[Understand how restorative rebase tokens work](https://btcst.medium.com/stp-8-restorative-rebase-b4fbbdfd96c).

### **Transaction cannot succeed**

Try trading a smaller amount, or increase slippage tolerance via the settings icon and try again. This is caused by low liquidity.

### **Price Impact too High**

Try trading a smaller amount, or increase slippage tolerance via the settings icon and try again. This is caused by low liquidity.

### **estimateGas failed**

> This transaction would fail. Please contact support

#### **Solution**
**Contact the project team of the token you're trying to swap.**

This issue must be resolved by the project team.

#### **Reason**
**This issue is caused by tokens which have hard-coded the AnpanSwap router into their contract.**

While this practice is ill-advised at best, the reason for these projects having done this appears to be due to their tokenomics, in which each purchase sends a % of the token to LPs.

The projects affected will likely not work with the router: they will most likely need to create new versions of their tokens pointing to our router address, and migrate any existing token holders to their new token.

We recommend that any projects which created such tokens should also make efforts to prevent their users from adding them to LP.

The router address is [https://bscscan.com/address/0xe8d2c4ca811e0445b83326abe85b7c40c66759ed](https://bscscan.com/address/0xe8d2c4ca811e0445b83326abe85b7c40c66759ed)

### **Cannot read property 'toHexString' of undefined**

> "Unknown error: "Cannot read property 'toHexString' of undefined"

When trying to swap tokens, the transaction fails and this error message is displayed. This error has been reported on mobile devices using Trust Wallet.

#### **Solution**
1. Attempt the transaction again with increased slippage allowance.
2. If 1. does not resolve your problem, consider using another wallet such as SafePal for your transaction.

#### **Reason**
**This usually happens when trading tokens with insufficient slippage allowance on Trust Wallet.**

The exact details of the problem are still being investigated.

### **Execution reverted: TransferHelper: TRANSFER\_FROM\_FAILED.**

> The transaction cannot succeed due to error: execution reverted: TransferHelper: TRANSFER\_FROM\_FAILED.

When trying to swap tokens, the transaction fails and this error message is displayed. This error has been reported across platforms.

#### **Solution**
1. Check to make sure you have sufficient funds available.
2. Ensure you have given the contract allowance to spend the amount of funds you're attempting to trade with.

#### **Reason**
This error happens when trading tokens with insufficient allowance, or when a wallet has insufficient funds.  
If you're trading tokens with Restorative Rebase like assets tDoge or tBTC, make sure you understand how they work first with this [guide to Rebase tokens](https://btcst.medium.com/stp-8-restorative-rebase-b4fbbdfd96c).

## **Issues with Honey Pools**

### **BEP20: burn amount exceeds balance**

> Fail with error 'BEP20: burn amount exceeds balance'

You don't have enough HONEY in your wallet to unstake from the ANPAN-ANPAN pool.

#### **Solution 1**
**Get at least as much HONEY as the amount of ANPAN that you’re trying to unstake.**

1. Buy HONEY on the exchange. If you want to unstake 100 ANPAN, you need at least 100 HONEY.
2. Try unstaking again.

#### **Solution 2**
If that still fails, you can perform an “emergencyWithdraw” from the contract directly to unstake your staked tokens.

1. Go to: [https://bscscan.com/address/0x305E193C7B6955564D0D3D35d918FF1f27F3A610\#writeContract ](https://bscscan.com/address/0x305E193C7B6955564D0D3D35d918FF1f27F3A610#writeContract)
2. Click **“Connect to Web3”** and connect your wallet.

![](img-troubleshooting4-2021-09-09-23-10-49.png)

3. In section **“4. emergencyWithdraw”**, enter "0" and click “Write”.

This will unstake your staked tokens and lose any uncollected ANPAN yield.

**Warning:**
**This will lose any yield that you haven’t harvested yet.**

#### **Reason**
To stop this happening again, **don’t sell your HONEY.** You still need it to unstake from the “Stake ANPAN Earn ANPAN” pool.

This error has happened because you have sold or transferred HONEY tokens. HONEY is minted in a 1:1 ratio to ANPAN when you stake in the ANPAN-ANPAN Honey Pool. HONEY must be burned at a 1:1 ratio to ANPAN when calling leaveStaking \(unstaking your ANPAN from the pool\), so if you don't have enough, you can't unstake from the pool.

![](img-troubleshooting7-2021-09-10-00-52-25.png)

### **Out of Gas error**

> Warning! Error encountered during contract execution \[out of gas\]

You have set a low gas limit when trying to make a transaction.

#### **Solution**
Try manually increasing the **gas limit** \(not gas price!\) in your wallet before signing the transaction.

A limit of 200000 is usually enough.

![](img-troubleshooting8-2021-09-10-00-54-14.png)

The above example is from Metamask; check your wallet's documentation if you aren't sure how to adjust the gas limit.

#### **Reason**
Basically, your wallet \(Metamask, Trust Wallet, etc.\) can't finish what it's trying to do.

Your wallet estimates that the gas limit is too low, so the function call runs out of gas before the function call is finished.

### **BEP20: transfer amount exceeds allowance**

> Fail with error 'BEP20: transfer amount exceeds allowance'

#### **Solution**
1. Use Unrekt.net to revoke approval for the smart contract you're trying to interact with
2. Approve the contract again, without setting a limit on spend allowance
3. Try interacting with the contract again.

#### **Reason**
This happens when you set a limit on your spend allowance when you first approved the contract, then try to swap more than the limit.

### **BEP20: transfer amount exceeds balance**

> Fail with error 'BEP20: transfer amount exceeds balance'

You're probably trying to unstake from a Honey Pool with low rewards in it. Solution below.

If not, you may be trying to send tokens that you don't have in your wallet \(for example, trying to send a token that is already assigned to a pending transaction\). In this case, just make sure you have the tokens you're trying to use.

#### **Solution**
Firstly,[ let the team know](https://docs.anpanswap.finance/#/contact-us/telegram) which pool you're trying to unstake from, so they can top up the rewards. If you're in a hurry to unstake and you don't mind losing your pending yield, try an emergencyWithdraw:

You can perform an “emergencyWithdraw” from the contract directly to unstake your staked tokens.

1. Find the contract address of the Honey Pool you're trying to unstake from. You can find it in your wallet's transaction log.
2. Go to [https://bscscan.com/](https://bscscan.com/address/0x305E193C7B6955564D0D3D35d918FF1f27F3A610#writeContract) and in the search bar, enter the contract address.
3. Select **Write Contract.**
4. Click **“Connect to Web3”** and connect your wallet.!

![](img-troubleshooting4-2021-09-09-23-10-49.png)

5. In section **“4. emergencyWithdraw”,** enter "0" and click “Write”.

This will unstake your staked tokens and lose any uncollected yield.

**Warning:**
**This will lose any yield that you haven’t harvested yet.**

#### **Reason**
This error tends to appear when you're trying to unstake from an old Honey Pool, but there aren't enough rewards in the pool left for you to harvest when withdrawing. This causes the transaction to fail.

## **Other issues**

### **Provider Error**

> Provider Error  
> No provider was found

This happens when you try to connect via a browser extension like MetaMask or Binance Chain Wallet, but you haven’t installed the extension.

#### **Solution**
Install the official browser extension to connect, or read our guide on [how to connect a wallet to AnpanSwap](https://docs.anpanswap.finance/#/get-started/connection-guide).

### **Unsupported Chain ID**

Switch your chain to Binance Smart Chain. Check your wallet's documentation for a guide if you need help.

### **Issues buying SAFEMOON and similar tokens**

To trade SAFEMOON, you must click on the settings icon and **set your slippage tolerance to 12% or more.**  
This is because **SafeMoon taxes a 10% fee on each transaction**:

* 5% fee = redistributed to all existing holders
* 5% fee = used to add liquidity

This is also why you might not receive as much of the token as you expect when you purchase.  

### **Internal JSON-RPC errors**

> "MetaMask - RPC Error: Internal JSON-RPC error. estimateGas failed removeLiquidityETHWithPermitSupportingFeeOnTransferTokens estimateGas failed removeLiquidityETHWithPermit "

Happens when trying to remove liquidity on some tokens via Metamask. Root cause is still unknown. Try using an alternative wallet.

> Internal JSON-RPC error. { "code": -32000, "message": "insufficient funds for transfer" } - Please try again.

You don't have enough BNB to pay for the transaction fees. You need more BEP-20 network BNB in your wallet.

### **Error: \[ethjs-query\]**

> Error: \[ethjs-query\] while formatting outputs from RPC '{"value":{"code":-32603,"data":{"code":-32000,"message":"transaction underpriced"}}}'

Increase the gas limit for the transaction in your wallet. Check your wallet's documentation to learn how to increase gas limit.

> Swap failed: Error: \[ethjs-query\] while formatting outputs from RPC '{"value":{"code":-32603,"data":{"code":-32603,"message":"handle request error"}}}'

Cause unclear. Try these steps before trying again:

1. Increase gas limit
2. Increase slippage
3. Clear cache

