# Blockchain Security Checklist

[] Understanding the project

##### CENTRALIZATION / PRIVILEGE

[] Secure the function have access by owner
[] Initial token distribution, should prefix receiver address
[] Use multisign wallet for dev
[] Unrestricted privilege function

##### EVENT LOG

[] Event for any set function
[] Event for significant transactions
[] Event name must clear and avoid misunderstand
[] Favor capitalization and a prefix in front of events (we suggest Log)

##### VOLATILE CODE

[] Avoid Reentrancy
[] Check the return value of external call such as transfer
[] Avoid state change after call external function
[] Math Operation
[] Check insecure arithmetic, integer under/overflow
[] Validity check
[] Clear Code structure
[] Don't use transfer() or send()
[] Handle errors in external call ( can use try/catch)
[] Favor pull over push for external calls
[] Don't delegatecall to untrusted code
[] Force-feeding Ether

##### GAS OPTIMIZATION

[] Don't use recursive function
[] Use appropriate type (uint8, map...)
[] Initial variable in constructor
[] Use external function instead of public

##### UNIT TEST

[] Unit test for get/set function
[] Unit test for overflow data
[] Unit test for external call
[] Run unit test before deploy

##### CODING STYLE

[] Language specific
[] Store config of upgrade contract and push to git
[] Check Reusable Code, use modifier
[] Check for minimal source code
[] Have note/status for deployed code

##### LOGICAL ISSUE

[] Check over minted token
[] Don't trust tx.origin for authorization, use msg.sender for authorization.
[] Check timestamp for logic will be manipulated by a miner
[] Avoid using block.number as a timestamp


##### POTENTIAL ATTACK
[] Check for Potential Sandwich Attack