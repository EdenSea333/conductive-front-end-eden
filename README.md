# Conductive.ai Frontend Engineer Test: Sankey Chart Visualization

Hi there! 👋

Thanks for you interest in the Senior Frontend Engineer role at [Conductive.ai](https://www.conductive.ai) 

This test helps us understand your skill level as a front-end developer. From our experience, technical interviews can be a challenge since a lot of the questions are not necessarily related to the day-to-day work.

It makes sense for you demonstrate to us your skill with a practical real-world example producing code we would actually use in production.

_**Please fork this repository and check in your code, please deploy to Heroku/Netlify/etc. and send us a link to your REPO and live site**_


# Objective

---

This test will evaluate your skills at displaying information and familiarity with **data visualization**. 

We will evaluate your ability in:

- Understanding a domain-specific data set
- Creating optimized parsing functions
- Integration with visualization libraries like D3js.
- This task should take no more than 2 hours

# Overview

---

The objective of this specific test is to visualize the flow of a particular fungible token in the first 2282 transfers using a [SanKey diagram](https://datavizcatalogue.com/methods/sankey_diagram.html).

It is not entirely relevant to know this for the purposes of this test, but it helps to know that tokens (ERC-20 tokens) on a blockchain are actually represented by a smart contract which has an address, just like a regular wallet.

The data about a token is stored inside the contract, and it keeps an internal ledger of who owns what. If you decide to transfer tokens, you invoke a smart contract’s function to transfer tokens you hold to another recipient.

# Data source

---

You will be provided with a CSV file named `quidd-bsc-transfers-0x7961Ade0a767c0E5B67Dd1a1F78ba44F727642Ed.csv` available in this repo. This CSV file contains the first 7,695 transactions from the token smart contract. This CSV file comes from the blockexplorer BSCScan which you can see below.

### Transactions

On Bscscan you can see a list of all the historical transaction of the token this test is based on [QUIDD](https://bscscan.com/token/0x7961ade0a767c0e5b67dd1a1f78ba44f727642ed).

You can see these transactions online here: [https://bscscan.com/token/0x7961ade0a767c0e5b67dd1a1f78ba44f727642ed](https://bscscan.com/token/0x7961ade0a767c0e5b67dd1a1f78ba44f727642ed)

You can see an example below:  

![Untitled](https://github.com/conductiveai/conductive-frontend-test/blob/main/img/1.png?raw=true)

### Columns

The file contains a few columns, the columns that will be relevant for this test are:

- **`UnixTimestamp`** - This is the unix timestamp of the transaction
- **`DateTime`** - This is the human readable date time of when the transaction occured
- **`From`** - This indicates the originator of the transaction and in a transfer, who the tokens will be coming from
- **`To`** - This indicates the recipient of the transaction, and in a transfer, who the tokens will be going to
- **`Quantity`** - This is the amount of tokens being transferred
- **`Method`** - This indicates the method or action you would like to take on a specific Token Smart Contract like “Transfer”.
    
    
    ![Screen Shot 2022-01-17 at 11.55.53 AM.png](https://github.com/conductiveai/conductive-frontend-test/blob/main/img/2.png?raw=true)
    

### **Assumptions**

- Tokens can be transferred in any direction between users and smart contracts. Some of these smart contracts belong to [DEX (Decentralized Exchange)](https://www.coinbase.com/learn/crypto-basics/what-is-a-dex) smart contracts like [Pancakeswap](https://pancakeswap.finance/).
- We can assume the **`Fund`** method is a user wallet sending tokens to a smart contract and is done by the owner
- In order to interact with a token (like transfer), a user must invoke a smart contract method

### QUIDD’s launch

The QUIDD token had its [first offering on Polkastarter](https://polkastarter.com/projects/quidd), a launchpad with the first public offering of the token. 2,000,000 QUIDD tokens were transferred from the owner of the token to fund a smart contract which distributes the QUIDD tokens to Polkastarter users.

🤖 **Polkastarter Smart Contract** [https://bscscan.com/address/0xee62650fa45ac0deb1b24ec19f983a8f85b727ab](https://bscscan.com/address/0xee62650fa45ac0deb1b24ec19f983a8f85b727ab)

🔃 **Transaction hash for the transfer**

[https://bscscan.com/token/0x7961ade0a767c0e5b67dd1a1f78ba44f727642ed?a=0xee62650fa45ac0deb1b24ec19f983a8f85b727ab](https://bscscan.com/token/0x7961ade0a767c0e5b67dd1a1f78ba44f727642ed?a=0xee62650fa45ac0deb1b24ec19f983a8f85b727ab)

![Untitled](https://github.com/conductiveai/conductive-frontend-test/blob/main/img/3.png?raw=true)

# Problem

---

Your task is to visualize the transactions in the CSV `quidd-bsc-transfers-0x7961Ade0a767c0E5B67Dd1a1F78ba44F727642Ed.csv` and specifically point out which transactions came from the Polkastarter smart contract (`0xee62650fa45ac0deb1b24ec19f983a8f85b727ab`) in a Sankey diagram with the following requirements:

- Create a node showing the token mint. A mint even is designated when the `From` address is `0x0000000000000000000000000000000000000000` and the resulting tokens go to the owners wallet, in this case `0x72571d815dd31fbde52be0b9d7ffc8344aede616`
- Create a node for the Polkastarter smart contract which is `0xee62650fa45ac0deb1b24ec19f983a8f85b727ab`
- Create nodes for locations where the tokens can be transferred to
    - Represent tokens that were transferred to a decentralized exchange (DEX) such as PancakeSwap with a red node. The PankcakeSwap address is `0xd6d206f59cc5a3bfa4cc10bc8ba140ac37ad1c89`.
    - Represent tokens that were transferred to another wallet, but not a DEX with a yellow node labeled “Jump 1”. This can be labeled with any method under the `Method` column.
    - Represent tokens that were not transferred with a green node.
- Create links between the nodes to show the flow of tokens from one node to the next. The link between nodes should be visually thicker when more tokens are transferred.
- Display the amount of QUIDD tokens transferred between nodes on a mouseover of a link in the diagram.
- Display the value of QUIDD tokens in USD in each node when on a mouseover of a node in the diagram.
- Repeat this process for tokens transferred to another wallet (yellow node) to visualize their flow to a proceeding wallet, decentralized exchange, or if the tokens were not transferred further.
- Display a summary of metrics to explain what is being displayed
    - **Total Value QUIDD** - This displays the total lifetime sum of transfer value in QUIDD tokens
    - **Total number of transactions** - This displays the total number of transactions being displayed.

# Solution

---

### Diagram

Below is an example of what we expect the visualization to look like. As mentioned, it should display:

1. Nodes representing the Polkastarter smart contract (blue), PancakeSwap (red), a second wallet (yellow), a third wallet (purple) and no further transfers (green).
2. The number of transfers to each node represented by link size.
3. Summary panel displaying statistics of the visualization

![Untitled](https://github.com/conductiveai/conductive-frontend-test/blob/main/img/4.png?raw=true)

### Solution Technical Requirements

- The visualization component can be done in React, Vue or vanilla JS.
- You may use any visualization library, but we highly recommend using d3 ([https://d3js.org/](https://d3js.org/)) which has out-of-the-box code for rendering Sankey diagrams. Please see below
- Final submission should be done via a fork of this repository and build instructions provided in the file `build.md`

# Sankey Examples

---

- https://github.com/d3/d3-sankey
- https://github.com/tamc/sankey

[Sankey Diagram](https://observablehq.com/@d3/sankey)

- https://github.com/nickbalestra/sankey
    - [https://nick.balestrafoster.com/sankey/](https://nick.balestrafoster.com/sankey/)
- https://github.com/csaladenes/sankey
    - [https://sankey.csaladen.es/](https://sankey.csaladen.es/)
- https://github.com/nowthis/sankeymatic
    - [https://sankeymatic.com/build/](https://sankeymatic.com/build/)
- https://github.com/ricklupton/d3-sankey-diagram
    - [https://ricklupton.github.io/d3-sankey-diagram/](https://ricklupton.github.io/d3-sankey-diagram/)

# Reaching Out

---

Feel free to reach out with technical and non-technical questions. Our goal is to help you do your best with this test, so honest communication is key.
